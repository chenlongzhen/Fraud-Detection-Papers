## Contrastive Learning 对比学习

### 综述

- [对比学习（Contrastive Learning）相关进展梳理](https://zhuanlan.zhihu.com/p/141141365)
- [对比学习（Contrastive Learning）最新综述](https://zhuanlan.zhihu.com/p/410442591)
- [Contrastive Self-Supervised Learning](https://ankeshanand.com/blog/2020/01/26/contrative-self-supervised-learning.html)
- [[ICLR2021] 对比学习（Contrastive Learning）NLP进展梳理](https://zhuanlan.zhihu.com/p/396430548)
- [对比学习（Contrastive Learning）在CV与NLP中的研究进展](https://zhuanlan.zhihu.com/p/389064413)

Self-supervised Learning 不需要标签信息。通过定义规则作为监督信号去训练模型。目的是学到的特征能够使其和其他样本区别开来，类似的样本在特征空间里的相邻区域，不类似样本都在不相邻区域。

- Pretext Tasks - 学习好的特征
- Loss Functions - 定义目标函数 on-the-fly
  - Contrastive Loss：特征空间里衡量各个样本对的相似性
  - 正例pair和负例pair隔开至少 $\eta$ 的距离
- 如何构建正例和负例: anchor, postive sample, negative sample
  - 设计出合理的正例和负例pair，并且尽可能提升pair能够cover的semantic relation，才能让得到的表示在downstream task表现的更好。
  - Instance Discrimination

**Contrastive Learning Framework**
- 学习一个映射函数f，把样本x编码成其表示f(x), 使得

 ![image](https://user-images.githubusercontent.com/46979228/190882819-ea6f469b-efce-4302-94b2-36819eaea3b9.png)
- What you need?
  - 距离度量函数: 判断两个向量在投影空间里的距离远近
  - Loss Function

### Theory & Application

- [A Theoretical Analysis of Contrastive Unsupervised Representation Learning, 2019](https://arxiv.org/pdf/1902.09229.pdf)
- [Contrastive Learning of Structured World Models, 2020](https://arxiv.org/pdf/1911.12247.pdf)
- [Understanding Contrastive Representation Learning through Alignment and Uniformity on the Hypersphere, 2022](https://arxiv.org/pdf/2005.10242.pdf)

### CV

- [[InstDisc] Unsupervised Feature Learning via Non-Parametric Instance Discrimination, 2018](https://arxiv.org/pdf/1805.01978.pdf)
- [[CPC] Representation Learning with Contrastive Predictive Coding, 2019](https://arxiv.org/pdf/2010.15464.pdf)
- [[CMC] Contrastive Multiview Coding, 2020](https://arxiv.org/pdf/1906.05849.pdf)
- [What Makes for Good Views for Contrastive Learning?](https://proceedings.neurips.cc/paper/2020/file/4c2e5eaae9152079b9e95845750bb9ab-Paper.pdf)

基于负例对比学习:
- [[MoCo] Momentum Contrast for Unsupervised Visual Representation Learning, 2020](https://arxiv.org/pdf/1911.05722.pdf)
  - 用dynamic dictionary提供稳定的自监督信号，让正负样本有效对比，预训练学到好的特征迁移到downstream task。
  - InfoNCE Loss
  - [MoCo 精读](https://www.bilibili.com/video/BV1C3411s7t9/?spm_id_from=333.788)
- [[SimCLR] A Simple Framework for Contrastive Learning of Visual Representations, 2020](https://arxiv.org/pdf/2002.05709.pdf)
  - Data augmentation & MLP
  

基于聚类对比学习:
- [[DeepClustering] Deep Clustering for Unsupervised Learning of Visual Features, 2019](https://arxiv.org/pdf/1807.05520.pdf)
- [[SwAV] Unsupervised Learning of Visual Features by Contrasting Cluster Assignments, 2021](https://arxiv.org/pdf/2006.09882.pdf)
  - "Swapped" prediction

不用负样本对比学习：
- [[BYOL] Bootstrap Your Own Latent A New Approach to Self-Supervised Learning, 2020](https://arxiv.org/pdf/2006.07733.pdf)
- [Understanding Self-Supervised and Contrastive Learning with "Bootstrap Your Own Latent" (BYOL)](https://generallyintelligent.ai/blog/2020-08-24-understanding-self-supervised-contrastive-learning/)
  - Batch norm: all examples compared with mode
- [[BYOL] BYOL works even without batch statistics, 2020](https://arxiv.org/pdf/2010.10241.pdf)
- [[SimSiam] Exploring Simple Siamese Representation Learning, 2020](https://arxiv.org/pdf/2011.10566.pdf)

Vision Transformer
- [[DINO] Emerging Properties in Self-Supervised Vision Transformers, 2021](https://arxiv.org/pdf/2104.14294.pdf)

Prototypical Contrastive Learning
- [[PCL] Prototypical Contrastive Learning of Unsupervised Representations, 2021](https://arxiv.org/pdf/2005.04966.pdf)
  - ProtoNCE loss
- [[HCSC] HCSC: Hierarchical Contrastive Selective Coding, 2022](https://arxiv.org/pdf/2202.00455.pdf)

### NLP

利用对比学习思想，自监督训练sentence-embedding。运用Contrastive Learning思想主要分为两类：1）损失联合优化；2）构造增强样本，fine-tune模型；
- [[SimCSE] SimCSE: Simple Contrastive Learning of Sentence Embeddings, 2022](https://arxiv.org/pdf/2104.08821.pdf)

**损失联合优化**

将Contrastive Learning的loss和其他loss混合
- [[CLEAR] Contrastive LEArning for sentence Representation](https://arxiv.org/pdf/2012.15466.pdf)


**文本生成**


- [文本生成基础与方案梳理](https://zhuanlan.zhihu.com/p/162035103)
- [Nucleus Sampling与不同解码策略](https://zhuanlan.zhihu.com/p/442557114)
  - 无论top-k/top-p还是temprature sampling都是在优化解码词概率分布，通过re-scaling，尽量让生成的句子多样性更好，同时不损失通顺度。
  - 重新归一化后,虽然之前概率最大的词现在概率值还是最大，但是整体概率分布会更加均匀，这样别的词被采样到的概率会稍微大一点。

seq2seq, 由encoder和decoder组成。先将输⼊映射到一个序列，然后通过序列解码，解码的时候当前值依赖与隐藏节点和解码出来的上一个节点。由于seq2seq的loss采用最大似然求解，必然容易导致偏置问题，生成的内容重复且单一， 如果想要模型生成出更多有趣多样的内容，就需要复杂的数据过滤，采样技术等。

- [Contrastive Learning with Adversarial Perturbations for Conditional Text Generation, 2021](https://arxiv.org/pdf/2012.07280.pdf)
  - 解决“exposure bias”: model is exposed to various valid or incorrect perturbations of the inputs, for improved generalization
- [[SimCTG] A Contrastive Framework for Neural Text Generation, 2022](https://arxiv.org/pdf/2202.06417.pdf)
  - Motivation: 在文本生成中，用MLE去decode经常会出现重复性Token，影响文本质量，在长文本中可能尤其明显。
  - Method: 提出contrastive search—to encourage diversity while maintaining coherence in the generated text. 使用 Token Similarity Matrix (the token similarity matrix should be sparse and the representations of distinct tokens should be discriminative)。
  - Loss: $L_{SimCTG}$ = $L_{MLE}$ + $L_{CL}$
  - [Bert中词向量的各向异性Anisotropic](https://www.zhihu.com/question/460991118) 一个好的向量表示应该同时满足Alignment和Uniformity。

**NER**
- [CONTAINER: Few-Shot Named Entity Recognition via Contrastive Learning, 2022](https://arxiv.org/pdf/2109.07589.pdf)

**NCE**
- [Understanding Hard Negatives in Noise Contrastive Estimation, 2021](https://arxiv.org/pdf/2104.06245.pdf)

### Loss

**NCE(Noise Contrastive Estimation) Loss**

NCE通过学习数据分布样本和噪声分布样本之间的区别，从而发现数据中的一些特性。
- Data Sample vs Noise Sample
- 如果负样本的数量很多，使用NCE的变种InfoNCE 


### 其他

- [推荐系统中的对比学习（Contrastive Learning）方法](https://zhuanlan.zhihu.com/p/405117499)
- [对比学习论文实现](https://zhuanlan.zhihu.com/p/408156196)
