---
title: "AI Safety Diary: September 6, 2025"
date: '2025-09-06T18:07:18+03:00'
categories:
  - "AI Safety"
  - "AI Interpretability"
  - "AI Alignment"
tags:
  - "ai safety diary"
  - "chain-of-thought"
  - "monitoring"
  - "unfaithful reasoning"
summary: "A diary entry on how Chain-of-Thought (CoT) reasoning affects LLM's ability to evade monitors, and the challenge of unfaithful reasoning in model explanations."
---

Today, I explored two research papers as part of my AI safety studies. Below are the resources I reviewed.

## Resource: When Chain of Thought is Necessary, Language Models Struggle to Evade Monitors
- **Source**: [When Chain of Thought is Necessary, Language Models Struggle to Evade Monitors](https://arxiv.org/pdf/2507.05246), arXiv:2507.05246, July 2025.
- **Summary**: This paper investigates scenarios where chain-of-thought (CoT) reasoning is required, finding that LLMs struggle to evade safety monitors in these contexts. It highlights challenges in ensuring CoT faithfulness, critical for detecting misbehavior and maintaining AI safety.

## Resource: Reasoning Models Don’t Always Say What They Think
- **Source**: [Reasoning Models Don’t Always Say What They Think](https://arxiv.org/pdf/2505.05410), arXiv:2505.05410, May 2025.
- **Summary**: This paper explores unfaithful reasoning in LLMs, where models generate misleading CoT explanations that don’t reflect their actual decision-making process. It discusses implications for AI safety, particularly the difficulty of relying on CoT for monitoring and alignment.