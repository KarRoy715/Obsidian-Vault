一、模型
- 2.8T，MOE-104B激活参数（16/896激活路由），原生视觉能力，1m上下文
- 基于Kimi Delta Attention and Attention Residuals，Stable LatentMoE
- 16 of 896 routed experts per token

三、 预训练
- 文本数据：每个领域通过基于规则的启发式、基于分类器的质量评分和重复数据删除的组合进行过滤，并通过对较小模型的消融研究确定特定于领域的采样率
- 视觉数据：视觉语料遵循kimi k2.5的taxonomy，将开源数据集与用于过滤、合成和消重的内部管道相结合。

 - 3.1-预训练数据
	 - 文本：web、code、math、knowledge
		 - 对文本部分的数据进行了数据增强和合成数据
	 - 多模态：caption、interleaved image-text document、OCR、perception、video、visual coding 
		 - 坐标包含绝对和归一化的形式	
- 3.2-Scaling Law
	- 通过消融实验进行超参数的选择
- 3.3-Training Recipe
	- 原生多模态，从训练开始就联合优化语言和视觉，不是通过后续的对齐阶段，将视觉编码器和语言模型对齐
- 3.4-Long-Context Extension
	- Positional encoding：NoPE
	- 长文本数据的扩展：
	- 渐进式扩展：预训练8k token-> 64k，冷启动阶段256k - 1M

四、后训练
- 4.1-方法：三阶段框架， 通过SFT初始化agent基础能力