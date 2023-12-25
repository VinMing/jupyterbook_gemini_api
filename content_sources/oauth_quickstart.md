# OAuth 身份验证
使用 OAuth 进行身份验证快速入门

Gemini API 允许您对自己的数据执行语义检索。由于这是您的数据，因此需要比 API 密钥更严格的访问控制。

本快速入门使用适合测试环境的简化身份验证方法。对于生产环境，请先了解[身份验证和授权](https://developers.google.com/workspace/guides/auth-overview)，然后再[选择适合您的应用程序的访问凭据](https://developers.google.com/workspace/guides/create-credentials#choose_the_access_credential_that_is_right_for_you)。

## 目标
- 为 OAuth 设置云项目
- 设置应用程序默认凭据
- 管理程序中的凭据，而不是使用 gcloud auth


## 前提条件
要运行此快速入门，您需要：
- 一个 [Google Cloud 项目](https://developers.google.com/workspace/guides/create-project)
- [gcloud CLI 的本地安装](https://cloud.google.com/sdk/docs/install)

## 设置您的云项目
要完成本快速入门，您首先需要设置您的云项目。

### 1. 启动API
在使用 Google API 之前，您需要在 Google Cloud 项目中启用它们。
- 在 Google Cloud 控制台中，启用 Google [Generative Language API](https://console.cloud.google.com/flows/enableapi?apiid=generativelanguage.googleapis.com)。

### 2. 配置 OAuth 同意屏幕
接下来配置项目的 OAuth 同意屏幕并将您自己添加为测试用户。如果您已为云项目完成此步骤，请跳至下一部分。
1. 在[Google Cloud 控制台](https://console.cloud.google.com/apis/credentials/consent)中，转到**菜单 > API 和服务 > OAuth 同意屏幕**。
2. 为您的应用程序选择**外部**用户类型，然后单击**创建**。
3. 填写应用程序注册表（您可以将大部分字段留空），然后单击**保存并继续**。
4. 现在，您可以跳过添加范围并单击**保存并继续**。今后，当您创建要在Google Workspace组织外部使用的应用时，您必须添加并验证应用所需的授权范围。
5. 添加测试用户
    1. 在测试用户下，单击添加用户。
    2. 输入您的电子邮件地址和任何其他授权测试用户，然后单击**保存并继续**
6. 查看您的应用程序注册摘要。要进行更改，请单击**编辑**。如果应用程序注册看起来正常，请单击**返回仪表板**。

### 3. 授权桌面应用程序的凭据
要作为最终用户进行身份验证并访问应用程序中的用户数据，您需要创建一个或多个 OAuth 2.0 客户端 ID。客户端 ID 用于向 Google 的 OAuth 服务器标识单个应用程序。如果您的应用程序在多个平台上运行，则必须为每个平台创建单独的客户端 
1. 在[Google Cloud 控制台](https://console.cloud.google.com/apis/credentials)中，转到**菜单 > API 和服务 > 凭据**。
2. 单击**创建凭据 > OAuth 客户端 ID**。
3. 单击**应用程序类型 > 桌面应用程序**。
4. 在**名称**字段中，输入凭证的名称。此名称仅显示在 Google Cloud 控制台中。
5. 单击**创建**。将出现 OAuth 客户端创建屏幕，其中显示您的新客户端 ID 和客户端密码。
6. 单击**确定**。新创建的凭据显示在**OAuth 2.0 客户端 ID下**。
7. 单击下载按钮保存 JSON 文件。它将保存为`client_secret_<identifier>.json`，并将其重命名为`client_secret.json`并将其移动到您的工作目录。


## 设置应用程序默认凭据
要将 client_secret.json 文件转换为可用凭据，请将其位置传递给 gcloud auth application-default login 命令的 --client-id-file 参数。
```shell
gcloud auth application-default login \
    --client-id-file=client_secret.json \
    --scopes='https://www.googleapis.com/auth/cloud-platform,https://www.googleapis.com/auth/generative-language.retriever'
```
本教程中简化的项目设置会触发**Google 尚未验证此应用程序**。对话。这是正常现象，选择**继续**。 这会将生成的令牌放置在众所周知的位置，以便 gcloud 或客户端库可以访问它。

这会将生成的令牌放置在众所周知的位置，以便 gcloud 或客户端库可以访问它。

```{tip}
注意：如果在 Colab 上运行，请包含 --no-browser 并仔细按照其打印的说明进行操作（不要仅单击链接）。另请确保您本地的 gcloud --version 是与 Colab 匹配的最新版本。
```
```python
gcloud auth application-default login 
    --no-browser
    --client-id-file=client_secret.json 
    --scopes='https://www.googleapis.com/auth/cloud-platform,https://www.googleapis.com/auth/generative-language.retriever'
```
设置应用程序默认凭据 (ACD) 后，大多数语言的客户端库只需很少甚至不需要帮助即可找到它们。
### Curl
测试其是否有效的最快方法是使用它通过curl 访问REST API：
```shell
access_token=$(gcloud auth application-default print-access-token)
project_id=<MY PROJECT ID>

curl -X GET https://generativelanguage.googleapis.com/v1/models \
    -H 'Content-Type: application/json' \
    -H "Authorization: Bearer ${access_token}" \
    -H "x-goog-user-project: ${project_id}" | grep '"name"'
```
### Python
在 python 中，客户端库应该自动找到它们：
```shell
pip install google-generativeai
```
测试它的最小脚本可能是：
```python
import google.generativeai as genai
print('Available base models:', [m.name for m in genai.list_models()])
```
## 下一步
如果这有效，您就可以尝试对文本数据进行[语义检索](semantic_retriever.ipynb)。


## 自行管理凭证 [Python]
在许多情况下，您无法使用 gcloud 命令从客户端 ID (client_secret.json) 创建访问令牌。 Google 提供了多种语言的库，让您可以在应用程序中管理该流程。本节用 python 演示了该过程。[Drive API](https://developers.google.com/drive/api/quickstart/python)文档中提供了针对其他语言的此类过程的等效示例
### 1. 安装必须的库
安装适用于 Python 的 Google 客户端库和 Gemini 客户端库。
```python
pip install --upgrade -q google-api-python-client google-auth-httplib2 google-auth-oauthlib
pip install google-generativeai
```
### 2.编写凭证管理器
为了最大限度地减少单击授权屏幕的次数，请在工作目录中创建一个名为`load_creds.py`的文件来缓存 `token.json`文件，以便稍后重用，或者在过期时刷新。

从以下代码开始，将`client_secret.json`文件转换为可用于`genai.configure`的令牌：
```python
import os.path

from google.auth.transport.requests import Request
from google.oauth2.credentials import Credentials
from google_auth_oauthlib.flow import InstalledAppFlow

SCOPES = ['https://www.googleapis.com/auth/generative-language.retriever']

def load_creds():
    """Converts `client_secret.json` to a credential object.

    This function caches the generated tokens to minimize the use of the
    consent screen.
    """
    creds = None
    # The file token.json stores the user's access and refresh tokens, and is
    # created automatically when the authorization flow completes for the first
    # time.
    if os.path.exists('token.json'):
        creds = Credentials.from_authorized_user_file('token.json', SCOPES)
    # If there are no (valid) credentials available, let the user log in.
    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file(
                'client_secret.json', SCOPES)
            creds = flow.run_local_server(port=0)
        # Save the credentials for the next run
        with open('token.json', 'w') as token:
            token.write(creds.to_json())
    return creds
```
### 3. 编写你的程序
现在创建你的`script.py`：
```python
import pprint
import google.generativeai as genai
from load_creds import load_creds
creds = load_creds()
genai.configure(credentials=creds)
print('Available base models:', [m.name for m in genai.list_models()])
```
### 4.运行你的程序
在您的工作目录中，运行示例：`python script.py`
第一次运行该脚本时，它会打开一个浏览器窗口并提示您授权访问。
1. 如果您尚未登录 Google 帐户，系统会提示您登录。如果您登录了多个帐户，请务必在配置项目时选择设置为“测试帐户”的帐户。
```{tip}
注意：本教程中的简化项目设置会触发“Google 尚未验证此应用程序”。对话。这是正常现象，选择“继续”。
```
2. 授权信息存储在文件系统中，因此下次运行示例代码时，系统不会提示您授权。 
您已成功设置身份验证。

