# 问题排查指南
本指南可帮助您诊断和解决调用 Gemini API 时发生的常见问题。如果您遇到 API 密钥问题，请确保您已根据 API 密钥设置指南正确设置了 API 密钥。

## 检查您的 API 调用是否存在模型参数错误
确保您的模型参数在以下值范围内：

| 模型参数 |	值（范围）|
| --- | --- |
| 候选人数量| 	1-8（整数）|
| 温度| 	0.0-1.0|
| 输出令牌数量上限 |	使用 get_model (Python) 确定所用模型的令牌数量上限。|
| TopP| 	0.0-1.0|
## 检查您的模型是否正确
确保您使用的是受支持的模型。使用[list_models (Python)](https://ai.google.dev/api/python/google/generativeai/list_models?hl=zh-cn)获取所有模型可供使用。
## 安全问题
如果您发现提示因 API 调用中的安全设置而被屏蔽，请根据您在 API 调用中设置的过滤条件检查提示。

如果您看到`BlockedReason.OTHER`，则表示查询或响应可能违反了[服务条款](https://ai.google.dev/terms?hl=zh-cn)或不受支持。
## 改进模型输出
若要获得更高质量的模型输出，不妨尝试编写更多结构化提示。[提示设计简介](prompt_best_practices.md)页面介绍了一些基本概念、策略和最佳实践，可帮助您入门。

如果您有数百个优质输入/输出对的示例，还可以考虑[模型调整](model_tuning_guidance.md)。
## 了解令牌限制
使用 `ModelService API` 可获取有关模型的其他元数据，[如PaLM模型的其他元数据](palm:model-metadata)，包括输入和输出令牌限制。

如需获取提示所使用的令牌，请将[countMessageTokens](https://ai.google.dev/api/rest/v1beta/models/countMessageTokens?hl=zh-cn)用于聊天模型，将[countTextTokens](https://ai.google.dev/api/rest/v1beta/models/countTextTokens?hl=zh-cn)用于文本模型。
## 已知问题
- 对 Google AI Studio 的移动设备支持：虽然您可以在移动设备上打开网站，但该网站还没有针对小- 进行优化。
- 该 API 仅支持英语。使用不同语言提交提示可能会产生意外甚至被屏蔽的响应。如需了解最新动态，请参阅[可用语言](https://ai.google.dev/available_regions?hl=zh-cn#available_languages)。

## 提交 bug
在 GitHub 中提交问题，以便提出问题或提交功能请求或 bug。

- [API 使用方面的一般问题](https://github.com/google/generative-ai-docs)
- [Python 客户端库问题](https://github.com/google/generative-ai-python)
- [Swift 客户端库问题](https://github.com/google/generative-ai-swift)
