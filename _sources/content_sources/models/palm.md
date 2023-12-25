# PaLM 2 模型(旧版)
[PaLM 2](https://ai.google/discover/palm2/?hl=zh-cn)是一系列语言模型，针对关键开发者使用场景进行了优化。PaLM 系列模型包括针对文本和聊天生成以及文本嵌入训练的变体。本指南介绍了每种变体，以帮助您确定哪个变体最适合您的用例。

## 模型尺寸
模型尺寸以动物名称描述。下表显示了可用尺寸及其相互之间的相对关系。
| 模型大小 | 说明 | 服务 |
| --- | ---- | ----|
| Bison（野牛） |性能最强的 PaLM 2 模型大小。 |- PLAIN_TAXT<br>- 聊天|
| Gecko（壁虎） |最小、最高效的 PaLM 2 模型大小。 |- 嵌入|


## 模型变体
有多种 PaLM 模型可供选择，并已针对特定用例进行了优化。下表介绍了每种类型的属性。

| 变体 | 属性 | 说明 |
| --- | ---- | ----|
| Bison text |模型上次更新时间 | 2023 年 5 月|
| | 型号代码| text-bison-001|
| | 模型功能| 输入：文字<br>输出：文本<br>针对语言任务进行了优化，例如：<br><tab>代码生成<br><tab>文本生成<br>可以处理零样本、一样本和少样本任务。|
| | 支持生成方法| generateContent|
| | 输入令牌限制| 30720 |
| | 输出令牌限制| 2048|
| | 模型安全| 针对开发者可针对 6 个损害维度调整的安全设置。 如需了解详情，请参阅[安全设置](../safety_setting_gemini.ipynb)|
| | 速率限制| 每分钟90个请求|
|||
|Bison Chat |模型上次更新时间 | 2023 年 5 月|
| | 型号代码| chat-bison-001|
| | 模型功能| 输入：文字和图片<br>输出：文本<br>以对话格式生成文本。<br>针对对话语言任务（例如聊天机器人或 AI 代理的实现）进行了优化。<br>可以处理零样本、一样本和少样本任务|
| | 模型安全| 没有可调整的安全设置。 |
| | 速率限制| 每分钟90个请求|
|||
| Gecko embedding |模型上次更新时间 | 2023 年 5 月|
| | 型号代码| embedding-gecko-001|
| | 模型功能| 输入：文字<br>输出：文本<br>为输入文本生成文本嵌入<br>针对最多 1024 个词元的文本创建嵌入进行了优化。|
| | 支持生成方法| embedContent|
| | 模型安全| 没有可调整的安全设置。 |
| | 速率限制| 每分钟1500个请求|

(palm:model-metadata)=
## 模型元数据
使用`ModelService`API 可获取有关最新模型的其他元数据，例如输入和输出令牌限制。下表显示了 text-bison-001 模型变体的元数据。
```{tip}
注意 ：对于 PaLM 2 模型，一个令牌相当于大约 4 个字符。100 个词元约为 60-80 个英语单词。
```
| 属性 | 值 |
|--- | --- |
|显示名称 | 	文本 Bison |
|型号代码 | 	models/text-bison-001 |
|说明 | 	适合文本生成的模型 |
|输入令牌限制 | 	8196 |
|输出令牌限制 | 	1024 |
|支持的生成方法  |	generateText |
|温度  |	0.7 |
|top_p  |	0.95 |
|top_k 	 |40 |
## 模型属性
下表介绍了所有模型变体通用的 PaLM 2 属性。
```{tip}
注意 ：可配置参数仅适用于文本和聊天模型变体，而不适用于嵌入。
```

|属性| 	说明|
|--- | --- |
|训练数据| PaLM 2 的知识截止时间为 2021 年年中。并且无法了解该时间之后发生的事件。 |
|支持的语言| 	英语|
|可配置的模型参数| - 前p<br>- 前k个<br>- 温度<br>- 停止序列<br>- 最大输出长度<br>- 候选回复数量|

如需了解每个参数，请参阅“LLM 简介”指南的[模型参数](model_parameters)部分。