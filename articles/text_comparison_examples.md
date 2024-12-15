# 文本比较示例

可以使用 [OpenAI API 的嵌入端点](https://beta.openai.com/docs/guides/embeddings) 来测量文本片段之间的相关性或相似性。

通过利用 GPT-3 对文本的理解，这些嵌入在无监督学习和迁移学习环境下的基准测试中 [达到了最先进的结果](https://arxiv.org/abs/2201.10005)。

嵌入可以用于语义搜索、推荐、聚类分析、近似重复检测等。

有关更多信息，请阅读 OpenAI 的博客公告：

- [引入文本和代码嵌入（2022年1月）](https://openai.com/blog/introducing-text-and-code-embeddings/)
- [新的和改进的嵌入模型（2022年12月）](https://openai.com/blog/new-and-improved-embedding-model/)

要与其他嵌入模型进行比较，请参见 [Massive Text Embedding Benchmark (MTEB) 排行榜](https://huggingface.co/spaces/mteb/leaderboard)

## 语义搜索

嵌入可以单独用于搜索，也可以作为更大系统中的一个特征。

使用嵌入进行搜索的最简单方法如下：

- 在搜索之前（预计算）：
  - 将您的文本语料库分割成小于令牌限制的块（`text-embedding-3-small` 的令牌限制为 8,191）
  - 嵌入每个文本块
  - 将这些嵌入存储在您自己的数据库中，或者在向量搜索提供商如 [Pinecone](https://www.pinecone.io)、[Weaviate](https://weaviate.io) 或 [Qdrant](https://qdrant.tech) 中
- 在搜索时（实时计算）：
  - 嵌入搜索查询
  - 在您的数据库中找到最接近的嵌入
  - 返回顶部结果

关于如何使用嵌入进行搜索的示例，请参见 [Semantic_text_search_using_embeddings.ipynb](../examples/Semantic_text_search_using_embeddings.ipynb)。

在更高级的搜索系统中，嵌入的余弦相似度可以用作多个特征之一来排序搜索结果。

## 问答

从 GPT-3 获取可靠诚实答案的最佳方式是提供它能够定位正确答案的源文档。使用上面的语义搜索程序，您可以以低成本在文档语料库中搜索相关信息，然后通过提示将其提供给 GPT-3 来回答问题。我们在 [Question_answering_using_embeddings.ipynb](../examples/Question_answering_using_embeddings.ipynb) 中演示了这一点。

## 推荐

推荐与搜索非常相似，只是输入不是自由形式的文本查询，而是集合中的项目。

关于如何使用嵌入进行推荐的示例，请参见 [Recommendation_using_embeddings.ipynb](../examples/Recommendation_using_embeddings.ipynb)。

类似于搜索，这些余弦相似度分数可以单独用来对项目进行排序，也可以作为更大排序算法中的特征。

## 自定义嵌入

虽然不能微调 OpenAI 的嵌入模型权重，但您仍然可以使用训练数据来定制嵌入以适应您的应用程序。

在 [Customizing_embeddings.ipynb](../examples/Customizing_embeddings.ipynb) 中，我们提供了一个使用训练数据定制嵌入的示例方法。该方法的思路是训练一个自定义矩阵来乘以嵌入向量，以获得新的定制嵌入。通过良好的训练数据，这个自定义矩阵将有助于强调与您的训练标签相关的特征。您可以等效地将矩阵乘法视为（a）嵌入的修改或（b）用于测量嵌入之间距离的距离函数的修改。
