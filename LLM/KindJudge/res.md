统计方式

avg Δrank 统计所有response的rank attack_response rank 评估变化量的绝对值

Attack i spearman 是把所有responseattack以后算的排位

qwen2.5

conala

No attack spearman  0.2366850937735697

Attack i avg. Δrank 1.2801587301587303

Attack i spearman  -0.09221375517203143

Attack i Δspearman -0.3288988489456011

  

humaneval

No attack spearman  0.27051102404628735

Attack i avg. Δrank 6.141741071428571

Attack i spearman  0.1383936158428601

Attack i Δspearman -0.13211740820342724

  

armoRM

conala

No attack spearman  0.6761400205953629

Attack i avg. Δrank 1.8235449735449738

Attack i spearman  0.4357364224113951

Attack i Δspearman -0.24040359818396784

  

humaneval

No attack spearman  0.4365127343039977

Attack i avg. Δrank 9.46339285714286

Attack i spearman  0.177921845007807

Attack i Δspearman -0.2585908892961907

  

Eurus

conala

No attack spearman  0.5239179425214464

Attack i avg. Δrank 0.87989417989418

Attack i spearman  0.4721134213507732

Attack i Δspearman -0.05180452117067319

  

humaneval

No attack spearman  0.03707028116754964

Attack i avg. Δrank 8.416741071428572

Attack i spearman  0.18491010954745188

Attack i Δspearman 0.14783982837990223

  

sfairRM

conala

No attack spearman  0.7116721268746794

Attack i avg. Δrank 1.5515873015873016

Attack i spearman  0.6062011464591296

Attack i Δspearman -0.10547098041554981

  

humaneval

No attack spearman  0.5052327401496826

Attack i avg. Δrank 9.239508928571428

Attack i spearman  0.24352790210910835

Attack i Δspearman -0.2617048380405742

  

RM_Mistral

conala

No attack spearman  0.6856630883273448

Attack i avg. Δrank 1.7449735449735448

Attack i spearman  0.5670303518941308

Attack i Δspearman -0.11863273643321404

  

humaneval

No attack spearman  0.4429171471154983

Attack i avg. Δrank 9.342187499999998

Attack i spearman  0.2433592717217456

Attack i Δspearman -0.1995578753937527

  
  

lcb

armoRM

No attack spearman  0.125

Attack i avg. Δrank 0.5

Attack i spearman  -0.23529411764705882

Attack i Δspearman -0.3602941176470588

  
  

sfairRM

No attack spearman  0.23529411764705882

Attack i avg. Δrank 0.5

Attack i spearman  -0.058823529411764705

Attack i Δspearman -0.29411764705882354

  

qwen2.5

No attack spearman  0.23529411764705882

Attack i avg. Δrank 0.3897058823529412

Attack i spearman  -0.29411764705882354

Attack i Δspearman -0.5294117647058824

  

RM_Mistral_7B

No attack spearman  -0.11764705882352941

Attack i avg. Δrank 0.5

Attack i spearman  -0.11764705882352941

Attack i Δspearman 0.0

  
  
  

Conala

| Model       | Spearman₀ | Spearman₁ | ΔSpearman | ΔS / S₀     |

| ----------- | --------- | --------- | --------- | ----------- |

| qwen2.5     | 0.237     | -0.092    | 0.329     | **1.388**   |

| armoRM      | 0.676     | 0.436     | 0.240     | 0.355       |

| Eurus       | 0.524     | 0.472     | 0.052     | **0.099** ✅ |

| sfairRM     | 0.712     | 0.606     | 0.105     | 0.148       |

| RM\_Mistral | 0.686     | 0.567     | 0.119     | 0.174       |

  

humm

| Model       | Spearman₀ | Spearman₁ | ΔSpearman | ΔS / S₀          |

| ----------- | --------- | --------- | --------- | ---------------- |

| qwen2.5     | 0.271     | 0.138     | 0.132     | 0.487            |

| armoRM      | 0.437     | 0.178     | 0.259     | 0.593            |

| Eurus       | 0.037     | 0.185     | -0.148    | **-4.000** ❗（提升） |

| sfairRM     | 0.505     | 0.244     | 0.262     | 0.519            |

| RM\_Mistral | 0.443     | 0.243     | 0.200     | 0.451            |

  
  

总评

| 模型              | 准确性评价           | 鲁棒性评价                        | 综合表现简评                |

| --------------- | --------------- | ---------------------------- | --------------------- |

| **qwen2.5**     | ❌ 准确性差          | ⚠️ 鲁棒性差（conala）、中（humaneval） | 初始就弱，攻击下更不稳定          |

| **armoRM**      | ✅ 准确性强          | ❌ 鲁棒性差                       | 表现准，但容易被攻击扰乱          |

| **Eurus**       | ⚠️ humaneval 极差 | ✅ conala 最鲁棒                 | 鲁棒稳定，但准确性偏弱，有奇特行为     |

| **sfairRM**     | ✅ 最准确（两任务）      | ⚠️ 鲁棒性一般偏差                   | 表现强但“玻璃心”，不抗攻击        |

| **RM\_Mistral** | ✅ 准确性较高         | ✅ 鲁棒性较强                      | 🥇 **准确 + 稳定，综合最佳候选** |

我们要找到这样一个场景 模型能够生成足够正确但不够优秀的代码 并被不够鲁棒的RM一票否决。

也就是说 我们要找到模型“竭尽全力”的一个数据集。

实在不行就把mutation塞给LLM的回答吧。