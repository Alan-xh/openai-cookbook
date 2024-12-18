# 什么让文档变得优秀

文档将有用的信息放入他人的头脑中。遵循这些建议来撰写更好的文档。

### 使文档易于浏览

很少有读者会从上到下线性阅读。他们会跳来跳去，试图评估哪个部分解决了他们的问题（如果有的话）。为了减少他们的搜索时间并增加成功的机会，使文档易于浏览。

**将内容分割成带标题的部分。** 部分标题就像路标，告诉读者是否需要深入或跳过。

**优先使用具有信息性句子的标题而不是抽象名词。** 例如，如果你使用“结果”这样的标题，读者需要跳到后面的文本中了解实际结果是什么。相反，如果你用“流式处理将首个令牌的时间减少了50%”这样的标题，它立即向读者提供了信息，无需额外的跳转。

**包含目录。** 目录帮助读者更快地找到信息，就像哈希表比链表有更快的查找速度一样。目录还有一个经常被忽视的第二个好处：它们为读者提供了关于文档的线索，帮助他们判断是否值得阅读。

**保持段落简短。** 较短的段落更易于浏览。如果你有一个关键点，考虑将其放在一个单句段落中以减少被忽略的可能性。长段落可能会埋没信息。

**以简短的题目句开始段落和部分，这些句子可以独立预览。** 当人们浏览时，他们会不成比例地关注段落的第一个单词、第一行和第一句话。写这些句子时不要依赖前文。例如，考虑第一个句子“在此基础上，让我们现在谈谈一个更快的方法。”这个句子对于没有读过前一个段落的人来说是没有意义的。相反，用一种可以独立理解的方式来写它：例如，“向量数据库可以加速嵌入搜索。”

**在题目句的开头放置主题词。** 读者在只需读一两个词就能知道段落主题时，浏览效率最高。因此，在撰写主题句时，优先将主题放在句子的开头而不是结尾。例如，假设你正在写一篇关于嵌入搜索的长文章中的向量数据库段落。不要写“嵌入搜索可以通过向量数据库加速”，而是写“向量数据库加速嵌入搜索。”后者更适合浏览，因为它将段落主题放在了段落的开头。

**将关键信息放在前面。** 将最重要的信息放在文档和部分的顶部。不要写苏格拉底式的长篇大论。在介绍你的方法之前不要先介绍结果。

**使用项目符号和表格。** 项目符号列表和表格使文档更易于浏览。经常使用它们。

**加粗重要文本。** 不要害怕加粗重要文本以帮助读者找到它。

### 写得好

写得差的文本读起来很费力。通过写得好来尽量减少读者的负担。

**保持句子简单。** 将长句分成两句。删掉副词。删除不必要的词和短语。如果适用，使用命令式语气。按照写作书籍的建议做。

**写出可以无None歧义解析的句子。** 例如，考虑句子“用句子标题部分。”当读者读到“标题”这个词时，他们的大脑还不知道“标题”是名词、动词还是形容词。解析剩余的句子时需要一些脑力，并可能造成停顿，如果他们的大脑预测错了意思。优先选择更容易解析的句子（例如，“将部分标题写成句子”），即使它更长。同样，避免像“自行车清理练习通知”这样的名词短语，因为它们需要额外的努力来解析。

**避免左分支句子。** 语言学树显示了句子中单词之间的关系。左分支树比右分支句子需要读者在记忆中保持更多信息，类似于广度优先搜索和深度优先搜索之间的区别。一个左分支句子的例子是“你需要面粉、鸡蛋、牛奶、黄油和一点盐来做None煎饼。”在这个句子中，你直到句子末尾才知道‘你需要’连接到什么。一个更易读的右分支版本是“为了做None煎饼，你需要面粉、鸡蛋、牛奶、黄油和一点盐。”注意那些读者必须记住的句子，看看是否可以重新表述。

**避免使用指示代词（例如，“这”），尤其是跨句的。** 例如，不要说“基于我们之前的话题讨论，现在让我们讨论函数调用”，而是说“基于消息格式化，现在让我们讨论函数调用。”第二句话更易理解，因为它不让读者去回忆前一个话题。寻找机会完全去掉指示代词：例如，“现在让我们讨论函数调用。”

**保持一致性。** 人类大脑是惊人的模式匹配器。不一致会让读者感到恼火或分心。如果我们到处使用标题大写，就使用标题大写。如果我们到处使用终止None逗号，就使用终止None逗号。如果所有Cookbook笔记本都是用下划线和句子大写命名的，就使用下划线和句子大写。不要做任何会让读者去‘嗯，这很奇怪’的事情。帮助他们专注于内容，而不是其不一致性。

**不要告诉读者他们在想什么或该做什么。** 避免像“现在你可能想了解如何调用函数”或“接下来，你需要学习如何调用函数”这样的句子。这两句话都假定了读者的心理状态，这可能会让他们恼火或降低我们的可信度。使用不假定读者状态的短语。例如，“要调用函数，…”

### 广泛有用

人们来阅读文档时有不同的知识水平、语言熟练度和耐心。即使我们针对经验丰富的开发者，我们也应该努力写出对每个人都有帮助的文档。

**简单写作。** 比你认为需要的还要简单地解释事情。许多读者可能不是以英语为母语。许多读者可能对技术术语感到非常困惑，并且几乎没有多余的脑力来解析英语句子。写得简单。（但不要过度简化。）

**避免使用缩写。** 把事情写出来。对专家的成本低，对初学者的好处大。不要写IF，而要写instruction following。不要写RAG，而要写retrieval-augmented generation（或者我更喜欢的术语：搜索-询问程序）。

**提供可能问题的解决方案。** 即使95%的读者知道如何安装Python包或保存环境变量，提前解释这些问题也是值得的。包含解释对专家来说不是成本——他们可以直接略过。但不包含解释对初学者来说是有成本的——他们可能会卡住甚至放弃我们。记住，即使是一个经验丰富的JavaScript工程师或C++工程师也可能在Python方面是初学者。在解释过多而不是太少之间保持平衡。

**优先使用具体准确的术语。** 行话不好。优化文档以适应新进入该领域的人，而不是我们自己。例如，不要写“提示”，而写“输入。”或者不要写“上下文限制”，而写“最大令牌限制。”后者更容易理解，并且可能比基本模型时期发展出的行话更好。

**保持代码示例通用且可导出。** 在代码展示中，尽量减少依赖。不要让用户安装额外的库。不要让他们不得不来回参考不同的页面或部分。尽量使示例简单且自包含。

**根据价值优先排序主题。** 覆盖常见问题（例如，如何计算令牌）的文档比覆盖罕见问题（例如，如何优化一个表情符号数据库）的文档价值大得多。相应地进行优先排序。

**不要教坏习惯。** 如果API密钥不应该存储在代码中，永远不要分享一个在代码中存储API密钥的例子。

**以宽泛的开头引入话题。** 例如，如果解释如何编程一个好的推荐系统，考虑通过简要提及推荐在网络中广泛存在，从YouTube视频到Amazon商品再到维基百科，来开始。用一个宽泛的开头来None奠定一个狭None窄的话题基础，可以帮助人们在进入不确定的领域之前感到更安全。如果文本写得好，那些已经知道的人也可能会喜欢它。

### 当你有充分理由时，可以打破这些规则

最终，做你认为最好的。文档是同理心的练习。把自己放在读者的位置上，做你认为对他们最有帮助的事。