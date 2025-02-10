---
date: 2025-02-10
tags:
  - LLM
  - Judge
---
## 1. Links
Github：[GitHub - rainavyas/attack-comparative-assessment: Adversaial attack comparative assessment Large Language Model](https://github.com/rainavyas/attack-comparative-assessment)
Paper：[2024.emnlp-main.427.pdf](https://aclanthology.org/2024.emnlp-main.427.pdf)

## 2. Intro
**现状**
现有研究尚未分析评判者LLMs对对抗性攻击的脆弱性。

本研究首次探讨了评估LLMs的对抗鲁棒性，可以通过拼接简短的通用对抗短语来欺骗LLM Judge，使其预测出虚高的分数。

鉴于攻击者可能不了解或无法访问评判者LLMs，我们提出了一种简单的替代攻击方法，即先攻击一个替代模型，然后将学习到的攻击短语转移到未知的评判者LLMs上。我们提出了一种实用算法来确定简短的通用攻击短语，并证明当这些短语转移到未见过的模型时，分数可以被大幅夸大，以至于无论评估文本如何，都会预测出最高分。研究发现，当评判者LLMs用于绝对评分而非比较评估时，它们对这些对抗性攻击的敏感性显著增加。 我们的研究结果引发了对 LLM 作为评判方法可靠性的担忧，并强调了在高风险现实场景中部署之前解决LLM评估方法中漏洞的重要性。