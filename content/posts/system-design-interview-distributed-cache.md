---
title: "System Design Interview - Distributed Cache"
date: '2021-02-03T09:21:45+00:00'
tags:
  - "system design"
  - "distributed systems"
  - "caching"
  - "scalability"
  - "architecture"
  - "interview prep"
categories:
  - "System Design Interview"
  - "Technical Article"
summary: "A comprehensive, in-depth guide to designing a distributed caching system. We explore core concepts from data partitioning with consistent hashing to advanced topics like consistency models, fault tolerance, and handling real-world challenges like thundering herds."
---

## 1. Introduction: The Need for Speed and Scale

In any large-scale application, performance is paramount. As user load increases, backend services, particularly databases, often become the primary bottleneck. Repeatedly fetching the same data from a disk-based database is inefficient, leading to high latency for users and immense strain on database resources.

**Caching** is the foundational strategy to mitigate this. By storing frequently accessed data in a faster, in-memory data store, we can serve user requests in a fraction of the time.

However, a single cache server has its limits:
*   **Limited Memory:** It can only store a finite amount of data.
*   **Single Point of Failure (SPOF):** If the cache server goes down, the entire application's performance degrades as all requests flood the database.
*   **Limited Throughput:** A single server can only handle a certain number of requests per second.

To overcome these limitations, we evolve to a **Distributed Cache**: a collection of interconnected cache servers (nodes) that work together as a single, cohesive unit. This guide provides a deep dive into the components and trade-offs involved in designing such a system, tailored for a system design interview context.

## 2. Laying the Foundation: Requirements and Goals

A good design starts with clear requirements.

**Functional Requirements:**
*   `Set(key, value, ttl)`: Store a key-value pair with an optional Time-To-Live (TTL).
*   `Get(key)`: Retrieve the value associated with a key.
*   `Delete(key)`: Invalidate/remove a key-value pair.

**Non-Functional Requirements:**
*   **Low Latency:** Read and write operations must be extremely fast (sub-millisecond).
*   **High Scalability:** The system must scale horizontally to handle increasing load and data size by adding more nodes.
*   **High Availability:** The system should remain operational even if some cache nodes fail.
*   **Tunable Consistency:** The system should support different levels of consistency between the cache and the source of truth (the database).
*   **Fault Tolerance:** The system must be resilient to node failures without significant data loss.

## 3. Core Challenge: Data Partitioning (Sharding)

With multiple nodes, the first critical question is: **How do we decide which node stores which key?** This is data partitioning.

#### The Naive Approach: Modulo Hashing
A simple method is to use a hash function and the modulo operator: `node_index = hash(key) % N`, where `N` is the number of nodes.

*   **Fatal Flaw:** This scheme is extremely brittle. If you add or remove a node, `N` changes, causing almost every key to be remapped to a new node. This mass invalidation results in a "cache stampede" or "thundering herd," where the database is suddenly overwhelmed with requests, defeating the purpose of the cache.

#### The Superior Approach: Consistent Hashing
Consistent Hashing is the industry-standard solution to this problem.

1.  **The Hash Ring:** Imagine a conceptual ring or circle representing the entire range of a hash function's output (e.g., 0 to 2^32 - 1).
2.  **Place Nodes:** Hash each node's ID (e.g., IP address) and place it on this ring.
3.  **Place Keys:** To determine where a key belongs, hash the key and find its position on the ring.
4.  **Assign Responsibility:** From the key's position, walk clockwise around the ring until you encounter a node. That node is responsible for storing that key.

**The Magic of Consistent Hashing:**
*   **Node Removal:** If a node is removed, only the keys it was responsible for are remapped to the next node clockwise. The vast majority of keys are unaffected.
*   **Node Addition:** When a new node is added, it takes responsibility for a portion of keys from the next node clockwise. Again, the impact is localized.

**Improvement: Virtual Nodes**
A potential issue with the basic approach is non-uniform data distribution if nodes are not spread evenly on the ring. To solve this, we introduce **virtual nodes**. Each physical node is mapped to multiple virtual nodes on the ring. This ensures that if a physical node is added or removed, the load is distributed much more evenly across the remaining nodes.

## 4. Ensuring Reliability: Replication & Fault Tolerance

Consistent hashing helps route around failed nodes, but the data on the failed node is lost (at least temporarily). This leads to cache misses and increased database load.

**Solution: Replication.** We store copies (replicas) of each piece of data on multiple nodes.
*   A common strategy is to have a **replication factor** (e.g., 3). A key is stored on its primary responsible node (as determined by consistent hashing) and also on the next `N-1` nodes clockwise on the ring.
*   When a primary node fails, requests for its data can be served by its replicas, ensuring high availability.
*   **Replication can be:**
    *   **Synchronous:** Write to primary and all replicas must succeed before acknowledging the client. This provides strong consistency but at the cost of higher write latency.
    *   **Asynchronous:** Write to primary and acknowledge the client immediately. The primary then replicates the data to its replicas in the background. This offers low latency but has a small window for data loss if the primary fails before replication completes. For most caching use cases, asynchronous replication is the preferred trade-off.

## 5. The Million-Dollar Question: Consistency Models

How do we keep the cache synchronized with the database? This is a central trade-off between performance, consistency, and complexity.

#### Cache-Aside (Lazy Loading)
This is the most common caching strategy.
1.  **On Read:** The application first queries the cache.
    *   **Cache Hit:** The data is returned directly from the cache.
    *   **Cache Miss:** The application queries the database, retrieves the data, **populates the cache with it**, and then returns the data.
2.  **On Write:** The application writes directly to the database and then issues a command to **invalidate (delete)** the corresponding key in the cache.

*   **Pros:** Resilient (cache failures don't prevent writes), and only data that is actually requested gets cached.
*   **Cons:** Higher latency on a cache miss. There's also a potential for a race condition leading to stale data (if data is updated in the DB after a miss but before the cache is populated).

#### Write-Through
1.  **On Write:** The application writes to the cache. The cache synchronously writes the data to the database. The operation is only complete after both are successful.
*   **Pros:** High data consistency between cache and DB.
*   **Cons:** Higher write latency as it includes a database write. The database can still be a bottleneck for write-heavy loads.

#### Write-Back (Write-Behind)
1.  **On Write:** The application writes only to the cache, which is extremely fast. The cache then asynchronously writes the data to the database in batches after a certain delay.
*   **Pros:** Extremely low write latency and high write throughput.
*   **Cons:** High risk of **data loss** if a cache node fails before it has persisted its data to the database. Suitable only for non-critical data.

## 6. Handling Finite Space: Eviction Policies

When the cache is full, we need a policy to decide which items to discard.
*   **LRU (Least Recently Used):** Discards the item that hasn't been accessed for the longest time. The most common and often a great default choice.
*   **LFU (Least Frequently Used):** Discards the item that has been accessed the fewest times.
*   **FIFO (First-In, First-Out):** Discards the oldest item.
*   **Random Replacement:** Randomly selects an item to discard.

The choice of policy depends heavily on the data access patterns of the application.

## 7. Advanced Challenges & Solutions

#### Thundering Herd Problem
When a very popular item expires from the cache, multiple requests might miss simultaneously, all hitting the database to fetch the same item and then trying to write it back to the cache.
*   **Solution 1 (Locking):** Only the first process to miss acquires a lock to repopulate the cache. Others wait.
*   **Solution 2 (Stale-while-revalidate):** Serve the old, stale data to most clients for a brief period while a single background process fetches the new data. This prioritizes availability and low latency.

#### Monitoring
A production-grade system needs robust monitoring. Key metrics to track:
*   **Cache Hit/Miss Ratio:** The single most important metric for cache effectiveness.
*   **Latency:** p95, p99 latencies for Get/Set operations.
*   **Eviction/Expiration Rate:** How many items are being removed.
*   **Node Health:** CPU, Memory, and Network I/O for each cache node.

## 8. Conclusion: It's All About Trade-offs

Designing a distributed cache is a classic system design problem because it forces a discussion of fundamental trade-offs:
*   **Performance vs. Consistency:** Write-through is consistent but slow; write-back is fast but risky. Cache-aside is a common balance.
*   **Simplicity vs. Completeness:** A simple modulo sharding is easy to implement but fails under scale; consistent hashing is more complex but robust.
*   **Cost vs. Availability:** Higher replication factors increase availability and read performance but also increase memory costs.

In an interview, demonstrating your understanding of these core concepts—**Partitioning, Replication, Consistency, and Eviction**—and your ability to articulate the trade-offs for a given scenario is the key to success.