# 获取 API 密钥
如需使用该 API，您需要拥有 API 密钥。 只需在 Google AI Studio 中点击一下[获取密钥](https://makersuite.google.com/app/apikey?hl=zh-cn)，即可创建密钥。

```{admonition} 重要提示
请记住安全地使用您的 API 密钥。查看[API 快速入门](tutorials.md)，了解特定于语言的最佳实践，以保护您的 API 密钥。对于一些一般最佳实践，您还可以查看此[支持](https://support.google.com/googleapi/answer/6310037?hl=zh-cn)文章。
```
## 使用 curl 命令验证您的 API 密钥
您可以使用 curl 命令来验证设置。 您可以通过以下任一网址传递 API 密钥：
```shell
API_KEY="YOUR_API_KEY"
curl -H 'Content-Type: application/json' \
     -d '{"contents":[
            {"role": "user",
              "parts":[{"text": "Give me five subcategories of jazz?"}]}]}' \
     "https://generativelanguage.googleapis.com/v1/models/gemini-pro:generateContent?key=${API_KEY}"

```
或者在 x-goog-api-key 标头中：
```shell
API_KEY="YOUR_API_KEY"
curl -H 'Content-Type: application/json' \
     -H "x-goog-api-key: ${API_KEY}" \
     -d '{"contents":[
            {"role": "user",
              "parts":[{"text": "Give me five subcategories of jazz?"}]}]}' \
     "https://generativelanguage.googleapis.com/v1/models/gemini-pro:generateContent"
```
## 后续步骤
- 请参阅[API 快速入门](tutorials.md)，了解保护 API 密钥的安全和使用 API 密钥的最佳实践。