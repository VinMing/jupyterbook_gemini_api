# 适用于 Gemini API 的 AI 模型
Gemini API 为 Gemini 和 PaLM 生成式 AI 模型提供了编程接口。Gemini 是 Google 的最新一代生成模型，超越了 PaLM 模型系列的功能。本页面提供了有关如何为您的应用选择模型的一般指导。 本指南提供了有关每个模型系列和变体的信息，以帮助您确定哪个模型系列最适合您的使用场景。下面简要概述了可用模型及其功能：

| 模型     | 输入      |输出      |
| -------- | --------- | --------- |
|[Gemini](gemini.md) |  | |
|- Gemini pro|  文字 | 文字 |
|- Gemini pro vision|  文字和图片 | 文字 |
|[PaLM(旧版)](palm.md) |  | |
|- Bison Text|  文字 | 文字 |
|- Bison Chat|  文字 | 文字 |
|[Embedings](gemini.md#embeding) |  | |
|- Embedding|  文字 | 文字 embeddings |
|[Retrieval](gemini.md#aqa) |  | |
|- AQA|  文字 | 文字 |

Gemini 和 PaLM 模型之间的主要区别在于 Gemini 视觉模型能够处理图像输入。您可以使用文本和/或图片向 Gemini 模型提示。PaLM 模型仅处理文本输入和输出。这两个模型系列都可以执行文本提示、聊天互动和结构化提示。

重要提示：当前版本的 Gemini 模型不支持 PaLM 模型支持以下功能：
- 使用您自己的数据[微调](../tuning_quickstart_python.ipynb)语言模型以执行特定任务


## 安全性和预期用途
生成式人工智能模型是功能强大的工具，但也有其局限性。它们的多功能性和适用性有时可能会导致意外的输出，例如不准确、偏向或攻击性的输出。后期处理和严格的手动评估对于限制此类输出造成的损害风险至关重要。如需了解其他安全使用建议，请参阅[安全指南](../safety_guidance.md)。

Gemini API 提供的模型可用于各种生成式 AI 和自然语言处理 (NLP) 应用。您只能通过 Gemini API 或 Google AI Studio Web 应用使用这些函数。在使用 Gemini API 时，您还需要遵守[《生成式 AI 使用限制政策》](https://policies.google.com/terms/generative-ai/use-policy?hl=zh-cn)和 [Gemini API 服务条款](terms.md)。