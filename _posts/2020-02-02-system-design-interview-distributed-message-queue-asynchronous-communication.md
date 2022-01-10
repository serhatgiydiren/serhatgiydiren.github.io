---
layout: default
---

###### [1 - Synchronous Communication](system-design-interview-distributed-message-queue-synchronous-communication)  
###### [2 - Asynchronous Communication](system-design-interview-distributed-message-queue-asynchronous-communication)  
###### [3 - Functional Requirements](system-design-interview-distributed-message-queue-functional-requirements)  
###### [4 - Non-Functional Requirements](system-design-interview-distributed-message-queue-non-functional-requirements)  
###### [5 - High-level Architecture](system-design-interview-distributed-message-queue-high-level-architecture)  
###### [6 - VIP and Load Balancer](system-design-interview-distributed-message-queue-vip-and-load-balancer)  
###### [7 - FrontEnd Service](system-design-interview-distributed-message-queue-frontend-service)  
###### [8 - Metadata Service](system-design-interview-distributed-message-queue-metadata-service)  
###### [9 - BackEnd Service](system-design-interview-distributed-message-queue-backend-service)  
###### [10 - Option A : Leader - Follower Relationship](system-design-interview-distributed-message-queue-option-a-leader-follower-relationship)  
###### [11 - Option B : Small cluster of independent hosts](system-design-interview-distributed-message-queue-option-b-small-cluster-of-independent-hosts)  
###### [12 - In-cluster Manager vs Out-cluster Manager](system-design-interview-distributed-message-queue-in-cluster-manager-vs-out-cluster-manager)  
###### [13 - Queue creation and deletion](system-design-interview-distributed-message-queue-queue-creation-and-deletion)  
###### [14 - Message deletion](system-design-interview-distributed-message-queue-message-deletion)  
###### [15 - Message replication](system-design-interview-distributed-message-queue-message-replication)  
###### [16 - Message delivery semantics](system-design-interview-distributed-message-queue-message-delivery-semantics)  
###### [17 - Push vs Pull](system-design-interview-distributed-message-queue-push-vs-pull)  
###### [18 - FIFO](system-design-interview-distributed-message-queue-fifo)  
###### [19 - Security](system-design-interview-distributed-message-queue-security)  
###### [20 - Monitoring](system-design-interview-distributed-message-queue-monitoring)  
###### [21 - Final Look](system-design-interview-distributed-message-queue-final-look)  

### Asynchronous Communication
- Queue : Producer sends data to that component and exactly one consumer gets this data to a short time after.
- It is distributed, because data is stored across several machines. 
- Do not confuse queue with topic. In case of a topic, message goes to all subscribers. In case of a queue, message is received by only one consumer. 