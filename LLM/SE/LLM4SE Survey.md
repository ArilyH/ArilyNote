#LLM #SE #AI 
# RQ
RQ1：迄今为止，为了解决软件工程任务，使用了哪些LLM方法？（Methodology&LLMs）
RQ2：在LLM中，与软件工程相关的数据集是如何收集、预处理和使用的？（Datasets）
RQ3：用于优化和评估 LLM4SE 的技术有哪些？（Optimization&Evaluation）
RQ4：迄今为止，使用 LLM4SE 有效地解决了哪些软件工程任务？（Tasks&Goals）


# Keywords
## SE
软件工程、软件开发、软件测试、软件维护*、SE、软件生命周期、软件设计*、代码表示、代码生成、代码注释生成、代码搜索、代码本地化、代码补全、代码摘要、方法名生成、错误检测、错误定位、漏洞检测、测试技术、测试用例生成、程序分析、错误分类、缺陷预测、程序修复、代码克隆检测、错误报告、软件质量评估、SATD 检测、代码异味检测、编译相关、代码审查、软件分类、代码分类、代码变更、事件检测、需求提取、需求可追溯性、需求验证、努力成本预测、GitHub/Github 挖掘、SO（Stack Overflow）/SO 挖掘、应用挖掘、标签/标签挖掘、基于开发者的挖掘

## LLM
LLM，LM，PLM，预训练，NLP，ML，DL，AI，Transformer，BERT，CodeX，GPT，T5，注意力模型，迁移学习，NN，ChatGPT

# Topics
![[Pasted image 20250304054053.png]]



# RQ 1 LLMs
## RQ 1.1 目前的LLM4SE任务中使用的LLM
![[Pasted image 20250304054320.png]]
### EncoderOnly（理解型任务）
仅利用模型编码器组件的神经网络架构。处理和编码输入，将其转换为隐藏feature，捕捉词语之间的关系和句子的整体上下文。如BERT。在需要细微理解整个句子或代码片段的任务中可以使用编码器大模型。例如代码审查、错误报告理解和与代码实体相关的命名实体识别。
训练过程一般采用掩码语言模型任务（预测token）
目前的LLM4SE任务中有多个基于BERT的研究，
如CodeBERT：整合了一种标记预测方案，通过预测后续标记来理解代码，从而增强了其在代码补全和错误检测等任务中对编程语言的理解。其目标是通过联合学习自然语言与编程语言的语义关联，支持代码搜索、生成等跨模态任务
GraphCodeBERT：将代码元素之间的关系识别为图（数据流图），引入了数据流图边类型预测，可以更好地学习代码和程序结构之间的语义关联。GraphCoderBERT 能够利用代码结构，提高其在代码摘要和程序分析等任务中的有效性。

### Encoder-Decoder（理解&生成）
结合编码器和解码器的LLM。编码器将输入句子编码成feature，有效地捕捉其底层结构和语义。这弥合了不同输入和输出格式之间的差距。解码器利用feature生成目标输出文本，将抽象表示转化为具体且与上下文相关的表达。如T5 和 CodeT5 等模型体现了这种架构。适用于代码摘要等（理解&生成任务）
训练过程一般采用Span Corruption（会Mask连续一段，按顺序预测token）

CodeT5：基于T5的结构，在三个任务（代码预测、标识符区分、标识符预测）预训练，并通过下游任务微调适应具体场景。
AlphaCode：核心创新在于推理阶段的生成-筛选策略：针对同一题目，生成 ​**数百万个候选代码**​（例如 100 万条 Python 代码）。用题目提供的示例测试用例快速验证代码。淘汰 ​**99% 以上的错误代码**，仅保留通过示例测试的代码（约 1 万条）。对剩余代码进行 ​**聚类分析**，基于代码结构或输出结果相似性分组。从每个聚类中选取 ​**代表性代码**​ 提交，避免重复提交相似代码。

## DecoderOnly（生成型任务）
仅使用解码器模块生成目标输出文本，遵循强调序列预测的独特训练范式。仅解码器架构通过自回归（Autoregressive）方式生成目标文本。与编码器-解码器架构不同，它无需独立的编码器处理输入，而是将输入和输出统一为连续的序列。如GPT Copilot。仅解码器LLMs通常更适合各种生成任务，如代码生成和代码补全。这些模型通常可以从几个示例或简单指令中执行下游任务，而无需添加预测头或微调。生成结果依赖提示设计（Prompt Engineering），不稳定。对复杂任务（如多步推理）性能可能不如微调模型。适用于代码生成。
典型工作如InstructGPT（RLHF&SFT）、CodeX&Copilot（代码&IDE数据微调）。

## Trend
![[Pasted image 20250304165500.png]]
2020年集中在仅编码器模型（BERT等）OnSE研究。
2021年开始出现较多仅解码器模型forSE研究。
2022年（GPT3）后LLM4SE方向研究激增，且仅解码器模型逐渐占领统治地位。

显著趋势是针对精确软件工程（SE）任务的LLMs定制。通过使用针对特定功能（如错误检测或代码审查）定制的数据集微调模型，研究能够实现显著的性能提升。


## Summary
每个LLM架构在软件工程任务中都有其特定的用途，其中仅编码器LLMs专注于全面理解，编码器-解码器LLMs用于需要理解输入信息后进行内容生成的任务，而仅解码器LLMs更适合生成任务。
最广泛使用的LLMs是仅使用解码器架构。

# RQ2 DataSets
![[Pasted image 20250304192507.png]]
## Source
数据源分为四类：开源数据集、收集数据集、构建数据集和工业数据集。
开源数据集如：
- ​**HumanEval**：包含164个手工编写的Python编程问题及其单元测试，用于评估代码生成能力。
- **CodeSearchNet**：涵盖多种编程语言（Python、Java等）的代码片段与自然语言查询配对。
 ​**高可信度**：经学术界验证，标注质量可靠。**可复现性**：支持研究复现和结果对比。**领域限制**：可能无法覆盖特定场景。
 
收集数据集如从​**Stack Overflow**爬取问答帖构建代码修复数据集/**GitHub Issues**提取用户反馈生成需求描述与代码的映射。数据来源广，费时费力且需清洗无关内容。

构建数据集通过人工标注、数据增强或合成方法改造现有数据，以满足特定需求。可以直接适配研究目标、平衡数据分布。但费时费力，且合成数据可能与真实场景存在差距。

工业数据集是来自企业内部的私有数据，包含业务代码、用户日志等敏感信息。直接反映实际场景需求。获取此类数据可能需要保密协议和其他法律保障来保护商业利益。每种数据集类型都提供了独特的优势和挑战，选择它们应指导当前研究项目的具体需求和限制。


## Category & Data Type
![[Pasted image 20250304192736.png]]
**Category**：基于代码的、基于文本的、基于图的、基于软件仓库的以及混合数据。大多数研究采用基于文本的数据集。
基于文本的数据集在训练用于 SE 任务的LLMs中的主导地位突显了模型卓越的自然语言处理能力。使它们成为涉及代码理解、错误修复、代码生成和其他以文本为导向的 SE 研究的理想选择。

**Data Type**: 
基于文本：

![[Pasted image 20250304194629.png]]
*Exp from HumanEval*
基于文本数据集中，LLMs最常使用的数据类型是**编程任务/问题**。通过提供代码片段、单元测试及问题描述，使模型掌握语法结构、逻辑推理和跨任务泛化能力，例如基于HumanEval或CodeContests等数据集优化代码生成与补全。

![[Pasted image 20250304194955.png]]
*Exp from CodeXGLUE*
**Prompt**。通过自然语言指令（如“生成Java快速排序代码”）或上下文示例，明确任务目标并约束输出格式，显著提升模型对特定需求的响应精度。

**Stack Overflow帖子和错误报告**等文本数据。为模型注入实际开发场景的知识，如从问答对中学习错误调试模式，或通过缺陷描述与修复代码的映射关系训练自动补丁生成能力。然而，这些数据面临质量参差、领域局限及隐私风险等挑战，需借助自动化清洗（如静态分析验证代码）、领域微调（适配企业私有代码库）及合规处理（差分隐私脱敏）等手段平衡实用性与安全性。未来趋势将侧重于多模态数据融合（如结合代码与设计文档）及交互式学习（实时采集开发者行为），以构建更全面、动态且符合伦理的代码智能系统。



基于源代码：
在软件工程任务的大语言模型（LLM）训练中，**源代码**​作为最核心的数据类型，直接支撑模型对软件开发逻辑的深度理解，使其能够生成、分析和重构代码。
例如通过大规模开源代码库（如GitHub）学习语法规则与控制流模式。



![[Pasted image 20250304195935.png]]
*Exp from Defect4J*
**缺陷代码与补丁**​聚焦于程序修复任务，通过缺陷-修复对（如Defects4J数据集）训练模型识别错误模式并生成有效补丁。


![[Pasted image 20250304200106.png]]
*Exp from SARD*
**易受攻击代码**​针对漏洞检测场景，利用含CVE标注的代码（如SARD数据集）增强模型对安全风险的敏感度；

**图结构数据**​（如GUI界面依赖图）通过捕捉代码组件间的关系（如Android应用界面布局），支持模型处理需结构化表征的任务（如界面原型生成），凸显多模态数据在复杂软件工程问题中的潜力。这些数据类型的多样性共同推动了LLM在代码生成、缺陷定位、安全检测及系统设计等全链路任务中的适应性。

**基于软件仓库的数据集**通过整合版本控制系统（如Git）中的代码库、提交记录、问题追踪等信息，全面覆盖软件开发生命周期中的代码演化、缺陷修复及协作动态，例如从代码提交历史中提取代码变更模式以分析质量演化趋势，或结合问题追踪系统的标签与讨论内容训练模型自动分类缺陷优先级，此类数据不仅支持实证研究（如开发者行为分析、代码异味检测），还为模型训练提供真实场景下的多模态输入（如代码差异补丁、提交注释与需求描述的关联），从而提升自动化工具在代码推荐、缺陷预测等任务中的实用性和准确性。


## Preprocessing
![[Pasted image 20250304200506.png]]
基于文本数据集的预处理流程包含七个核心步骤：首先通过**数据提取**从软件工程文档（如缺陷报告、需求文档、代码注释及API文档）中抽取任务相关文本，确保覆盖多样化的文本信息；随后进行**文本分割与分类**，按研究需求将文本划分为句子或单词级别（例如过滤少于15字的缺陷报告以剔除信息量不足的样本），并执行**低质数据删除**以移除无效或无关内容；接着通过**标准化清洗**​（如去除特殊符号、停用词）统一文本格式，降低模型处理噪声；**去重操作**消除重复样本以避免数据冗余和偏差，提升模型泛化能力；**数据标记化**将文本拆分为词或子词单元（如WordPiece分词），适配大语言模型的输入格式；最后将处理后的数据划分为**训练集、验证集与测试集**，确保模型评估的科学性。相较于代码数据集预处理（如AST解析或语法约束），文本处理更强调自然语言结构的规范化和语义单元的精细化切分，以适配问答、摘要等任务的上下文理解需求。

基于代码数据集的预处理流程涵盖七个步骤：首先通过**数据提取**从软件仓库或版本控制系统（如Git）中按粒度（方法、文件或完整项目）抽取代码段，例如从Apache Commons提取特定函数；随后**移除低质代码**​（如不完整或无关片段）以确保数据任务相关性，并**去重**以消除冗余提升多样性；接着**数据编译**将筛选后的代码整合为统一数据集，优化存储与访问效率；**移除无效代码**​（如无法编译的代码段）保证数据可用性；**代码表示**阶段将代码转换为模型可处理的格式，例如基于词法单元的标记化（Tokenization）、抽象语法树（AST）解析或程序依赖图（PDG）构建（如控制流图CFG与调用图CG），以捕捉代码结构及依赖关系；最终通过**数据分割**划分为训练集、验证集与测试集，分别用于模型训练、超参调优及性能评估。这一流程确保数据集的结构化与标准化，适配代码补全、缺陷检测及摘要生成等任务，例如通过AST增强模型对语法规则的理解，或利用PDG提升漏洞检测的上下文感知能力。

## Formats
![[Pasted image 20250304201236.png]]
![[Pasted image 20250304201735.png]]

**基于Token的输入。** 将代码和文本分解为更小的语义单元（Token），以适配大语言模型（LLMs）的序列处理能力。保留编程语言的关键字和符号结构。对于混合场景（如代码注释与代码关联），模型通过联合Token序列同时学习代码逻辑与自然语言描述的对应关系。
![[Pasted image 20250304201820.png]]
子词Token化（如BPE、WordPiece）可进一步处理复杂词汇，例如将变量名`userAuthenticationHandler`拆解为`["user", "Auth", "entication", "Handler"]`，增强模型对未登录词（OOV）的泛化能力。这种细粒度表示使模型能精准捕捉代码语法（如括号匹配）、语义（如变量作用域）及跨模态关联（如注释与代码功能的一致性），支撑代码补全、缺陷检测等任务的高效执行。

**基于树**（如AST）的输入通过层级结构解析代码的语法规则，适用于静态分析任务（如语法纠错、代码补全），例如通过AST节点精准定位缺失的括号；而**基于图**（如CFG、DFG）的输入通过非线性关系建模代码的运行时行为，擅长动态分析场景（如漏洞检测、性能优化），例如通过数据流图追踪未经验证的输入传递路径。树结构强调代码的**语法正确性**，图结构则关注代码的**逻辑安全性**，两者结合可提升模型对代码的全方位理解能力，从而在代码生成、缺陷修复及安全检测等任务中实现更高精度。

**基于像素的输入**。基于像素的输入将代码可视化为图像，其中每个像素代表一个代码元素或标记。这种视觉表示允许LLMs通过基于图像的学习来处理和理解代码。在这种输入形式中，LLMs从代码中的视觉模式和结构中学习，以执行代码翻译或生成代码可视化等任务。

**混合输入**通过整合多模态数据（如Token化代码与视觉化表示）为模型提供多样化的视角，从而提升代码理解能力。例如，将Python代码`def add(a, b): return a + b`的Token序列`["def", "add", "(", "a", ",", "b", ")", ":", "return", "a", "+", "b"]`与其抽象语法树（AST）或控制流图（CFG）结合，使模型既能学习细粒度的语法细节（如变量名与操作符），又能捕捉代码的整体结构（如函数嵌套与执行路径）。这种输入方式在复杂代码模式理解（如递归函数或多线程逻辑）和代码生成任务中表现尤为突出，例如通过联合Token与AST信息生成更符合语法规则的代码补全建议，或结合CFG与自然语言描述生成功能更准确的代码摘要。多模态输入的灵活性与丰富性使模型能够更全面地理解代码的语义与结构，从而在代码修复、翻译及安全分析等任务中实现更高精度。

总结来说，输入形式使用趋势显示出对基于Token的输入的强烈偏好，证明了其在各种软件工程（SE）任务中的多样性和有效性。

## Summary
开源数据集的使用最为普遍。
基于文本和基于代码的类型在应用于LLMs软件工程任务时使用最为频繁。这种模式表明，LLMs在处理软件工程任务中的文本和基于代码数据方面特别擅长，利用其自然语言处理能力。
常见的预处理步骤如数据提取、不合格数据删除、重复实例删除和数据分割。


# RQ3 Evaluation & Optimization

在LLM4SE研究中，常常需要为特定的SE下游任务调整模型。
## Fine Tuning
### 全微调
**全微调（Full Fine-tuning）​**​ 是一种在预训练模型基础上，对**所有参数**进行调整以适应特定任务的微调技术。与**部分微调**​（如仅微调部分层或使用适配器）不同，全微调会更新模型的全部参数，以更好地适应下游任务。
在研究中，BERT常使用全微调以适应下游 SE 任务。为每个下游任务分别训练和部署微调模型成本很高，因为传统的全微调方法需要对每个下游任务复制一个模型并执行全参数微调。

### 上下文学习
![[Pasted image 20250305181328.png]]
*Exp for ICL*
为模型提供手动设计的Prompt，这些提示过度依赖人类设计，根本不需要更新模型参数。
然而，ICL 仅在推理时运行，不涉及学习特定于任务的参数，实验证明，这在下游任务中对模型的改进有限




传统的全微调需要更新模型的所有参数，计算成本高且容易导致**灾难性遗忘**。为了解决这个问题，研究人员已经开始将参数高效微调 （PEFT） 技术应用于 LLMs。PEFT 旨在通过优化微调的参数子集来提高预训练模型在新任务上的性能，从而降低整体计算复杂性。这种方法将预训练模型的大部分参数保持在固定状态，将微调工作集中在一组最小但有影响力的参数上。
### LoRA微调
![[Pasted image 20250305182206.png]]
*Exp from steloCoder, a LoRA finetuned model on python PL dataset from StarCoder*
LoRA 将低秩可训练矩阵注入 Transformer 架构的注意力层，以显著减少可训练参数的数量。


### Prompt Tuning
![[Pasted image 20250305183112.png]]
提示调优（Prompt Tuning）是一种高效适配预训练模型的技术，通过在输入中添加**可学习的提示标记**​（learnable prompts）（类似T5里的context，就是学习任务指令的最优向量）引导模型优化任务性能，同时保持模型架构和内部参数固定。其核心原理是设计动态的上下文感知提示，使模型在不调整原始参数的情况下，通过优化少量附加的提示标记来适配特定任务。

微调范式是通过设计不同的目标函数，使得 LM 去迁就下游任务，但是与之相反，提示调优是通过重塑下游任务使之迁就 LM，但本质上两种范式的目的都是为了拉近 LM 和下游任务之间的距离，

例如，在代码方法命名任务中，AUMENA方法利用自动化提示调优，动态生成与代码语义相关的提示，显著提升命名的准确性和一致性。相比传统微调，提示调优仅需训练极少量参数（通常为模型总参数的0.1%-1%），大幅降低计算成本，且能有效缓解灾难性遗忘问题，适用于少样本学习、多任务迁移等场景，兼具高效性与灵活性。

### Prefix Tuning
**前缀调优（Prefix Tuning）​**​ 是一种适配预训练语言模型的技术，通过在模型的**输入层和内部层**添加**可训练的前缀标记**，影响模型的中间表示，从而实现任务特定的定制化。与提示调优（Prompt Tuning）仅在输入层添加可学习标记不同，前缀调优将可训练标记扩展到模型的多个层，更深入地引导模型的行为。这种方法**无需修改模型的原始参数**，仅通过优化前缀标记即可高效适配新任务。

### Adapter Tuning
![[Pasted image 20250305192839.png]]
**适配器调优（Adapter Tuning）​**​ 是一种高效适配预训练模型的技术，通过在模型的每一层中插入**小型神经网络模块（适配器）​**（就是插一堆MLP？），在不修改原始模型参数的情况下，仅训练这些适配器来适应特定任务。以下是适配器调优的详细介绍，包括其工作原理、实现方式和应用场景。
通过适配器调优在LLMs代码搜索和代码摘要任务中表现得非常好。
### SFT

## Prompt Engineering
![[Pasted image 20250305194602.png]]
这种方法能够LLMs仅根据给定的提示无缝集成到下游任务中，指导模型行为，而无需更新模型参数
### Few-Shot
小样本提示涉及向模型提供有限数量的示例或指令以执行特定任务。该模型从这些例子中学习，并以最少的训练数据推广到类似的任务。
### Zero-Shot
模型被期望执行一项任务，而无需对该任务进行任何明确的训练。相反，它依靠推理过程中提供的提示来生成所需的输出。(就是常用的方法。)
### CoT
![[Pasted image 20250305195513.png]]
*Exp 结构层次提取、嵌套代码块提取、嵌套代码块的 CFG 生成、以及所有嵌套代码块的 CFG 合并 CoT指导生成CFG*
思维链。-将复杂任务分解为多个子步骤，引导模型逐步生成推理链。每个提示都建立在前一个提示的基础上，形成一条连贯的推理链。通过引导模型生成结构化、逻辑性强的响应，提升其在复杂任务中的表现。

CoT变体-CoC
![[Pasted image 20250305200011.png]]
*CodeChain*
通过CoT提示生成模块化解决方案，提取并聚类子模块，然后在自我修订阶段重用或调整这些模块，以生成更高质量的代码。

AutoCoT
![[Pasted image 20250305201523.png]]
*Exp from ART*
根据输入和任务需求**自动生成提示序列**，引导模型完成复杂推理任务。Auto-CoT 的核心思想是无需人工设计提示，而是利用预定义的任务库或规则，动态生成多步推理步骤，从而提升模型在代码生成、逻辑推理等任务中的表现。

MoT
![[Pasted image 20250305201802.png]]
*Exp from MoTCoder*
***左图**：为了将传统指令转换为模块化思维（Module-of-Thought, MoT）指令，我们的 MoT 指令转换方法通过两步过程引导模型。首先，模型被指示**规划所需的子模块**，仅生成这些子模块的函数头（function headers）和描述其用途的文档字符串（docstrings）。随后的指令则引导模型**实现这些子模块**，并将它们整合为一个完整的最终解决方案。该指令还通过一个**单样本示例**​（one-shot example）进一步增强模型的理解。  
**右图**：展示了**指令**、**模块化代码**和**解决方案**的示例。*

在代码生成任务中，LLMs通常以单个代码块的形式生成解决方案，限制了它们在处理复杂问题时的有效性。研究者 提出了模块化思维编码器 （MoTCoder）。他们引入了一种新的 MoT 提示优化框架，以促进任务分解为逻辑子任务和子模块。

SCoT FOR Code
![[Pasted image 20250305202418.png]]
![[Pasted image 20250305202516.png]]

*Exp SCoT & CoT*
**结构化链式思维提示（Structured Chain-of-Thought, SCoT）​**​ 是一种专门针对**代码生成任务**设计的提示技术。与传统的链式思维提示（CoT）不同，SCoT 强调利用源代码中的**丰富结构信息**，引导大语言模型（LLMs）从源代码的角度构建中间推理步骤，从而生成更高质量的代码。
### APE
![[Pasted image 20250306130539.png]]
*Exp from PromptCS 通过训练一个提示代理（prompt agent），自动生成连续提示。*
手工设计的离散提示（discrete prompts）可能不够灵活，难以充分挖掘 LLMs 的潜力。
Automatic Prompt Engineer (APE)​ 是一种自动化生成和选择提示（prompts）的系统，灵感来源于**经典程序合成**和**人工提示工程方法**，旨在为特定任务自动生成有效的提示，从而简化提示工程的过程。

## Metrics
![[Pasted image 20250306131158.png]]
MAE（预测值&真实值的绝对误差）常用于回归任务。
对于分类任务，最常用的指标是精度、召回率和 F1 score。
在生成任务中，用的比较多的是NLP里的传统指标。BLEU （n-grams）等指标及其变体 BLEU-4 (4-grams)和 BLEU-DC(考虑相关度的n-grams) 、Pass@k是最常用的。
此外，ROUGE/ROUGE-L （计算参考文本&生成文本的n-grams重叠率）、METEOR （常用于翻译，就是引入词序惩罚的F1Score）、EM （精确匹配）（要求参考&生成完全匹配）和 ES （编辑距离，就是一个string最多通过几步单字符变换到另一个string）用于特定研究，以评估生成的代码或自然语言代码描述的质量和准确性。
## Summary
有效参数微调 （PEFT） 技术，包括低秩适应 （LoRA）、提示调优、前缀调优和适配器调优，在优化LLMs的同时最大限度地降低计算复杂性，越来越受到重视。
小样本提示、零样本提示、思维链 （CoT）、自动提示工程 （APE）、代码链 （CoC）、自动思维链 （Auto-CoT）、模块化思维 （MoT） 和结构化思维链 （SCoT），应用于 LLM4SE 领域以提高模型性能。这些技术利用特定于任务的指令（称为提示）来指导LLMs，而无需修改核心模型参数。

# RQ4 Tasks
## What Tasks？
![[Pasted image 20250306135144.png]]
在软件开发领域观察到的研究数量最多。这强调了迄今为止的主要重点是利用LLMs来增强编码和开发过程。
软件维护任务约占研究份额的 22.71%，突出了在协助软件更新和改进LLMs方面的重要性。
软件质量保证领域约占研究比例的 15.14%，表明人们对自动化测试程序的兴趣日益浓厚。
需求工程和软件设计活动分别占研究份额的约 3.9% 和 0.92%，表明迄今为止在这些领域的探索相对有限。软件管理领域的研究代表性最少，仅占 0.69% 的比例。
这种分布强调了对开发和维护任务的重要关注，同时也表明了在测试、设计和管理领域进一步研究的潜在途径。

## 需求工程中LLM解决了的问题
### **指代歧义处理**
软件需求中的歧义是指单个读者可以以多种方式理解自然语言（NL）需求，或者不同读者对同一需求有不同理解。这种歧义可能导致在后续开发阶段产生次优的软件工件。
在分析英文软件需求的研究中，ChatGPT 始终证明了其准确识别的能力。

### 需求分类
需求源自 自然语言 文档，需要有效的分类。在质量约束下，分为功能 （FR） 或非功能 （NFR） 需求。研究中有使用 BERT（读取整个单词序列以更深入地理解语言）和 K-means 聚类，为语料库中的每个术语创建和分组向量。该方法已在包含 8 个不同领域的大型计算机科学和多领域语料库上得到验证。

### 共指消解
由不同利益相关者编写的需求通常会不断演变，导致术语差异和不一致性。共指（不同表达式指向同一实体）可能引起混淆。

### 追溯
可追溯性涉及建立和维护软件工件（如需求、代码、测试用例）之间的关系，以支持产品查询和开发。T-BERT 能够有效地将知识从代码搜索迁移到 NLA-PLA（自然语言工件到编程语言工件）可追溯性，即使在训练实例有限的情况下也能表现良好。

## 软件设计中LLM解决了的问题
### ​**GUI（图形用户界面）检索**
 GUI 文档通常不是标准的、结构良好的文本，难以通过基于文本的排序任务进行处理。
 研究基于 BERT 的学习排序（LTR）模型，用于 GUI 检索任务。作者通过将自然语言查询与 GUI 文档文本拼接，训练了不同的 BERT-LTR 模型。

### **快速原型设计**
快速原型设计使开发人员能够快速可视化和迭代软件设计，从而加速开发过程并确保与用户需求的一致性。研究表明，快速原型设计领域可以从与先进机器学习技术的更深层次集成中受益，从而为更直观和以用户为中心的软件设计提供研究机会。

### ​**软件规范**
软件配置对系统行为至关重要，但随着系统规模的增大，难以管理&生成配置。
SpecSyn，一个使用 LLM 从自然语言源自动合成软件规范的框架。该端到端方法将任务视为Seq2Seq学习问题。

## 软件开发中LLM解决了的问题
### ​**代码生成**
![[Pasted image 20250306142220.png]]
LLMs可以通过将自然语言描述转换为代码来自动生成代码。这些模型从自然语言描述中生成程序代码，从而提高代码编写效率和准确性。 它们在代码补全、代码自动生成、自然语言标注转换为代码等方面表现出优异的性能，为软件开发人员提供了强大的辅助工具，进一步促进了代码编写和开发过程的自动化和智能化。
- 编程思维链 指导LLMs在深入研究详细实现之前首先制作高级代码草图，合成代码表现出更高的准确性和健壮性。
- 集成。在已建立的 SE 工具和实践集成LLMs。
### 代码补全
近年来，LLM 在生成代码片段方面表现尤为出色。这些模型通过训练海量自然语言文本，具备了强大的语义理解能力。在代码补全场景中，LLMs 如 CodeX、GitHub Copilot [70, 244, 344]、CodeParrot、GPT 系列、T5 、CodeGen [67, 68, 244, 311] ，能够基于代码上下文和语法结构生成准确且智能的代码建议。它们能够理解开发者的意图，预测下一个可能的代码片段，并根据上下文提供合适的推荐。

近期的研究还有着眼于根据开发人员的编码风格和偏好提供个性化的代码推荐。

### 代码摘要
LLM ​​在代码摘要中发挥了重要作用，通过分析代码结构和上下文生成信息丰富的自然语言摘要。具体来说，LLMs 如 Codex、CodeBERT 和 T5 能够理解代码的功能和逻辑，生成易于理解的自然语言描述。
CodeX 表现出色。在 LLMs 的支持下，代码摘要显著提高了代码的可读性，改善了软件文档的质量，并加速了开发人员之间的代码理解和协作。这种先进的代码摘要方法展示了在现代软件工程实践中利用 LLMs 自动化和简化软件开发各个环节的巨大潜力。
但专注于Code的LLM可能通用性有所欠缺。

### 代码检索
代码检索，是从大型代码库中根据用户的自然语言查询检索源代码的任务。
近年来，一些基于 BERT 的双模态预训练模型被提出，例如 ​**CodeBERT**​和 ​**GraphCodeBERT**​ 。这些双模态预训练模型通过设计预训练目标，以无监督的方式从大量数据中学习通用表示。

这些研究表明，仅靠预训练任务可能不足以满足代码搜索的需求，强调了需要对数据（包括自然语言和代码）进行多模态理解 。此外，研究表明，使用代码生成模型（如 ​**Codex**​ ）可以通过从自然语言文档生成代码片段来增强代码检索，从而提高语义相似性。

### 代码理解
代码理解则涉及对源代码的深入分析，以理解其逻辑、结构、功能和依赖关系，同时还包括对编程语言、框架和库的理解。
LLM通过其强大的自然语言处理能力，能够解释与代码相关的文本（如注释和文档），从而辅助开发人员理解代码功能、识别依赖关系并生成相关代码文档。凭借其同时理解代码和自然语言的能力，LLMs 提高了代码理解的效率和准确性，使开发人员能够更有效地维护、优化和集成代码。

### 程序合成
LLMs可以有效地解释自然语言描述、代码注释和需求（与代码生成不同的地方），然后生成满足给定规范的相应代码片段。这有助于开发人员快速构建代码原型并自动执行重复的编码任务。LLMs通过基于高级输入的自动化代码编写过程来提高生产力并减轻开发人员的负担 。它们能够理解自然语言和编程语言的细微差别。

### 表示学习
将代码语义编码到分布式向量表示中，并在基于深度学习的代码智能模型中发挥关键作用。注意力机制与代码的句法结构高度一致，预训练的代码语言模型可以在每个转换层的中间表示中保留代码的句法结构，并且预训练的代码模型具有理解代码句法树的能力。 这些发现表明，将代码的句法结构纳入预训练过程会导致更好的代码表示。

### 注释生成
使用LLM自动为源代码创建注释，旨在阐明代码功能、实现逻辑和输入输出细节，从而提高可读性和可维护性。Codex  和 T5 已被有效地应用于代码注释生成。这些模型在大量数据上进行了预训练，并拥有强大的自然语言处理和语义理解能力。在注释生成过程中，LLMs 分析源代码的结构、语义和上下文，以自动生成与代码功能逻辑相对应的高质量注释。

### 方法命名
![[Pasted image 20250306152708.png]]
Exp from CodeT5 & AUMENA
CodeT5学习编程和自然语言的上下文表示，然后利用LLMs与提示调整来检测不一致的方法名称并建议准确的替代方案,将任务建模为二分类问题。

## 测试中LLM的应用
### 测试自动化
自动化测试方法提供了一系列工具和策略，旨在评估软件应用的准确性、可靠性和性能。这些方法包括各种技术，如突变测试和Fuzzing。
LLMs用于突变测试，向代码库引入故障以评估测试套件在识别和检测错误方面的有效性。
LLMs可以帮助进行模糊测试，生成有效且多样化的输入程序，有助于识别漏洞和错误。

## 错误定位
测试套件通常包括两种类型的测试用例：通过测试用例和故障诱导测试用例。在实践中，找到故障诱导测试用例是很困难的。这是因为开发者首先需要找到触发程序故障的测试输入，而这样的测试输入的搜索空间是巨大的。此外，开发者需要构建一个测试预示来自动检测程序故障。

## 软件维护与修复
![[Pasted image 20250306165502.png]]

### 自动程序修复
CodeBERT、CodeT5、Codex、 GPT 系列，在生成语法正确且上下文相关的代码方面已显示出有效性。利用LLMs进行程序修复可以在生成各种类型错误和缺陷的补丁方面实现有竞争力的性能。
此外，LLMs可以在特定的代码修复数据集上进行微调，进一步提高它们为现实世界软件项目生成高质量补丁的能力。 

### 代码审查
代码审查是一种关键的质量保证实践，用于检查、评估和验证软件代码的质量和一致性。代码审查旨在识别潜在的错误、漏洞和代码质量问题，同时提高代码的可维护性、可读性和可扩展性。
BERT、ChatGPT和 T5，这些在大量代码仓库上训练的模型具有理解和学习代码语义、结构和上下文信息的能力。在代码审查过程中，这些模型帮助审查员全面理解代码意图和实现细节，从而更准确地检测潜在问题和错误。

### 错误复现
![[Pasted image 20250306195829.png]]
*Exp from Prompting Is All You Need*
AdbGPT，利用LLM的自然语言理解和逻辑推理能力从错误报告中提取复现步骤实体，并根据当前的图形用户界面（GUI）状态引导错误回放过程。研究人员描述了如何利用提示工程、Few-Shot和CoT来利用LLM的知识进行自动化错误回放。 

## Summary
 代码生成和程序修复是LLM辅助软件开发和维护活动中最普遍的任务。

# Challenges & Opportunities
## Challenges
目前LLMs在需求工程、软件设计和软件管理中的应用相对较少。
许多软件工程（SE）领域，包括安全关键系统和特定行业，都面临着开源数据集稀缺的问题，这阻碍了在这些专业领域的应用。
LLM的可解释性、可信度&鲁棒性研究。
确保LLMs具有良好的泛化能力需要仔细的微调、在不同数据集上的验证以及持续的反馈循环。
LLMs在训练和微调过程中严重依赖大量不同的数据集，数据的质量、多样性和数量直接影响模型的性能和泛化能力。

## Opportunities
专注于代码生成的LLM工具。
基于特定任务的再训练LLM。
整合多个LLMs 或将LLMs与专门的模型相结合，以提高其在 SE 任务中的有效性。
利用新的自然语言输入形式，如口语、图表和多媒体输入，为增强LLMs理解和处理多样化用户需求。
探索基于图的数据集。图可以捕捉代码中的结构关系和依赖，使LLMs更好地理解代码交互和依赖。
指标。
LLM4SE 的全面评估框架。