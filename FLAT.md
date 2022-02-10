## FLAT相关内容
### TTODO
- [比较有用的解读](https://zhuanlan.zhihu.com/p/362349210)
- 涉及Transformer中相对位置编码 [RPE](https://zhuanlan.zhihu.com/p/105001610)
- 采用的[transformer-xl](https://zhuanlan.zhihu.com/p/84159401)中的self-attention
- TENER: Adapting Transformer Encoder for Named Entity Recognition 给出答案： 原生Transformer中的绝对位置编码并不直接适用于NER任务。
- [代码实践](https://zhuanlan.zhihu.com/p/391560782)
- [视频梳理NER任务中引入词汇信息](https://link.zhihu.com/?target=https%3A//b23.tv/kVlK7e)
- [Lex-BERT：超越FLAT的中文NER模型？](https://zhuanlan.zhihu.com/p/343231764)

### 总结
- 目的：lattice structure，将词汇信息引入模型，解决中文NER中分词错误对结果的影响
- 主体：
	- 整体结构是按字的序列标注框架，为一层Transformer Encoder做序列编码，后接CRF解码。只是在使用Transformer获得字向量表示的过程中，通过对Transformer的改造，引入了词汇信息，具体就是将句子中匹配到的词直接添加到最后，使得self-attention过程中利用其全局注意力机制，每个字都能关注到句子中出现的增强词汇的信息。
- 亮点：
	- 较以往的动态词汇增强来说，并行程度提高很多
	- 改进Transformer的位置编码，引入相对位置信息
- 过程：
	- 具体过程结合代码 #todo


## Transformer中位置编码的[研究](https://zhuanlan.zhihu.com/p/105001610)
- 跟位置编码有关的运算在self attention
- 计算第i和第j个位置之间的attention score公式展开
- 展开式包含四个部分，第一个位置不含位置向量，第二和第三部分都只含一个位置向量，所以也不包含相对位置信息，第四部分包含两个位置向量
- 如果两个位置向量直接点乘可以证明其结果只跟位置相对值k有关
- 但是由于多头，在中间加入了一个不可知的线性变换后，就没有相对位置信息了
- 所以有几种在Transformer中加入相对位置信息的变法：
	1. [Self-Attention with Relative Position Representations](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1803.02155.pdf)
		- 既然相对位置信息是在self-attention计算时候丢失的，那么最直接的想法就是在计算self-attention的时候再加回来，具体做法是在计算attention score和weighted value时各加入一个可训练的表示相对位置的参数，并且multi head之间可以共享。
	2. [Transformer-XL: Attentive Language Models Beyond a Fixed-Length Context](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1901.02860.pdf)
		- 将展开公式的各部分进行改写
	3. Encoding word order in complex embeddings（openreview）
