> ## Documentation Index
> Fetch the complete documentation index at: https://wiki.agnes-ai.cn/llms.txt
> Use this file to discover all available pages before exploring further.

# 概述

> Agnes AI API 在线文档

## 1. Agnes AI API 介绍

Agnes AI API 为开发者提供统一、稳定、易于集成的多模态 AI 模型服务，支持文本、图像、视频和多模态生成与理解能力。

通过 Agnes AI API，开发者可以快速构建 AI 原生应用，包括但不限于：

* AI 聊天和文本生成
* 逻辑推理和内容理解
* 文生图和图像编辑
* 图生视频和视频生成
* 音视频同步生成
* Agent 工具和自动化工作流
* 创意内容生成和多模态交互应用

Agnes AI API 兼容 OpenAI 风格的接口，开发者可以轻松迁移和集成现有项目，只需极少的代码修改，降低开发成本，提高集成效率。

## 2. 核心能力

Agnes AI API 目前支持以下核心能力：

<CardGroup cols={2}>
  <Card title="3.1 文本生成与推理">
    支持高质量文本生成、内容续写、摘要总结、逻辑推理、问答、代码辅助和 Agent 任务执行。

    适用场景包括：

    * AI 聊天助手
    * 内容创作
    * 文档摘要
    * 智能问答
    * 代码生成
    * Agent 自动化任务
  </Card>

  <Card title="3.2 图像生成与编辑">
    支持根据文本提示生成高清图像，以及对现有图像进行编辑、增强和风格化处理。

    适用场景包括：

    * 文生图
    * 图像编辑
    * 产品图生成
    * 创意海报生成
    * 人物图像生成
    * 社交媒体素材创作
  </Card>

  <Card title="3.3 视频与音视频同步生成">
    支持生成高质量视频内容和同步音视频输出，降低后期制作复杂度，提高内容创作效率。

    适用场景包括：

    * AI 视频生成
    * 图生视频
    * 短视频创作
    * 创意广告素材
    * 角色动画
    * 音视频同步内容生成
  </Card>

  <Card title="3.4 多模态理解与创作">
    支持文本、图像、视频和推理能力的结合，帮助开发者构建更智能、更自然的交互体验。

    适用场景包括：

    * 多模态 AI 助手
    * 图像理解
    * 视频理解
    * 创意工作流
    * AI 社交互动
    * 教育、娱乐和生产力工具
  </Card>
</CardGroup>

## 3. 模型能力概览

Agnes AI 提供多种类型的模型能力，包括文本模型、图像模型、视频模型和多模态模型。

Agnes AI 模型套件包括文本、图像、视频和多模态模型，覆盖推理、创意生成和生产应用场景。

这些模型帮助开发者快速集成完整的 AI 能力栈，从内容理解和文本生成，到图像生成、图像编辑和视频生成。

## 4. API 兼容性

Agnes AI API 兼容 OpenAI 风格的接口。

如果你已经使用过 OpenAI 兼容 API，通常只需要修改以下配置：

* `Base URL`
* `API Key`
* `模型名称`

这种兼容性让开发者可以快速迁移现有项目，降低集成成本，避免重复开发工作。

## 5. Base URL

所有 API 请求应使用以下 Base URL 发送：

请确保你的请求路径基于此地址正确构建，以避免因路径错误导致请求失败。

## 6. 认证

所有 API 请求必须使用 API Key 进行认证。

在请求头中包含以下参数，将 `YOUR_API_KEY` 替换为你的有效 API Key：

<span class="field-row"><code>Authorization: Bearer YOUR\_API\_KEY</code></span>

## 7. 安全提示

<Warning>
  你的 API Key 是敏感信息，请妥善保管。

  请勿在以下场景暴露你的 API Key：

  * 公开代码仓库
  * 前端客户端代码
  * 截图或屏幕录制
  * 公开文档
  * 他人可访问的配置文件

  如果你的 API Key 不慎泄露，建议立即在控制台中删除或重置，以防止未经授权的使用。
</Warning>

## 8. 快速开始

你可以按照以下步骤开始使用 Agnes AI API：

<Steps>
  <Step title="创建 API Key">
    登录 Agnes AI 控制台，进入 API Key 管理页面，创建并复制你的 API Key。
  </Step>

  <Step title="选择模型">
    根据你的使用场景选择合适的模型，例如文本模型、图像模型、视频模型或多模态模型。
  </Step>

  <Step title="配置请求 Base URL">
    将 API Base URL 设置为：

    <span class="field-row"><code>[https://api.agnes-ai.cn/v1](https://api.agnes-ai.cn/v1)</code></span>
  </Step>

  <Step title="发送 API 请求">
    在请求头中添加你的 API Key，使用 OpenAI 兼容的 API 格式发送请求。
  </Step>
</Steps>

## 9. 适用开发场景

Agnes AI API 适用于以下产品和业务场景：

* AI 聊天应用
* AI 搜索和研究工具
* AI 写作和办公生产力工具
* AI 图像生成工具
* AI 视频生成工具
* AI 角色和交互应用
* AI 社交产品
* Agent 自动化平台
* 创意内容生产平台
* 教育、娱乐、电商和营销工具

## 10. 文档说明

本文档帮助开发者快速了解 Agnes AI API 的基本能力、集成方式和使用指南。

有关详细的模型参数、请求示例、响应格式和错误码说明，请参阅对应的模型专用集成文档。
