> ## Documentation Index
> Fetch the complete documentation index at: https://wiki.agnes-ai.cn/llms.txt
> Use this file to discover all available pages before exploring further.

# 快速开始

> 按照这些分步说明，快速高效地开始使用 Agnes AI API。

<Note>
  **前置条件**

  在发起任何 API 请求之前，请确保你已具备以下条件：

  * 一个有效的 Agnes AI 平台账户
  * 一个有效的 API 密钥（在 Agnes AI 开发者控制台中生成）
</Note>

<Steps>
  <Step title="创建账户">
    注册一个新账户，或登录你现有的 Agnes AI 平台账户。从开发者控制台，你可以管理 API 密钥、账单等。
  </Step>

  <Step title="生成 API Key">
    要认证你的 API 请求，请在 Agnes AI 平台中生成一个密钥 API Key：

    请妥善保存此密钥。你将使用它来认证所有 API 请求（如认证部分所述）：

    <span class="field-row"><code>Authorization: Bearer YOUR\_API\_KEY</code></span>
  </Step>

  <Step title="发起你的第一个请求">
    以下是使用 `curl` 创建聊天补全的示例请求（你也可以使用 Postman、Python requests 或其他 HTTP 客户端）：

    ```bash theme={null}
    curl https://api.agnes-ai.cn/v1/chat/completions \
    -H "Authorization: Bearer YOUR_API_KEY" \
    -H "Content-Type: application/json" \
    -d '{
        "model": "agnes-2.5-flash",
        "messages": [
          {
            "role": "user",
            "content": "你好！"
          }
        ]
      }'
    ```

    <Tip>
      在运行请求之前，请将 `YOUR_API_KEY` 替换为你实际的 API 密钥。成功的响应将返回与你输入匹配的聊天补全结果。
    </Tip>
  </Step>

  <Step title="后续步骤">
    在你的第一个请求之后，探索以下后续步骤以充分利用 Agnes AI API：

    * 阅读文档了解每个 API 端点的请求参数、响应格式和错误处理。
    * 集成流式响应或工具调用等高级功能，以增强你的应用功能。
  </Step>
</Steps>
