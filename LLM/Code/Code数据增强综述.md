#Code #DataAugentation
## 1. Links
Paper：[[2305.19915] Source Code Data Augmentation for Deep Learning: A Survey](https://arxiv.org/abs/2305.19915)
Github：[GitHub - terryyz/DataAug4Code: Source Code Data Augmentation for Deep Learning: A Survey.](https://github.com/terryyz/DataAug4Code)

## 2. Intro
在许多 NLP 任务中，句子的上下文可以是相对独立的，或者来 自周围的一两个句子。然而，在源代码中，由于函数调用、面向对象编程和 模块化设计的广泛应用，上下文可以跨越多个函数甚至不同的文件。 因此，针对源代码的 DA 方法需要考虑这种扩展上下文，以避免引入错误或改变原始程序的行为。此外，源代码遵循使用 上下文无关文法指定的严格语法规则。因此，传统 的 NLP 数据增强方法，如使用同义词进行 token 替换，可能会导致增强的 源代码无法编译，并为模型训练引入错误知识。

为了实现对给定源代码的复杂处理，一种常见的方法是使用解析器从代码中构建一个具体的语法树，以树形形式表示程序语法。具体的语法树将进一步转换为抽象语法树（AST），以简化表示但保持关键信 息，如标识符、if-else 语句和循环条件。解析信息 被用作基于规则的 DA 方法的基础，用于标识符替 换和语句重写。