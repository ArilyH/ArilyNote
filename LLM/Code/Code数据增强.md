#Code #DataAugentation
## 1. Links
Paper：[[2302.03499] Exploring Data Augmentation for Code Generation Tasks](https://arxiv.org/abs/2302.03499)

## 2. BackGrounds
### **Back-Translation**
反向翻译（Back-Translation, BT）是一种**数据增强技术**，最初用于自然语言处理（NLP）的机器翻译任务，后来被扩展到代码生成领域。它的核心思想是**利用一个辅助模型生成伪训练数据**，从而扩充原始数据集，提升模型的泛化能力。
假设我们要训练一个模型，将 ​**C# 代码** 翻译成 ​**Java 代码**，但现有的平行语料（成对的 C#-Java 代码）较少。反向翻译的步骤如下：
1. ​**训练一个逆向模型（Java→C#）​**
    - 使用现有的少量平行数据（C#↔Java）训练一个 Java→C# 的翻译模型 g。
2. ​**生成伪数据（C#→Java）​**
    - 收集大量**单语 Java 代码**​（不需要成对的 C# 代码）。
    - 用逆向模型 g 把这些 Java 代码**反向翻译**成 C# 代码，得到伪 C#-Java 对。
3. ​**训练正向模型（C#→Java）​**
    - 用**原始数据 + 伪数据**训练最终的 C#→Java 模型 f。

假设你试图训练一个模型去做**代码摘要**，那你则需要大量的单语注释，这是不太可能的。