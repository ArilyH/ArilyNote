#LLM #Start #DataWhale #NLP #BPE #Unigram #Tokenizer
# BPE-字对编码
BPE 的核心假设是：​**通过合并高频字符对，可以逐步构建出有意义的子词单元**。这种方法能够有效平衡词汇量和未登录词（OOV）问题。
1. ​**初始化词表**：
    - 初始词表包含所有字符（例如英文字母 `a-z` 和其他语言字符）。
    - 统计语料中所有字符对的频率。
2. ​**迭代合并**：
    - 在每轮迭代中，找到语料中出现频率最高的字符对（例如 `"h"` 和 `"a"` 合并为 `"ha"`）。
    - 将合并后的字符对加入词表，并更新语料中的字符对频率。
    - 重复上述过程，直到达到目标词表大小。
 不同语言的字符集差异很大（例如中文汉字、英文字母、日文假名等），直接对字符进行分词会导致词表膨胀。即**数据稀疏**问题。字节编码（UTF-8）将任何 Unicode 字符表示为 1 到 4 个字节，统一了所有语言的表示形式。在字节级别运行 BPE，可以避免对低频字符的依赖，从而提高模型的泛化能力。
 在分词时，如果我们先按频率排，而不是长度，会优先匹配高频 token，即使它们很短（如 `e`, `t`, `a`），会导致字符串被**过早切断成很多小 token**，从而错过更长、更语义完整的 token。

```
def get_tokens_from_vocab(vocab):
    tokens_frequencies = collections.defaultdict(int)
    vocab_tokenization = {}
    for word, freq in vocab.items():
        # 看vocabulary里面的token频率，相当于上面的code中的tokens去除freq为0的
        word_tokens = word.split()
        for token in word_tokens:
            tokens_frequencies[token] += freq
        # vocab和其对应的tokens
        vocab_tokenization[''.join(word_tokens)] = word_tokens
    return tokens_frequencies, vocab_tokenization

def measure_token_length(token):
    
    # 如果token最后四个元素是 < / w >
    if token[-4:] == '</w>':
        # 那就返回除了最后四个之外的长度再加上1(结尾)
        return len(token[:-4]) + 1
    else:
        # 如果这个token里面没有结尾就直接返回当前长度
        return len(token)
    
# 如果vocabulary里面找不到要拆分的词，就根据已经有的token现拆
def tokenize_word(string, sorted_tokens, unknown_token='</u>'):
    
    # base case，没词进来了，那拆的结果就是空的
    if string == '':
        return []
    # 已有的sorted tokens没有了，那就真的没这个词了
    if sorted_tokens == []:
        return [unknown_token] * len(string)

    # 记录拆分结果
    string_tokens = []
    
    # iterate over all tokens to find match
    for i in range(len(sorted_tokens)):
        token = sorted_tokens[i]
        
        # 自定义一个正则，然后要把token里面包含句号的变成[.]
        token_reg = re.escape(token.replace('.', '[.]'))
        
        # 在当前string里面遍历，找到每一个match token的开始和结束位置，比如string=good，然后token是o，输出[(2,2),(3,3)]?
        matched_positions = [(m.start(0), m.end(0)) for m in re.finditer(token_reg, string)]
        # if no match found in the string, go to next token
        if len(matched_positions) == 0:
            continue
        # 因为要拆分这个词，匹配上的token把这个word拆开了，那就要拿到除了match部分之外的substring，所以这里要拿match的start
        substring_end_positions = [matched_position[0] for matched_position in matched_positions]
        substring_start_position = 0
        
        
        # 如果有匹配成功的话，就会进入这个循环
        for substring_end_position in substring_end_positions:
            # slice for sub-word
            substring = string[substring_start_position:substring_end_position]
            # tokenize this sub-word with tokens remaining 接着用substring匹配剩余的sorted token，因为刚就匹配了一个
            string_tokens += tokenize_word(string=substring, sorted_tokens=sorted_tokens[i+1:], unknown_token=unknown_token)
            # 先把sorted token里面匹配上的记下来
            string_tokens += [token]
            substring_start_position = substring_end_position + len(token)
        # tokenize the remaining string 去除前头的substring，去除已经匹配上的，后面还剩下substring_start_pos到结束的一段substring没看
        remaining_substring = string[substring_start_position:]
        # 接着匹配
        string_tokens += tokenize_word(string=remaining_substring, sorted_tokens=sorted_tokens[i+1:], unknown_token=unknown_token)
        break
    else:
        # return list of unknown token if no match is found for the string
        string_tokens = [unknown_token] * len(string)
        
    return string_tokens

"""
该函数生成一个所有标记的列表，按其长度（第一键）和频率（第二键）排序。

EXAMPLE:
    token frequency dictionary before sorting: {'natural': 3, 'language':2, 'processing': 4, 'lecture': 4}
    sorted tokens: ['processing', 'language', 'lecture', 'natural']
    
INPUT:
    token_frequencies: Dict[str, int] # Counter for token frequency
    
OUTPUT:
    sorted_token: List[str] # Tokens sorted by length and frequency

"""
def sort_tokens(tokens_frequencies):
    # 对 token_frequencies里面的东西，先进行长度排序，再进行频次，sorted是从低到高所以要reverse
    sorted_tokens_tuple = sorted(tokens_frequencies.items(), key=lambda item:(measure_token_length(item[0]),item[1]), reverse=True)
    
    # 然后只要tokens不要频次
    sorted_tokens = [token for (token, freq) in sorted_tokens_tuple]

    return sorted_tokens

#display the vocab
tokens_frequencies, vocab_tokenization = get_tokens_from_vocab(vocab)

#sort tokens by length and frequency
sorted_tokens = sort_tokens(tokens_frequencies)
print("Tokens =", sorted_tokens, "\n")

#print("vocab tokenization: ", vocab_tokenization)

sentence_1 = 'I like natural language processing!'
sentence_2 = 'I like natural languaaage processing!'
sentence_list = [sentence_1, sentence_2]

for sentence in sentence_list:
    
    print('==========')
    print("Sentence =", sentence)
    
    for word in sentence.split():
        word = word + "</w>"

        print('Tokenizing word: {}...'.format(word))
        if word in vocab_tokenization:
            print(vocab_tokenization[word])
        else:
            print(tokenize_word(string=word, sorted_tokens=sorted_tokens, unknown_token='</u>'))


```

# Unigram
在BPE过程结束后，我们得到一个分词表。
与仅仅根据频率进行拆分不同，一个更符合**机器学习直觉**的方法是在一个数据集上定义一个目标函数来捕捉一个好的分词的特征，这种基于目标函数的分词模型可以适应更好分词场景。
我们从分词表开始，在语料库上训练这样一个模型：
1. **​Expectation**：
    - 对语料库中的每个词，计算所有可能的分割方式及其似然值（即分割结果子词的概率连乘）。
    - 选择概率最大的分词法作为本轮的分词结果。
2. **​Maximizaion**：
	- 根据Expectation中的分词结果重新计算子词的频率。
3. **计算这样一个损失函数**：
	给定子词 $s$，对语料库 $V$ 中的所有词，计算其最优分割的对数似然之和：
	$$L=sum_{w∈V} P(w)$$ 
	其中$P(w)$是$w$最优分词的似然值。
	移除$s$，对语料库 $V$ 中的所有词，计算其最优分割的对数似然之和：
	$$L_{-s}=sum_{w∈V} P(w)$$
	有$Loss(s)=L-L_{-s}$。
4. **词表裁剪**
	- 根据损失 Loss(s) 对所有子词进行排序，损失越大的子词越重要。
	- 移除损失最小的一定比例子词。
	- 对裁剪后的词表，重新归一化子词概率。

其Predict过程和Training过程类似：
- 对输入词生成所有可能的子词分割路径。
- 选择概率乘积最大的路径作为最终分割结果。
优点：
- **灵活性强**：Unigram 模型支持动态调整子词概率，能够更好地适应不同任务和数据分布。
- ​**处理未登录词能力强**：Unigram 模型通过单字符分割保证覆盖，能够有效处理未登录词（**未登录词（Out-of-Vocabulary, OOV）​** 是指在模型训练过程中未出现在词表或训练数据中的词汇）。