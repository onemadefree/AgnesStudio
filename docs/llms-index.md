<!--
source: https://wiki.agnes-ai.com/llms.txt
downloaded: 2026-09-13（v1.34 模型升级时刷新）
note: 官方文档索引；包含所有模型的入口和集成指南链接。后续如需新增模型/集成指南，按此列表补本地副本。
-->

# Agnes-AI Docs Index（2026-09 快照）

> 2026-09 官方大版本升级：`agnes-2.0-flash` 已废弃（→ 2.5 Flash）；图像新增 2.5 Flash（协议同 2.1 + 档位/ratio）；视频新增 2.5 / 2.5 Flash（**协议大改**：mode/seconds/size/aspect_ratio，不再用 num_frames/fps）。

| 模型 / 文档 | 本地副本 | 官方 URL（英文源） | 状态 |
| --- | --- | --- | --- |
| **Agnes 2.5 Flash**（文本，✅ 在用，替代 2.0） | `docs/agnes-25-flash.md` | https://wiki.agnes-ai.com/en/docs/agnes-25-flash.md | GA · 当前免费 |
| **Agnes 2.5 Pro**（文本旗舰） | `docs/agnes-25-pro.md` | https://wiki.agnes-ai.com/en/docs/agnes-25-pro.md | 付费 |
| **Agnes 2.5 Pro Beta**（文本） | `docs/agnes-25-pro-beta.md` | https://wiki.agnes-ai.com/en/docs/agnes-25-pro-beta.md | 付费 |
| **Agnes 3.0 Flash**（文本，✅ 在用） | `docs/agnes-30-flash.md` | https://wiki.agnes-ai.com/en/docs/agnes-30-flash.md | 当前免费 |
| Agnes 2.0 Flash（文本，**已废弃**） | `docs/agnes-2.0-flash.md` | https://wiki.agnes-ai.com/en/docs/agnes-20-flash.md | 不要再用于新集成 |
| **Agnes Image 2.5 Flash**（图像，✅ 在用） | `docs/agnes-image-25-flash.md` | https://wiki.agnes-ai.com/en/docs/agnes-image-25-flash.md | 当前免费 |
| Agnes Image 2.1 Flash（图像，上一代） | `docs/agnes-image-21-flash.md` | https://wiki.agnes-ai.com/en/docs/agnes-image-21-flash.md | 仍可用 |
| **Agnes Video 2.5**（视频旗舰） | `docs/agnes-video-25.md` | https://wiki.agnes-ai.com/en/docs/agnes-video-25.md | 按秒计费 |
| **Agnes Video 2.5 Flash**（视频，✅ 在用） | `docs/agnes-video-25-flash.md` | https://wiki.agnes-ai.com/en/docs/agnes-video-25-flash.md | 限时免费 · 仅 720P |
| Agnes Video V2.0（视频，旧协议） | `docs/agnes-video-v2.0.md` | https://wiki.agnes-ai.com/en/docs/agnes-video-v20.md | 仍可用 · 免费 |
| Overview | `docs/overview.md` | https://wiki.agnes-ai.com/en/docs/overview.md | — |
| Quickstart | `docs/quickstart.md` | https://wiki.agnes-ai.com/en/docs/quickstart.md | — |
| Model Pricing | _未下载_ | https://wiki.agnes-ai.com/en/docs/pricing.md | 计费规则 |
| 通用错误码 | _未下载_ | https://wiki.agnes-ai.com/en/docs/code.md | — |
| FAQs | _未下载_ | https://wiki.agnes-ai.com/en/docs/faqs.md | — |

集成指南（OpenClaw / Claude CLI / Cherry Studio 等 cid1-cid8）见 https://wiki.agnes-ai.com/llms.txt ，按需下载。

> 维护说明：当官方更新 API 协议或新增模型时，用 `curl https://wiki.agnes-ai.com/en/docs/<file>.md` 覆盖本地副本，并在 AGENTS.md 变更记录里登记日期。

---

## 2026-09-13 官方 llms.txt 原始内容（备份）

```text
# Agnes-Ai Docs

## Developer Guides

### Get Started

- [Overview](https://wiki.agnes-ai.com/en/docs/overview.md): Agnes AI API Online Documentation
- [Quickstart](https://wiki.agnes-ai.com/en/docs/quickstart.md): Follow these step-by-step instructions to start using the Agnes AI API quickly and efficiently.

### Use Guides

#### Text

- [Agnes 2.5 Flash](https://wiki.agnes-ai.com/en/docs/agnes-25-flash.md): Generally available upgrade of Agnes 2.0 Flash with improved coding capability, agent workflows, tool calling, and multimodal understanding.
- [Agnes 2.5 Pro Beta](https://wiki.agnes-ai.com/en/docs/agnes-25-pro-beta.md): Integrate Agnes 2.5 Pro Beta for advanced reasoning, coding, long-context analysis, agentic workflows, and multimodal understanding.
- [Agnes 2.5 Pro](https://wiki.agnes-ai.com/en/docs/agnes-25-pro.md): The commercial stable version of the benchmark-listed Agnes 2.5 Pro Beta model for advanced coding, scientific reasoning, long-context analysis, agentic workflows, and multimodal understanding.
- [Agnes 3.0 Flash](https://wiki.agnes-ai.com/en/docs/agnes-30-flash.md): A new-generation Agnes AI text model with stronger Agnes Code task execution, tool orchestration, and trustworthy delivery.

#### Image

- [Agnes Image 2.0 Flash](https://wiki.agnes-ai.com/en/docs/agnes-image-20-flash.md): A high-performance image generation and editing model for text-to-image, image-to-image, and multi-image composition workflows.
- [Agnes Image 2.1 Flash](https://wiki.agnes-ai.com/en/docs/agnes-image-21-flash.md): An upgraded image generation model optimized for high-information-density visuals, with support for text-to-image and image-to-image workflows.
- [Agnes Image 2.5 Flash](https://wiki.agnes-ai.com/en/docs/agnes-image-25-flash.md): The latest-generation Agnes image model, with capabilities that comprehensively exceed Agnes Image 2.1 Flash and support for text-to-image, image-to-image, and multi-image composition.

#### Video

- [Agnes Video V2.0](https://wiki.agnes-ai.com/en/docs/agnes-video-v20.md): An asynchronous video generation API for text-to-video, image-to-video, and keyframe animation.
- [Agnes Video 2.5](https://wiki.agnes-ai.com/en/docs/agnes-video-25.md): Integrate Agnes Video 2.5 through an OpenAI Videos-compatible API for text-to-video, keyframe control, and multimodal reference generation.
- [Agnes Video 2.5 Flash](https://wiki.agnes-ai.com/en/docs/agnes-video-25-flash.md): Integrate Agnes Video 2.5 Flash through an OpenAI Videos-compatible API for text-to-video, keyframe control, and image-reference generation.

## Integration

### Common Integration

- [OpenClaw Integration Guide](https://wiki.agnes-ai.com/en/docs/cid1.md): OpenClaw Model Configuration Guide
- [HermesAgents Integration Guide](https://wiki.agnes-ai.com/en/docs/cid2.md): HermesAgents Model Configuration Guide
- [Claude CLI Integration Guide](https://wiki.agnes-ai.com/en/docs/cid3.md): Claude CLI Integration Guide for Agnes Models
- [Claude Desktop  Integration Guide](https://wiki.agnes-ai.com/en/docs/cid4.md): Claude Desktop Integration Guide for Agnes Models
- [WorkBuddy Integration Guide](https://wiki.agnes-ai.com/en/docs/cid5.md): WorkBuddy Model Configuration Guide
- [Cherry Studio Integration Guide](https://wiki.agnes-ai.com/en/docs/cid6.md): Cherry Studio Integration Guide for Agnes Models
- [Opencode Integration Guide](https://wiki.agnes-ai.com/en/docs/cid7.md): Opencode Integration Guide for Agnes Models
- [Codex++ Integration Guide](https://wiki.agnes-ai.com/en/docs/cid8.md): Codex++ Agnes Model Configuration Guide

## Token Plan

- [Token Plan FAQ](https://wiki.agnes-ai.com/en/docs/tokenplan.md)

## Pricing

- [Model Pricing](https://wiki.agnes-ai.com/en/docs/pricing.md): List prices, current prices, and promotional billing rules for Agnes AI text, image, and video models on the international site.

## FAQs

- [FAQs](https://wiki.agnes-ai.com/en/docs/faqs.md): Frequently Asked Questions (FAQ)
- [Common Error Codes](https://wiki.agnes-ai.com/en/docs/code.md): Agnes API Common Error Codes and Solutions

## Legal

- [Privacy Policy](https://wiki.agnes-ai.com/en/docs/privacy-policy.md): Agnes AI Privacy Policy
- [Terms of Service](https://wiki.agnes-ai.com/en/docs/terms-of-service.md): Agnes AI Terms of Service
```
