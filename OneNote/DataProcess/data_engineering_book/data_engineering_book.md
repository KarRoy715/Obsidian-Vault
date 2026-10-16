---
onenote-id: 82a6f13c-ae3b-00e6-260e-ccc6aa154546:1
---
Chinchilla 论文给出的"最优配比"建议是：==🟡每增加== ==🟡1== ==🟡个参数，应配套约== ==🟡20== ==🟡个== ==🟡Token== ==🟡的训练数据==。这意味着训练一个 7B 模型，理论上需要约 140B Token 的高质量语料——这个数字远远超出了许多团队最初的预期。

来自 \<https://datascale-ai.github.io/data_engineering_book/part1/1_1_data_change/>

而多模态时代，数据格式爆炸式增长：图片有 JPEG、PNG、WebP；视频有 MP4、AVI、MKV；音频有 WAV、MP3、FLAC；文档有 PDF、Word、HTML。每种格式都有其独特的解析方式和质量评估标准

来自 \<https://datascale-ai.github.io/data_engineering_book/part1/1_1_data_change/>

| **维度** | **纯文本时代** | **多模态时代** |
| --- | --- | --- |
| **数据格式** | 统一的 UTF-8 文本 | 图片、视频、音频、文档、网页等多种格式 |
| **存储需求** | TB 级 | PB 级（图片/视频占主导） |
| **对齐难度** | 无（纯文本无需对齐） | 极高（图文对齐、音视频同步、跨模态语义一致性） |
| **清洗工具链** | 成熟（FastText、KenLM） | 碎片化（每种模态都有独立工具链） |
| **质量评估** | 困惑度、去重率 | 美学评分、CLIP-Score、OCR 准确率、ASR 错误率等 |

来自 \<https://datascale-ai.github.io/data_engineering_book/part1/1_1_data_change/>

从工程实践的角度，**多模态数据处理需要模块化设计****——****每种模态独立处理后再进行跨模态对齐**。同时，**应优先投资于统一的数据格式标准，如** **WebDataset****、****TFDS** **等**，以降低下游处理的复杂度。**建立模态感知的质量评估体系也至关重要：图片需要美学评分和** **CLIP** **相似度，语音需要** **ASR** **准确率和说话人分离质量，****PDF** **需要** **OCR** **准确率和版面分析精度。**

来自 \<https://datascale-ai.github.io/data_engineering_book/part1/1_1_data_change/>

因此，数据处理流水线必须设计为可重复执行、可增量更新、可回滚版本。第二章将详细介绍如何使用 DVC、LakeFS 等工具实现数据版本控制。

来自 \<https://datascale-ai.github.io/data_engineering_book/part1/1_1_data_change/>

Common Crawl 官方文档（commoncrawl.org/the-data）详细介绍了数据格式和获取方式。RefinedWeb 论文（Falcon LLM: A Large Language Model for High-Quality Web Data）详细记录了从 Common Crawl 构建高质量预训练集的完整流程。The Pile 论文（The Pile: An 800GB Dataset of Diverse Text for Language Modeling）介绍了多源混合的数据构建策略。Trafilatura 文档（trafilatura.readthedocs.io）提供了全面的 API 说明和使用示例。The Stack 论文（StarCoder: May the Source Be with You!）介绍了大规模代码数据集的构建方法。

来自 \<https://datascale-ai.github.io/data_engineering_book/part2/2_1_data_acquisition/#33>

# **多模态数据工程**

**Ch08****：**

- 多模态数据组成
- 图文对：仅能让模型具有识物能力，没有复杂推理能力
- 代表开源集：LAION-5B、COYO-700M
- 交错图文：让模型具备复杂上下文中的“多图关联推理”能力
- 代表开源集：OVELICS、MMC4
- 长文档理解与截图：B端商业落地需求，自然图像无法满足，需要高分辨率文档数据
- 使用场景：依赖高分辨率分块（Patching）与 OCR 辅助。主要用于 SFT（监督微调）阶段，训练模型完成精密的字段提取与版面结构理解（如公式与图表的指代）。

**Ch09****：**