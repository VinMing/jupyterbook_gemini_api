# Gemini API 概览
借助 Gemini API，您可以使用 Google 提供的最新生成模型。熟悉了该 API 提供的一般功能后，请尝试根据所选语言快速入门，开始进行开发。
```{tip}
注意 ：如果您刚开始接触生成式 AI 模型，请访问[概念指南](concepts.md)，或开始在 [Google AI Studio](https://makersuite.google.com/?hl=zh-cn)中对提示进行原型设计。
```

## 模型
Gemini 是 Google 开发的一系列多模态生成式 AI 模型。Gemini 模型可以接受提示中的文本和图片（具体取决于您选择的模型变体），并输出文本响应。旧版 PaLM 模型接受纯文本和输出文本响应。

如需获取更详细的模型信息，请参阅[模型](./models/models.md)页面。您还可以使用[list_models](https://ai.google.dev/api/python/google/generativeai/list_models?hl=zh-cn)方法列出所有可用的模型，然后使用[get_model](https://ai.google.dev/api/python/google/generativeai/get_model?hl=zh-cn)方法获取特定模型的元数据。

## 提示数据和设计
特定的 Gemini 模型同时接受图片和文本数据作为输入。此功能为生成内容、分析数据和解决问题提供了许多其他的可能性。您需要考虑一些限制和要求，包括您所用模型的一般输入令牌限制。如需了解特定模型的令牌限制，请参阅[Gemini 模型](./models/gemini.md)。
### 提示的图片要求
使用图片数据的提示受到以下限制和要求的约束：

- 图片必须采用以下任一图片数据 MIME 类型：
    - PNG - 图片/png
    - JPEG - image/jpeg
    - WEBP - image/webp
    - HEIC - 图片/heic
    - HEIF - image/heif
- 最多 16 张图片
- 整个提示（包括图片和文本）不得超过 4MB
- 对图像中的像素数没有具体限制；但是，较大的图像会缩小以适应最大分辨率 (3072 x 3072)，同时保持其原始宽高比。

在提示中使用图片时，请遵循以下建议以获得最佳效果：
- 包含一张图片的提示往往能产生更好的结果。

### 提示设计和文本输入
创建有效的提示（即提示工程）是艺术与科学的结合。如需了解有关如何提示的指导，请参阅[提示指南](./prompt_best_practices.md)；如需了解不同的提示方法，请参阅[提示 101 ](prompt_design_101)指南。
## 生成内容
借助 Gemini API，您可以使用文本和图片数据进行提示，具体取决于您使用的模型变体。例如，您可以通过`gemini-pro`模型使用文本提示生成文本，并使用文本和图片数据向 `gemini-pro-vision`模型发出提示。本部分给出了每种方法的简单代码示例。 如需查看涵盖所有参数的详细示例，请参阅[generateGenerate](https://ai.google.dev/api/rest/v1beta/models/generateContent?hl=zh-cn) API 参考文档。
### 文本和图片输入
可以向`gemini-pro-vision`模型发送包含图片的文本提示，以执行与视觉相关的任务。例如，为图片添加说明或识别图片中的内容。
```{tip}
不能向 gemini-pro-vision 模型发送纯文本提示。请为纯文本提示使用 gemini-pro 模型。
```
以下代码示例展示了针对每种受支持语言的文本和图片提示的简单实现：  

````{tabbed} Python
```{code-block} python
model = genai.GenerativeModel('gemini-pro-vision')

cookie_picture = [{
    'mime_type': 'image/png',
    'data': Path('cookie.png').read_bytes()
}]
prompt = "Do these look store-bought or homemade?"

response = model.generate_content(
    model="gemini-pro-vision",
    content=[prompt, cookie_picture]
)
print(response.text)
```
如需查看完整的代码段，请参阅 [Python 快速入门](python_quickstart.ipynb)。
````

````{tabbed} Go
```{code-block} go
vmodel := client.GenerativeModel("gemini-pro-vision")

data, err := os.ReadFile(filepath.Join("path-to-image", imageFile))
if err != nil {
  log.Fatal(err)
}
resp, err := vmodel.GenerateContent(ctx, genai.Text("Do these look store-bought or homemade?"), genai.ImageData("jpeg", data))
if err != nil {
  log.Fatal(err)
}

```
如需查看完整示例，请参阅 Go 快速入门。
````


````{tabbed} Node.js
```{code-block} javascript
const model = genAI.getGenerativeModel({ model: "gemini-pro-vision" });

const prompt = "Do these look store-bought or homemade?";
const image = {
  inlineData: {
    data: Buffer.from(fs.readFileSync("cookie.png")).toString("base64"),
    mimeType: "image/png",
  },
};

const result = await model.generateContent([prompt, image]);
console.log(result.response.text());
```
如需查看完整示例，请参阅 Node.js 快速入门。
````

````{tabbed} Web
```{code-block} javascript
const model = genAI.getGenerativeModel({ model: "gemini-pro-vision" });

const prompt = "Do these look store-bought or homemade?";
const image = {
  inlineData: {
    data: base64EncodedImage /* see JavaScript quickstart for details */,
    mimeType: "image/png",
  },
};

const result = await model.generateContent([prompt, image]);
console.log(result.response.text());
```
如需查看完整示例，请参阅 JavaScript 快速入门。
````
````{tabbed} Swift
```{code-block} swift
let model = GenerativeModel(name: "gemini-pro-vision", apiKey: "API_KEY")
let cookieImage = UIImage(...)
let prompt = "Do these look store-bought or homemade?"

let response = try await model.generateContent(prompt, cookieImage)


```
如需查看完整示例，请参阅 Swift 快速入门。
````

````{tabbed} Android
```{code-block} c
val generativeModel = GenerativeModel(
    modelName = "gemini-pro-vision",
    apiKey = BuildConfig.apiKey
)

val cookieImage: Bitmap = // ...
val inputContent = content() {
  image(cookieImage)
  text("Do these look store-bought or homemade?")
}

val response = generativeModel.generateContent(inputContent)
print(response.text)
```
如需查看完整示例，请参阅Android 快速入门。
````
````{tabbed} cURL
```{code-block} shell
curl https://generativelanguage.googleapis.com/v1/models/gemini-pro-vision:GenerateContent?key=${API_KEY} \
    -H 'Content-Type: application/json' \
    -d @<(echo'{
          "contents":[
            { "parts":[
                {"text": "Do these look store-bought or homemade?"},
                { "inlineData": {
                    "mimeType": "image/png",
                    "data": "'$(base64 -w0 cookie.png)'"
                  }
                }
              ]
            }
          ]
         }')

```
如需查看完整示例，请参阅 REST API 快速入门。
````

### 纯文字输入
Gemini API 还可处理纯文字输入。借助此功能，您可以执行自然语言处理 (NLP) 任务，例如文本补全和摘要。

以下代码示例针对每种受支持的语言展示了一种纯文本提示的简单实现方法：

````{tabbed} Python
```{code-block} python
prompt = "Write a story about a magic backpack."

response = genai.generate_content(
    model="gemini-pro",
    prompt=prompt
)

```
如需查看完整的代码段，请参阅 [Python 快速入门](python_quickstart.ipynb)。
````

````{tabbed} Go
```{code-block} go
ctx := context.Background()
client, err := genai.NewClient(ctx, option.WithAPIKey(os.Getenv("API_KEY")))
if err != nil {
  log.Fatal(err)
}
defer client.Close()

model := client.GenerativeModel("gemini-pro")
resp, err := model.GenerateContent(ctx, genai.Text("Write a story about a magic backpack."))
if err != nil {
  log.Fatal(err)
}


```
如需查看完整示例，请参阅 Go 快速入门。
````


````{tabbed} Node.js
```{code-block} javascript
const model = genAI.getGenerativeModel({ model: "gemini-pro" });
const prompt = "Write a story about a magic backpack.";

const result = await model.generateContent(prompt);
console.log(result.response.text());

```
如需查看完整示例，请参阅 Node.js 快速入门。
````

````{tabbed} Web
```{code-block} javascript
const model = genAI.getGenerativeModel({ model: "gemini-pro" });
const prompt = "Write a story about a magic backpack.";

const result = await model.generateContent(prompt);
console.log(result.response.text());

```
如需查看完整示例，请参阅 JavaScript 快速入门。
````
````{tabbed} Swift
```{code-block} swift
let model = GenerativeModel(name: "gemini-pro", apiKey: "API_KEY")
let prompt = "Write a story about a magic backpack."

let response = try await model.generateContent(prompt)

```
如需查看完整示例，请参阅 Swift 快速入门。
````

````{tabbed} Android
```{code-block} c
val generativeModel = GenerativeModel(
    modelName = "gemini-pro",
    apiKey = BuildConfig.apiKey
)

val prompt = "Write a story about a magic backpack."
val response = generativeModel.generateContent(prompt)
print(response.text)

```
如需查看完整示例，请参阅Android 快速入门。
````
````{tabbed} cURL
```{code-block} shell
curl https://generativelanguage.googleapis.com/v1/models/gemini-pro:generateContent?key=$API_KEY \
    -H 'Content-Type: application/json' \
    -X POST \
    -d '{ "contents":[
      { "parts":[{"text": "Write a story about a magic backpack"}]}
    ]
}'
```
如需查看完整示例，请参阅 REST API 快速入门。
````
因为主要是Python语言，以下示例只展示Python代码的示例，更多的coding语言请移步到，[官方文档Gemini api](https://ai.google.dev/docs/gemini_api_overview)查看
### 多轮对话（聊天）
可以使用 Gemini API 为您的用户打造互动式聊天体验。 借助此 API 的聊天功能，您可以收集多轮问题和回复，从而支持用户逐步寻找答案，或获得有关多部分问题的帮助。此功能非常适合需要持续通信的应用，例如聊天机器人、互动式导师或客户服务助理。  
以下代码示例展示了针对每种受支持的语言聊天互动的简单实现：
````{tabbed} Python
```{code-block} python
model = genai.GenerativeModel('gemini-pro')
chat = model.start_chat(history=[])

response = chat.send_message(
    "Pretend you\'re a snowman and stay in character for each response.")
print(response.text)

response = chat.send_message(
    "What\'s your favorite season of the year?")
print(response.text)

```
如需查看完整的代码段，请参阅 [Python 快速入门](python_quickstart.ipynb)。
````
### 流式响应
Gemini API 提供了另一种从生成式 AI 模型接收响应的方式：以数据流的形式接收。流式响应在模型生成增量数据时，会将这些数据发送回您的应用。借助此功能，您可以快速响应用户请求以显示进度，并打造更具互动性的体验。

使用 Gemini 模型进行自由形式提示和聊天时可以使用流式响应。以下代码示例展示了如何针对每种受支持的语言请求针对提示的流式响应：
````{tabbed} Python
```{code-block} python
prompt = "Write a story about a magic backpack."

response = genai.stream_generate_content(
    model="models/gemini-pro",
    prompt=prompt
)


```
如需查看完整的代码段，请参阅 [Python 快速入门](python_quickstart.ipynb)。
````
## Embeddings
Gemini API 中的嵌入服务可为字词、短语和句子生成先进的嵌入。生成的嵌入随后可用于 NLP 任务，例如语义搜索、文本分类和聚类等等。请参阅[嵌入指南](embeddings_guide.md)，了解什么是嵌入以及嵌入服务的一些关键用例，以帮助您入门。
## 后续步骤
- 参阅 [Google AI Studio 快速入门](ai_studio_quickstart.md)，开始使用 Google AI Studio 界面。
- 查看 [Python](python_quickstart.ipynb)、Go 或 Node.js 快速入门，试用 Gemini API 的服务器端访问- 方式。
- 参考 [Web 快速入门]()，开始构建 Web 应用。
- 按照 [Swift 快速入门]()或 [Android 快速入门d]()开始构建移动应用。
- 如果您已是Google Cloud用户（或者希望在 Vertex 上使用 Gemini 以利用强大的Google Cloud 生态系统），请参阅 [Google Cloud 上的 Genmini](https://cloud.google.com/vertex-ai/docs/generative-ai/learn/overview?hl=zh-cn)了解详情。
