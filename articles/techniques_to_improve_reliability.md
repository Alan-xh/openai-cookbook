
question = r"""
将以下文本翻译成中文，切记保持md格式

# Techniques to improve reliability

When GPT-3 fails on a task, what should you do?

- Search for a better prompt that elicits more reliable answers?
- Invest in thousands of examples to fine-tune a custom model?
- Assume the model is incapable of the task, and move on?

# 提高可靠性的技巧

当GPT-3在某个任务上失败时，你应该做什么？

- 寻找一个更好的提示以引导出更可靠的答案？
- 投资成千上万的例子来微调一个自定义模型？
- 假设模型无法完成任务，继续前进？

没有简单的答案——这取决于具体情况。然而，如果你的任务涉及逻辑推理或复杂性，考虑尝试本文中介绍的技巧来构建更可靠、高性能的提示。

## 为什么GPT-3在复杂任务上失败

如果你被要求计算13乘以17，你的脑海中会立即浮现出答案吗？对于大多数人来说，可能不会。然而，这并不意味着人类无法进行两位数的乘法。花几秒钟，用一些笔和纸计算，13 x 17 = 130 + 70 + 21 = 221并不是太费力。

同样地，如果你给GPT-3一个任务，这个任务的复杂度超出了它在计算下一个token所需的时间，它可能会编造出一个错误的猜测。然而，就像人类一样，这并不一定意味着模型无法完成这个任务。给它一些时间和空间进行推理，模型仍然可能可靠地给出答案。

例如，如果你问`gpt-3.5-turbo-instruct`以下关于杂None耍球的数学问题，它会给出错误的回答：

```gpt-3.5-turbo-instruct
Q: A juggler has 16 balls. Half of the balls are golf balls and half of the golf balls are blue. How many blue golf balls are there?
A:
```

```gpt-3.5-turbo-instruct
There are 8 blue golf balls.
```

这是否意味着GPT-3不能处理简单的数学问题？不；实际上，通过使用`Let's think step by step`提示，模型可以可靠地解决问题：

```gpt-3.5-turbo-instruct
Q: A juggler has 16 balls. Half of the balls are golf balls and half of the golf balls are blue. How many blue golf balls are there?
A: Let's think step by step.
```

```gpt-3.5-turbo-instruct
There are 16 balls in total.
Half of the balls are golf balls.
That means that there are 8 golf balls.
Half of the golf balls are blue.
That means that there are 4 blue golf balls.
```

当然，仅从一个例子很难判断这个`Let's think step by step`的技巧是否在一般情况下有效，或者只是在这个特定的问题上碰巧成功了。但它确实有效。在一个词语数学问题的基准测试中，`Let's think step by step`技巧显著提高了GPT-3的解题率，从毫无价值的18%提高到相当不错的79%！

## 模型能力取决于上下文

在学习与GPT-3协作时，一个常见的概念性错误是认为它的能力在所有上下文中都是固定的。例如，如果GPT-3在一个简单的逻辑问题上回答错误，那么它一定无法处理简单的逻辑。

但正如`Let's think step by step`的例子所示，GPT-3的明显失败有时可以通过更好的提示来纠正，帮助模型引导自己走向正确的输出。

## 如何提高复杂任务上的可靠性

本文的其余部分将分享一些技巧，以提高大型语言模型在复杂任务上的可靠性。虽然有些技巧是针对特定类型的问题，但许多都是基于可以应用于广泛任务的一般原则，例如：

- 提供更清晰的指示
- 将复杂任务分解成更简单的子任务
- 结构化指令以保持模型专注于任务
- 提示模型在回答前解释
- 要求对多种可能的答案进行理由分析，然后综合
- 生成多个输出，然后使用模型选择最佳的一个
- 微调自定义模型以最大化性能

## 将复杂任务分解为更简单的任务

一种让模型有更多时间和空间思考的方法是将任务分解成更简单的部分。

例如，考虑一个任务，我们问模型一个关于文本的多项选择题——在这种情况下，是一个线索游戏。当直接询问时，`gpt-3.5-turbo-instruct`无法将线索3和5联系起来，回答错误：

```gpt-3.5-turbo-instruct
Use the following clues to answer the following multiple-choice question.

Clues:
1. Miss Scarlett was the only person in the lounge.
2. The person with the pipe was in the kitchen.
3. Colonel Mustard was the only person in the observatory.
4. Professor Plum was not in the library nor the billiard room.
5. The person with the candlestick was in the observatory.

Question: Was Colonel Mustard in the observatory with the candlestick?
(a) Yes; Colonel Mustard was in the observatory with the candlestick
(b) No; Colonel Mustard was not in the observatory with the candlestick
(c) Unknown; there is not enough information to determine whether Colonel Mustard was in the observatory with the candlestick

Solution:
```

```gpt-3.5-turbo-instruct
(c) Unknown; there is not enough information to determine whether Colonel Mustard was in the observatory with the candlestick
```

虽然线索3和5表明Colonel Mustard是唯一在天文台的人，而且天文台里的人有烛台，模型却未能将它们结合起来给出正确的答案(a) Yes。

然而，不是直接询问答案，我们可以将任务分成三部分：

- 首先，一一审查线索，考虑线索是否可能相关
- 其次，结合相关线索推理出问题的答案
- 第三，写出最终答案：(a)、(b)或(c)

```gpt-3.5-turbo-instruct
Use the following clues to answer the following multiple-choice question, using the following procedure:
(1) First, go through the clues one by one and consider whether the clue is potentially relevant
(2) Second, combine the relevant clues to reason out the answer to the question
(3) Third, map the answer to one of the multiple choice answers: either (a), (b), or (c)

Clues:
1. Miss Scarlett was the only person in the lounge.
2. The person with the pipe was in the kitchen.
3. Colonel Mustard was the only person in the observatory.
4. Professor Plum was not in the library nor the billiard room.
5. The person with the candlestick was in the observatory.

Question: Was Colonel Mustard in the observatory with the candlestick?
(a) Yes; Colonel Mustard was in the observatory with the candlestick
(b) No; Colonel Mustard was not in the observatory with the candlestick
(c) Unknown; there is not enough information to determine whether Colonel Mustard was in the observatory with the candlestick

Solution:
(1) First, go through the clues one by one and consider whether the clue is potentially relevant:
```

```gpt-3.5-turbo-instruct
Clue 1: Miss Scarlett was the only person in the lounge. This clue is not relevant.
Clue 2: The person with the pipe was in the kitchen. This clue is not relevant.
Clue 3: Colonel Mustard was the only person in the observatory. This clue is relevant.
Clue 4: Professor Plum was not in the library nor the billiard room. This clue is not relevant.
Clue 5: The person with the candlestick was in the observatory. This clue is relevant.

(2) Second, combine the relevant clues to reason out the answer to the question:
Clue 3 and Clue 5 both say that the person with the candlestick was in the observatory. Colonel Mustard was the only person in the observatory, so Colonel Mustard was the person with the candlestick.

(3) Third, write the final answer:
(a) Yes; Colonel Mustard was in the observatory with the candlestick
```

通过给模型更多的时间和空间进行思考，并引导它沿着推理计划进行，它能够找出正确的答案(a) Yes。

将复杂指令分解成较小的子任务的另一个好处是，它可以帮助模型保持对每个子任务的专注。

例如，如果我们要求`gpt-3.5-turbo-instruct`用文本的原始语言总结文本，模型可能会退回到英语：

```gpt-3.5-turbo-instruct
Summarize the text using the original language of the text. The summary should be one sentence long.

Text:
"
La estadística (la forma femenina del término alemán Statistik, derivado a su vez del italiano statista, "hombre de Estado")​ es una ciencia que estudia la variabilidad, colección, organización, análisis, interpretación, y presentación de los datos, así como el proceso aleatorio que los genera siguiendo las leyes de la probabilidad.​ La estadística es una ciencia formal deductiva, con un conocimiento propio, dinámico y en continuo desarrollo obtenido a través del método científico formal. En ocasiones, las ciencias fácticas necesitan utilizar técnicas estadísticas durante su proceso de investigación factual, con el fin de obtener nuevos conocimientos basados en la experimentación y en la observación. En estos casos, la aplicación de la estadística permite el análisis de datos provenientes de una muestra representativa, que busca explicar las correlaciones y dependencias de un fenómeno físico o natural, de ocurrencia en forma aleatoria o condicional.
"

Summary:
```

```gpt-3.5-turbo-instruct
The text explains that statistics is a science that studies the variability, collection, organization, analysis, interpretation, and presentation of data, as well as the random process that generates them following the laws of probability.
```

然而，如果我们首先要求模型识别文本的语言，然后再进行总结，它的可靠性会更高：

```gpt-3.5-turbo-instruct
First, identify the language of the text. Second, summarize the text using the original language of the text. The summary should be one sentence long.

Text:
"
La estadística (la forma femenina del término alemán Statistik, derivado a su vez del italiano statista, "hombre de Estado")​ es una ciencia que estudia la variabilidad, colección, organización, análisis, interpretación, y presentación de los datos, así como el proceso aleatorio que los genera siguiendo las leyes de la probabilidad.​ La estadística es una ciencia formal deductiva, con un conocimiento propio, dinámico y en continuo desarrollo obtenido a través del método científico formal. En ocasiones, las ciencias fácticas necesitan utilizar técnicas estadísticas durante su proceso de investigación factual, con el fin de obtener nuevos conocimientos basados en la experimentación y en la observación. En estos casos, la aplicación de la estadística permite el análisis de datos provenientes de una muestra representativa, que busca explicar las correlaciones y dependencias de un fenómeno físico o natural, de ocurrencia en forma aleatoria o condicional.
"

Language:
```

```gpt-3.5-turbo-instruct
Spanish

La estadística es una ciencia que estudia la variabilidad, colección, organización, análisis, interpretación, y presentación de los datos, así como el proceso aleatorio que los genera siguiendo las leyes de la probabilidad.
```


question = r"""
将以下文本翻译成中文，切记保持md格式

## Prompt the model to explain before answering

Another powerful technique for improving the reliability of answers is to prompt the model to gradually reason out the answer rather than jumping immediately to the final answer. By 'thinking aloud' the model can be far more likely to arrive at the correct answer.

### Zero-shot

#### Method

## 在回答前提示模型进行解释

提高答案可靠性的另一个强大技术是提示模型在立即跳到最终答案之前逐步推理出答案。通过“思考出声”，模型更有可能得出正确的答案。

### 零样本

#### 方法

由[Takeshi Kojima等人在2022年发表](https://arxiv.org/abs/2205.11916)，提示模型进行推理的最简单方法是在答案前添加“Let's think step by step.”（让我们一步一步来思考）。图2展示了一个例子：

[![零样本推理示例](/images/zero-shot_reasoners_fig2.png)
<br>来源：Takeshi Kojima等人的《大型语言模型是零样本推理者》（2022）。](https://arxiv.org/abs/2205.11916)

#### 结果

将这个简单技巧应用于MultiArith数学数据集时，作者发现“Let's think step by step”使准确率提高了四倍，从18%提高到79%！

[![零样本推理示例](/images/zero-shot_reasoners_tab5.png)
<br>来源：Takeshi Kojima等人的《大型语言模型是零样本推理者》（2022）。](https://arxiv.org/abs/2205.11916)

#### 影响

虽然“Let's think step by step”这个技巧在数学问题上效果很好，但并非在所有任务上都有效。作者发现，它对于多步算术问题、符号推理问题、策略问题和其他推理问题最有帮助。它对简单的数学问题或常识性问题没有帮助，显然对许多其他非推理任务也没有帮助。

[![零样本推理示例](/images/zero-shot_reasoners_tab1.png)
<br>来源：Takeshi Kojima等人的《大型语言模型是零样本推理者》（2022）。](https://arxiv.org/abs/2205.11916)

要了解更多，请阅读[完整论文](https://arxiv.org/abs/2205.11916)。

如果您将此技术应用于自己的任务，不要害怕尝试定制指令。“Let's think step by step”是相当通用的，因此您可能会发现通过定制更严格的格式来获得更好的性能。例如，您可以尝试更结构化的变体，如“首先，逐步思考为什么X可能是真的。其次，逐步思考为什么Y可能是真的。第三，逐步思考X还是Y更有道理。”。您甚至可以为模型提供一个示例格式来帮助它保持正确的轨道，例如：

```gpt-3.5-turbo-instruct
使用以下IRS指南，按照这个格式回答以下问题：
(1) 对于每个标准，确定它是否由车辆购买满足
- {标准} 让我们逐步思考。{解释} {是或否，或者如果问题不适用则N/A}。
(2) 在逐个考虑每个标准后，将最终答案表述为“由于{原因}，答案很可能是{是或否}。”

IRS指南：
"
如果您购买了一辆符合以下标准的汽车或卡车，您可能有资格获得第30D节规定的联邦税收抵免：
- 车辆是否至少有四个轮子？
- 车辆是否重量不到14,000None磅？
- 车辆是否从至少4千瓦时、可从外部源重新充电的电池中获取能量？
- 车辆是否在2022年之前购买？
  - 如果是，制造商是否出售了少于200,000辆符合条件的车辆？（Tesla和GM已经出售了超过200,000辆符合条件的车辆。）
- 车辆是否在2022年之后购买？
  - 如果是，车辆是否出现在以下北美组装车辆列表中？（唯一在北美组装的电动车是Audi Q5, BMW 330e, BMW X5, Chevrolet Bolt EUV, Chevrolet Bolt EV, Chrysler Pacifica PHEV, Ford Escape PHEV, Ford F Series, Ford Mustang MACH E, Ford Transit Van, GMC Hummer Pickup, GMC Hummer SUV, Jeep Grand Cherokee PHEV, Jeep Wrangler PHEV, Lincoln Aviator PHEV, Lincoln Corsair Plug-in, Lucid Air, Nissan Leaf, Rivian EDV, Rivian R1S, Rivian R1T, Tesla Model 3, Tesla Model S, Tesla Model X, Tesla Model Y, Volvo S60, BMW 330e, Bolt EV, Cadillac Lyriq, Mercedes EQS SUV和Nissan Leaf。）
"

问题：我可以在2021年购买的丰田普锐斯Prime上申请联邦税收抵免吗？

解决方案：

(1) 对于每个标准，确定它是否由车辆购买满足
- 车辆是否至少有四个轮子？让我们逐步思考。
```

```gpt-3.5-turbo-instruct
 丰田普锐斯Prime有四个轮子，所以答案是肯定的。
- 车辆是否重量不到14,000None磅？让我们逐步思考。丰田普锐斯Prime重量不到14,000None磅，所以答案是肯定的。
- 车辆是否从至少4千瓦时、可从外部源重新充电的电池中获取能量？让我们逐步思考。丰田普锐斯Prime有一个至少4千瓦时的电池，可以从外部源重新充电，所以答案是肯定的。
- 车辆是否在2022年之前购买？让我们逐步思考。丰田普锐斯Prime是在2021年购买的，确实在2022年之前，所以答案是肯定的。
- 车辆是否在2022年之后购买？N/A
- 如果是，车辆是否出现在以下北美组装车辆列表中？N/A

(2) 在逐个考虑每个标准后，将最终答案表述为“由于{原因}，答案很可能是{是或否}。”

由于丰田普锐斯Prime满足了所有联邦税收抵免的标准，答案很可能是肯定的。
```

### 少样本示例

#### 方法

提示模型进行推理的方式有很多种。一种方式是通过几个示例进行示范（“少样本”），如[Jason Wei和Denny Zhou等人来自Google的研究](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)所述。以下是一个示例的少样本思考链提示：

[![思考链示例](/images/chain_of_thought_fig1.png)
<br>来源：Jason Wei和Denny Zhou等人的《思考链提示引发大型语言模型的推理》（2022）](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)

更多由人工标注者编写的推理链示例：

[![思考链示例](/images/chain_of_thought_fig3.png)
<br>来源：Jason Wei和Denny Zhou等人的《思考链提示引发大型语言模型的推理》（2022）](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)

[(注意，梨子是否浮在水面上这一点实际上是有争议的)](https://twitter.com/Meaningness/status/1561062170074370048?s=20&t=mpHt8f3RRboztXxdhLFnWQ)

#### 结果

在小学数学问题上测试时，作者发现思考链提示使解决率提高了三倍，从18%提高到57%。

[![思考链示例](/images/chain_of_thought_fig5.png)
<br>来源：Jason Wei和Denny Zhou等人的《思考链提示引发大型语言模型的推理》（2022）](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)

除了数学问题外，思考链提示还在有关体育理解、硬币翻转跟踪和最后字母连接的问题上提升了性能。在大多数情况下，不需要太多示例就能达到性能增益的饱和点（大约8个左右）。

[![思考链示例](/images/chain_of_thought_fig11.png)
<br>来源：Jason Wei和Denny Zhou等人的《思考链提示引发大型语言模型的推理》（2022）](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)

要了解更多，请阅读[完整论文](https://arxiv.org/abs/2201.11903)。

#### 影响

与“Let's think step by step”技术相比，少样本示例方法的一个优势是您可以更容易地指定模型在给出最终答案之前应该执行的推理的格式、长度和风格。这在模型最初没有以正确的方式或深度进行推理的情况下特别有帮助。


question = r"""
将以下文本翻译成中文，切记保持md格式

### Least-to-most prompting

In addition to doing poorly on long reasoning chains (where selection-inference shines), chain-of-thought prompting can especially struggle when the examples are short but the task is long.

#### Method

Least-to-most prompting is another technique that splits up reasoning tasks into smaller, more reliable subtasks. The idea is to elicit a subtask from the model by prompting it with something like `To solve {question}, we need to 
### 最少到最多的提示

除了在长推理链上的表现不佳（选择推理在这里表现优异）之外，当示例简短但任务很长时，思维链提示可能会特别挣扎。

#### 方法

最少到最多的提示是另一种将推理任务分解为更小、更可靠的子任务的技术。其想法是通过提示模型以类似于`为了解决{问题}，我们需要首先解决:`的方式来引出子任务。然后，模型手握这个子任务，可以生成一个解决方案。这个解决方案会被附加到原始问题上，重复这一过程直到产生最终答案。

[![最少到最多的提示](/images/least-to-most_fig1.png)
<br>来源：_Least-to-most Prompting Enables Complex Reasoning in Large Language Models_ 由Denny Zhou等（2022年）](https://arxiv.org/abs/2205.10625)

#### 结果

当应用到涉及长推理链的基准测试中时（使用`code-davinci-002`，它被优化用于代码但仍能理解文本），作者测量到的提高幅度高达16% -> 99.7%！

[
![最少到最多的提示在最后字母连接任务上的结果](/images/least-to-most_tab4.png)
![最少到最多的提示在SCAN上的结果](/images/least-to-most_tab9.png)
![最少到最多的提示在DROP数值推理上的结果](/images/least-to-most_tab11.png)
<br>来源：_Least-to-most Prompting Enables Complex Reasoning in Large Language Models_ 由Denny Zhou等（2022年）](https://arxiv.org/abs/2205.10625)

#### 含义

尽管上述最少到最多的提示带来的提高是令人印象深刻的，但它们是在一小部分需要长推理链的任务上测量的。

尽管如此，它们展示了一个共同主题：通过（a）将复杂任务分解为更小的子任务和（b）给模型更多时间和空间来计算答案来提高可靠性。

要了解更多，请阅读[完整论文](https://arxiv.org/abs/2205.10625)。

## 相关概念

### 迈欧提克提示

#### 方法

与之前的技术不同，这些技术试图最大化正确答案的可能性，另一种方法是使用GPT-3生成一棵可能解释的树（包括正确的和错误的），然后分析它们的关系来猜测哪个集合是正确的。这种技术由[Jaehun Jung等人在2022年5月](https://arxiv.org/abs/2205.11822)提出并命名为迈欧提克提示（maieutic means relating to the Socratic method of asking questions to elicit ideas）。

这个方法复杂，具体如下：

- 首先，构建一个迈欧提克树，每个节点都是一个可能为真或假的陈述：
  - 从一个多项选择题或真/假陈述开始（例如`战争不可能有平局`）
  - 对于问题的每个可能答案，使用模型生成相应的解释（以`战争不可能有平局？真，因为`这样的提示开始）
  - 然后，提示模型提供问题和生成的解释，并要求它产生答案。如果反转解释（以`说{解释}是错误的`这样的前缀开始）反转了答案，则认为该解释是“逻辑上不可分割的”。
  - 如果一个解释不是逻辑上不可分割的，则重复上述过程进行递归处理，将每个解释转化为真/假问题，并为每个新问题生成更多解释。
  - 当所有的递归解释完成后，你会得到一棵解释的树，其中每个叶子节点都具有这样一个特性：反转解释会反转模型的答案。
- 第二步，将树转化为关系图：
  - 对树中的每个节点，计算模型对每个节点的相对信任度（从给定解释得到`True`答案的概率中推断出）
  - 对树中的每对节点，使用模型来识别它们是否被蕴含（暗示）或矛盾。
- 第三步，找到最一致的信念集合并认为这些是正确的：
  - 具体来说，使用每个节点的信任度强度和它们之间的逻辑关系，制定问题为一个加权最大可满足性问题（MAX-SAT）
  - 使用求解器找到最自None洽的信念集合，并认为这些为真

[
![迈欧提克提示](/images/maieutic_fig2.png)
![迈欧提克提示](/images/maieutic_fig6.png)
<br>来源：_Maieutic Prompting: Logically Consistent Reasoning with Recursive Explanations_ by Jaehun Jung等（2022年）](https://arxiv.org/abs/2205.11822)

#### 结果

[![迈欧提克提示结果](/images/maieutic_tab1.png)
<br>来源：_Maieutic Prompting: Logically Consistent Reasoning with Recursive Explanations_ by Jaehun Jung等（2022年）](https://arxiv.org/abs/2205.11822)

#### 含义

除了复杂性外，这种方法的一个局限性是它似乎只适用于可以作为多项选择题的问题。

要了解更多，请阅读[完整论文](https://arxiv.org/abs/2205.11822)。