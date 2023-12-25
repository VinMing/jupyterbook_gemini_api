# LLM 概念指南
从最基本的层面来讲，大语言模型 (LLM) 就像是复杂的自动补全。给定输入文本（“您可以引一匹马去水”），LLM 会根据从其训练数据中学到的模式，输出从统计学上来说可能会遵循的文本（“但你不能让它喝水”）。您可以使用 LLM 的这一基本属性为多种不同类型的应用提供支持：
- 生成诗歌、短篇故事、比喻、博文和其他创意文案
- 将结构化数据转换为自由格式文本
- 从自由格式文本中提取和汇总信息
- 生成代码
- 翻译
- 构建聊天机器人等
使用 LLM 构建这些功能不需要任何机器学习 (ML) 专业知识。您无需收集大型数据集或训练模型。启动您的第一个 LLM 原型只需用几句话描述您希望模型执行的操作。

## 示例应用

**诗歌生成器**
| 用户输入| 模型输出： |
| -------- | --------- |
| Write me a four-line poem about puppies and Android phones. Make sure it rhymes. | Puppies are cute,<br>Android phones are neat,<br>They both bring joy,<br>And make life complete.|
**列表生成器**

| 用户输入| 模型输出|
| -------- | --------- |
| Generate a bulleted list of items I need to pack for a three-day camping trip.|*   Tent<br>*   Sleeping bag<br>*   Sleeping pad<br>*   Camp stove<br>*   Pots and pans<br>*   Utensils<br>*   Food<br>*   Water<br>*   First-aid kit<br>*   Sunscreen<br>*   Insect repellent<br>*   ...|

您可以让 LLM 生成各种类似于这样的有用行为，只需撰写正确的输入文本（也称为“提示符”）即可。找出合适的措辞来让 LLM 完成你期望的操作，这门艺术和科学叫做“提示设计”（也称为“提示工程”，简称“提示”）。
(prompt_design_101)=
## 提示设计基础知识
上一部分展示了一些包含说明的提示示例，例如“给我写首诗”。这种说明可能适用于某些类型的任务。但是，对于其他应用，另一种称为小样本提示的方法可能效果更好。少样本提示充分利用了大型语言模型非常擅长识别和复制文本数据中的模式这一事实。其想法是向 LLM 发送一个它会学习完成的文本模式。例如，假设您要构建一个应用，该应用将国家/地区名称作为输入并输出其首府。以下是一个专门用来实现此目的的文本提示：
```
Italy : Rome
France : Paris
Germany :
```
在此提示中，您需要建立一种模式：`[country] : [capital]`。如果您将此提示发送到大型语言模型，模型将自动补全相应模式并返回如下内容：
```
    Berlin
Turkey : Ankara
Greece : Athens
```
此模型的回答可能有点奇怪。模型不仅返回了德国的大写（您的手写提示中的最后一个国家/地区），还返回了额外国家/大写对的完整列表。这是因为 LLM 正在“延续这种模式”。如果您只是想构建一个函数来告诉您输入国家/地区的首都（例如“Germany : Berlin”），那么您可能并不在意模型在“Berlin”之后生成的任何文本。事实上，作为应用设计人员，您可能希望截断这些无关的示例。此外，您可能希望将输入参数化，以便 Germany 不是固定字符串，而是最终用户提供的变量：
```
Italy : Rome
France : Paris
<user input here> :
```
你刚刚写了几个关于发展国家首都的提示。

您可以按照此“少样本提示”模板完成大量任务。以下是几张提示，格式略有不同，用于将 Python 代码转换成 JavaScript 代码：
```
Convert Python to Javascript.
Python: print("hello world")
Javascript: console.log("hello world")
Python: for x in range(0, 100):
Javascript: for(var i = 0; i < 100; i++) {
Python: ${USER INPUT HERE}
Javascript:

```
或者，按照这个“反向字典”提示操作。在给定定义后，它会返回符合该定义的单词：
```
Given a definition, return the word it defines.
Definition: When you're happy that other people are also sad.
Word: schadenfreude
Definition: existing purely in the mind, but not in physical reality
Word: abstract
Definition: ${USER INPUT HERE}
Word:

```

您可能已经注意到，这些小样本提示的确切模式略有不同。除了包含示例之外，在提示中提供说明也是您在编写自己的提示时需要考虑的另一种策略，因为这有助于将您的意图传达给模型。   
提示与传统软件开发   
与按照精心编写的规范设计的传统软件不同，LLM 的行为在很大程度上是不透明的，即使对于模型训练器而言也是如此。因此，您通常无法提前预测哪种类型的提示结构最适合特定模型。此外，LLM 的行为在很大程度上取决于其训练数据，由于模型会不断根据新数据集进行调整，有时模型的变化幅度过大，以至于它无意中更改了哪个提示结构的效果最佳。这对您来说意味着什么？进行实验！尝试使用不同的提示格式。
(model_parameters)=
## 模型参数
您发送到模型的每个提示都包含用于控制模型如何生成回答的参数值。对于不同的参数值，模型会生成不同的结果。最常见的模型参数包括：
- 输出令牌数量上限：指定响应中可生成的令牌数量上限。一个词元约为 4 个字符。100 个令牌大约对应 60-80 个单词。
- 温度：温度用于控制令牌选择的随机性。该温度用于在响应生成过程中进行采样（在应用 topP 和 topK 时发生）。较低的温度适合需要更具确定性或不够开放的回答的提示，而较高的温度可以产生更加多样化或更具创意的结果。温度为 0 表示确定性，即始终选择概率最高的响应。
- topK：topK 参数可更改模型选择输出令牌的方式。topK 为 1 表示所选词元是模型词汇表的所有词元中概率最高的词元（也称为贪心解码）。如果 topK 为 3，则表示系统将根据该温度从 3 个概率最高的词元中选择下一个词元。对于每个词元选择步骤，系统都会对概率最高的 topK 词元进行采样。然后，系统会根据 topP 进一步过滤令牌，并使用温度采样选择最终令牌。
- topP：topP 参数可更改模型选择输出令牌的方式。系统会按照概率从最高到最低的顺序选择词元，直到所选词元的概率总和等于 topP 值。例如，如果词元 A、B 和 C 的概率分别是 0.3、0.2 和 0.1，并且 topP 值为 0.5，则模型将根据温度选择 A 或 B 作为下一个词元，并将 C 作为候选词元。topP 的默认值为 0.95。
- stop_sequences：设置停止序列，告知模型停止生成内容。停止序列可以是任何字符序列。请尽量避免使用可能出现在生成内容中的字符序列。

## 提示类型
根据提示中包含的上下文信息的级别，提示大致可分为三种类型。
### 零样本提示
这些提示不包含要复制的模型的示例。零样本提示本质上显示了模型完成提示的能力，而无需任何其他示例或信息。这意味着模型必须依靠其先前存在的知识来生成合理的答案。

一些常用的零样本提示模式包括：
- 说明内容
```
<Overall instruction>
<Content to operate on>
```
例如：
```
Summarize the following into two sentences at the third-grade level:

Hummingbirds are the smallest birds in the world, and they are also one of the
most fascinating. They are found in North and South America, and they are known
for their long, thin beaks and their ability to fly at high speeds.

Hummingbirds are made up of three main parts: the head, the body, and the tail.
The head is small and round, and it contains the eyes, the beak, and the brain.
The body is long and slender, and it contains the wings, the legs, and the
heart. The tail is long and forked, and it helps the hummingbird to balance
while it is flying.

Hummingbirds are also known for their coloration. They come in a variety of
colors, including green, blue, red, and purple. Some hummingbirds are even able
to change their color!

Hummingbirds are very active creatures. They spend most of their time flying,
and they are also very good at hovering. Hummingbirds need to eat a lot of food
in order to maintain their energy, and they often visit flowers to drink nectar.

Hummingbirds are amazing creatures. They are small, but they are also very
powerful. They are beautiful, and they are very important to the ecosystem.

```
- 说明-内容-说明
```
<Overall instruction or context setting>
<Content to operate on>
<Final instruction>
```
例如，
```
Here is some text I'd like you to summarize:

Hummingbirds are the smallest birds in the world, and they are also one of the
most fascinating. They are found in North and South America, and they are known
for their long, thin beaks and their ability to fly at high speeds. Hummingbirds
are made up of three main parts: the head, the body, and the tail. The head is
small and round, and it contains the eyes, the beak, and the brain. The body is
long and slender, and it contains the wings, the legs, and the heart. The tail
is long and forked, and it helps the hummingbird to balance while it is flying.
Hummingbirds are also known for their coloration. They come in a variety of
colors, including green, blue, red, and purple. Some hummingbirds are even able
to change their color! Hummingbirds are very active creatures. They spend most
of their time flying, and they are also very good at hovering. Hummingbirds need
to eat a lot of food in order to maintain their energy, and they often visit
flowers to drink nectar. Hummingbirds are amazing creatures. They are small, but
they are also very powerful. They are beautiful, and they are very important to
the ecosystem.

Summarize it in two sentences at the third-grade reading level.
```

- 继续。有时，您可以让模型在没有任何指令的情况下继续文本。例如，下面是一个零样本提示，其中模型旨在继续输入所提供的输入：
```
Once upon a time, there was a little sparrow building a nest in a farmer's
barn. This sparrow
```
使用零样本提示生成创造性文本格式，例如诗歌、代码、脚本、音乐作品、电子邮件、信件等。

### 单样本提示
这些提示为模型提供单个示例来复制并继续模式。这样就可以从模型生成可预测的响应。

例如，您可以生成如下所示的食物配对：
```
Food: Apple
Pairs with: Cheese
Food: Pear
Pairs with:
```

### 少样本提示
这些提示为模型提供了多个可复制的样本。使用少样本提示来完成复杂的任务，例如根据模式合成数据。

示例提示可能是：
```
Generate a grocery shopping list for a week for one person. Use the JSON format
given below.
{"item": "eggs", "quantity": "6"}
{"item": "bread", "quantity": "one loaf"}
```
## LLM幕后故事
本部分旨在解答以下问题：**LLM 回答是随机性的，还是确定性的？**

简而言之，两个问题的答案都是肯定的。当您向 LLM 提出提示时，系统会分两个阶段生成其文本响应。在第一阶段，LLM 会处理输入提示，并针对可能紧随其后的可能词元（字词）生成概率分布。例如，如果您使用输入文本“The dog jumped over the ...”来提示，LLM 将生成一个可能的后续字词数组：
```
[("fence", 0.77), ("ledge", 0.12), ("blanket", 0.03), ...]
```
这个过程是确定的；LLM 每次输入相同的提示文本时都会生成相同的分布。

在第二阶段，LLM 会通过多种解码策略之一将这些分布转换为实际的文本响应。一个简单的解码策略可以在每个时间步选择最有可能的令牌。此过程始终具有确定性。不过，您也可以选择通过对模型返回的分布进行随机采样来生成响应。此过程是随机的（随机）。通过设置温度来控制此解码过程中允许的随机性。温度为 0 表示仅选择最可能的词元，不具有随机性。相反，高温会向模型选择的词元注入高度的随机性，从而导致模型响应更加意外和惊人。

## 深入阅读

- 现在您对提示和 LLM 有了更深入的了解，不妨尝试使用[Google AI Studio](https://aistudio.google.com/?hl=zh-cn)编写自己的提示。
- 如需详细了解创建提示的[最佳实践](prompt_best_practices.md)，请参阅提示指南。




