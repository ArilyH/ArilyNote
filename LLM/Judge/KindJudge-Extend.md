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

