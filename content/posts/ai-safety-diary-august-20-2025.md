---
title: "AI Safety Diary: August 20, 2025"
date: '2025-08-20T12:50:04+00:00'
categories:
  - "AI Safety"
tags:
  - "ai safety diary"
  - "vending-bench"
  - "autonomous agents"
  - "llm benchmarks"
  - "long-term coherence"
summary: "A diary entry on Vending-Bench, a benchmark for evaluating the long-term coherence and decision-making capabilities of autonomous LLM-based agents in a simulated business environment."
---

Today, I explored a research paper as part of my AI safety studies. Below is the resource I reviewed.

## Resource: Vending-Bench: A Benchmark for Long-Term Coherence of Autonomous Agents

- **Source**: [Vending-Bench: A Benchmark for Long-Term Coherence of Autonomous Agents](https://arxiv.org/pdf/2502.15840) by Axel Backlund and Lukas Petersson, Andon Labs, arXiv:2502.15840, February 2025.
- **Summary**: This paper introduces Vending-Bench, a simulated environment designed to test the long-term coherence of large language model (LLM)-based agents in managing a vending machine business. Agents must handle inventory, orders, pricing, and daily fees over extended periods (&gt;20M tokens per run), revealing high variance in performance. Models like Claude 3.5 Sonnet and o3-mini often succeed but can fail due to misinterpreting schedules, forgetting orders, or entering "meltdown" loops. The benchmark highlights LLMs’ challenges in sustained decision-making and tests their ability to manage capital, relevant to AI safety in scenarios involving powerful autonomous agents.