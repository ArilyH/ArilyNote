#Code #DataAugentation
## 1. Links
Paper：[[2305.19915] Source Code Data Augmentation for Deep Learning: A Survey](https://arxiv.org/abs/2305.19915)
Github：[GitHub - terryyz/DataAug4Code: Source Code Data Augmentation for Deep Learning: A Survey.](https://github.com/terryyz/DataAug4Code)


在许多 NLP 任务中，句子的上下文可以是相对独立的，或者来 自周围的一两个句子（Feng 等人，2021）。然 而，在源代码中，由于函数调用、面向对象编程和 模块化设计的广泛应用，上下文可以跨越多个函数 甚至不同的文件。 因此，我们认为针对源代码的 DA 方法需要考虑这种扩展上下文，以避免引入错 误或改变原始程序的行为。此外，源代码遵循使用 上下文无关文法指定的严格语法规则。因此，传统 的 NLP 数据增强方法，如