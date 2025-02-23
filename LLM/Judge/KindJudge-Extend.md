### 一、**同义词表与文本扰动工具**

#### 1. **WordNet（NLTK内置）**

- **功能**：提供英语词汇的语义网络（同义词、反义词、上下位词等）。
    
- **用途**：生成同义词替换列表。
    
- **代码示例**：
    
    python
    
    复制
    
    from nltk.corpus import wordnet as wn
    
    # 获取单词 "happy" 的同义词
    synonyms = set()
    for syn in wn.synsets("happy"):
        for lemma in syn.lemmas():
            synonyms.add(lemma.name())
    print(synonyms)  # 输出：{'happy', 'felicitous', 'glad', ...}


### 二、**写作风格分析与偏好建模**

#### 1. **Stylometric Features**

- **工具**：`style` 库（如 `stylo` R包）或自定义特征提取。
    
- **功能**：分析文本的句法复杂度、词汇多样性、词长分布等风格特征。
    
- **用途**：量化不同JudgeLLM的写作风格偏好。
    

#### 2. **预训练风格分类器**

- **工具**：Hugging Face模型（如 `roberta-base`）微调。
    
- **步骤**：
    
    1. 收集不同JudgeLLM的评分数据。
        
    2. 训练一个分类器，预测文本风格对应的JudgeLLM偏好。
        
    3. 提取风格敏感词汇（如某些同义词在特定模型中得分更高）。
        

#### 3. **GPT-4 Prompt Engineering**

- **方法**：通过Prompt直接探测LLM的风格偏好：
    
    复制
    
    "Which sentence is better? 
     A: 'The solution is effective and efficient.'
     B: 'The solution works well and is cost-effective.'"
    
- **用途**：快速验证不同同义词对模型评分的影响。

### 三、**攻击与防御方案**

#### 1. **攻击方案：基于偏好的对抗攻击**

- **步骤**：
    
    1. **构建偏好词典**：在本地蒸馏模型上训练，生成“同义词-偏好分数”映射表。
        
    
    python
    
    复制
    
    # 伪代码：训练一个线性模型预测同义词得分
    from sklearn.linear_model import LinearRegression
    
    # 假设 X 是词向量，y 是偏好分数
    model = LinearRegression().fit(X, y)
    preferred_synonyms = {word: score for word, score in zip(vocab, model.predict(X))}
    
    1. **动态替换**：根据目标JudgeLLM的偏好词典，替换原文中的词汇。
        
    3. **迁移攻击**：验证本地偏好词典对在线LLM（如ChatGPT）的攻击效果。
        

#### 2. **防御方案**

- **方法1：对抗训练（Adversarial Training）**
    
    - 在训练数据中加入扰动样本，增强模型鲁棒性。
        
- **方法2：风格归一化（Style Normalization）**
    
    - 使用文本风格迁移模型（如 [StylePTB](https://github.com/luofuli/StylePTB)）将输入文本转换为中性风格。
        
- **方法3：检测对抗样本**
    
    - 使用困惑度（Perplexity）或文本一致性检测工具（如 [GLTR](http://gltr.io/)）识别扰动文本。
        
    
    python
    
    复制
    
    from transformers import GPT2LMHeadModel, GPT2Tokenizer
    
    model = GPT2LMHeadModel.from_pretrained('gpt2')
    tokenizer = GPT2Tokenizer.from_pretrained('gpt2')
    
    def detect_attack(text, threshold=50):
        inputs = tokenizer(text, return_tensors="pt")
        loss = model(**inputs, labels=inputs["input_ids"]).loss
        perplexity = torch.exp(loss).item()
        return perplexity > threshold  # 高困惑度可能是对抗样本