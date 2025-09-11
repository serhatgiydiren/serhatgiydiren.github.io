---
title: "AI Safety Diary: August 27, 2025"
date: '2025-08-27T17:59:25+00:00'
categories:
  - "AI Safety"
  - "AI Interpretability"
tags:
  - "ai safety diary"
  - "chain-of-thought"
  - "faithfulness"
  - "implicit bias"
  - "llm reasoning"
summary: "A diary entry on the unfaithfulness of Chain-of-Thought (CoT) reasoning in LLMs, highlighting issues like implicit biases and logically contradictory outputs, which pose challenges for AI safety monitoring."
---

Today, I explored a research paper as part of my AI safety studies. Below is the resource I reviewed.

## Resource: Chain-of-Thought Reasoning In The Wild Is Not Always Faithful

- **Source**: [Chain-of-Thought Reasoning In The Wild Is Not Always Faithful](https://arxiv.org/pdf/2503.08679) by Iván Arcuschin et al., arXiv:2503.08679, June 2025.
- **Summary**: This paper investigates the faithfulness of chain-of-thought (CoT) reasoning in LLMs, finding that models can produce logically contradictory CoT outputs due to implicit biases, termed "Implicit Post-Hoc Rationalization." For example, models may justify answering "Yes" to both "Is X bigger than Y?" and "Is Y bigger than X?" The study shows high rates of unfaithful reasoning in models like GPT-4o-mini (13%) and Haiku 3.5 (7%), raising challenges for detecting undesired behavior via CoT monitoring.[](https://arxiv.org/abs/2503.08679)