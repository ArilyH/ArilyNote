## 语法错误扰动
- **拼写错误**
    - 示例：`exmaple` 替代 `example`。
- **时态错误**
    - 示例：`He go to school yesterday.` 替代 `He went to school yesterday.`。
- **主谓不一致**
    - 示例：`The data are accurate.` 替代 `The data is accurate.`。
- **词序错误**
    - 示例：`She quickly runs.` 替代 `Quickly she runs.`。
- **冗余或缺失词语**
    - 示例：`I not understand.` 替代 `I do not understand.`。

```
- **Spelling Errors**
    
    - Example: `exmaple` instead of `example`.
- **Tense Errors**
    
    - Example: `He go to school yesterday.` instead of `He went to school yesterday.`
- **Subject-Verb Agreement Errors**
    
    - Example: `The data are accurate.` instead of `The data is accurate.`
- **Word Order Errors**
    
    - Example: `She quickly runs.` instead of `Quickly she runs.`
- **Redundant or Missing Words**
    
    - Example: `I not understand.` instead of `I do not understand.`
```

## 扰动等级
- **轻度噪声**：1-2 个语法错误，分布于整个回答。
- **中度噪声**：3-5 个语法错误，分布于整个回答。
- **重度噪声**：每句话至少一个语法错误。
- **可控错乱**：将语法错误集中在特定位置，如回答开头或结尾，测试 LLM Judge 的敏感度。

## PPL
- 加噪
	针对 LLM，使用以下提示生成带语法错误的 response：
		将以下文本改写为包含轻微语法错误的版本，确保语义不变，并随机加入以下语法错误类型：拼写错误、时态错误、主谓不一致、代词指代不明等。错误数量不超过 2 个。
		文本：{original_response}
- 评估语义相似度
	采用SBert选用基于深层语义的嵌入模型，可以忽略语法结构或表面词汇变化对语义的影响。
- Bench
	使用加噪后的Bench评估LLM Judge

## Metrics
- Robust Rate: 扰动前后的Judge结果差异。
- Consistent Rate: 特定条件下的一致性差异。

## Exp
### 鲁棒性评价
评价不同扰动强度影响下现有Judge抗扰动的性能。

### 偏好评价
评价现有Judge在哪些错误类型下表现更好。

### Ablation
- 扰动模型与Judge模型同源状况下的影响。


