# My Agnes Studio · 项目提示词遵循文档

> 任何接手本项目的 AI 助手（包括我自己）在迭代前必须先通读本文件，并严格遵守其中的规范。

---

## 1. 项目概述

**My Agnes Studio** 是一个单文件 Web 应用，提供"无限画布 + 多模态 AI 生成"的工作台体验。

- **品牌标语**：前沿模型 免费畅用 让AI属于每个人
- **硬件形态**：单文件 HTML（`agnes-studio.html`），无构建步骤、无外部 JS 框架。

- **核心能力**
  - 在画布上自由拖拽、连线、组合节点
  - 调用 Agnes 文本模型改写/生成提示词
  - 调用 Agnes 图像模型做文生图 / 图生图
  - 调用 Agnes 视频模型做文生视频 / 图生视频 / 关键帧动画
- **形态**：所有功能都内联在一个 HTML 文件里，无构建步骤、无外部 JS 框架。
- **目标用户**：数字内容创作者、产品/设计/运营，需要快速把"灵感 → 图 → 视频"串起来的人。

---

## 2. 技术栈

| 维度 | 选型 | 说明 |
| --- | --- | --- |
| 容器 | 单 HTML 文件 | `agnes-studio.html` |
| 样式 | 原生 CSS + CSS 变量 | 深色主题，变量集中在 `:root` |
| 逻辑 | 原生 JavaScript（ES2020+） | 不依赖 Vue/React/jQuery |
| HTTP | `fetch` + 自封装 `callApi / callApiWithRetry` | 处理超时与重试 |
| 持久化 | `localStorage`（API Key + 画布 JSON） | 不引入 IndexedDB / 后端 |
| 节点渲染 | DOM + CSS 变换 | 自实现画布平移/缩放 |

> **禁止**：引入 npm 依赖、引入打包工具、拆出多个 JS/CSS 文件。如确有必要，先和用户确认。

---

## 3. Agnes 模型 API 集成规范

### 3.1 通用配置

```js
// agnes-studio.html 中已固化
const baseUrl = 'https://apihub.agnes-ai.com/v1';

// 请求头（每次请求必带）
{
  'Authorization': 'Bearer ' <API_KEY>,  // 用户在前端输入,存 localStorage
  'Content-Type': 'application/json'
}
```

- API Key **不要硬编码**，由用户在设置面板输入后保存到 `localStorage`。
- 所有 API 调用必须经过统一的 `callApiWithRetry`，避免散落 `fetch`。

### 3.2 文本模型 `agnes-2.0-flash`

| 项目 | 值 |
| --- | --- |
| 端点 | `POST /v1/chat/completions` |
| 上下文 | 512K |
| 用途 | 提示词改写、占位符替换、镜头动作生成、JSON 化提取 |

**常用模式**：
- 多轮对话：`messages: [{role}, ...]`
- 流式输出：`stream: true`
- 工具调用：`tools: [...]`（按需启用）
- 思考模式：`chat_template_kwargs: { enable_thinking: true }`

**提示词结构（项目里默认遵循）**：
```
[角色] + [任务] + [上下文] + [要求] + [输出格式]
```

### 3.3 图像模型 `agnes-image-2.1-flash`

| 项目 | 值 |
| --- | --- |
| 端点 | `POST /v1/images/generations` |
| 用途 | 文生图（T2I）、图生图（I2I） |

**请求结构**：
```jsonc
{
  "model": "agnes-image-2.1-flash",
  "prompt": "...",
  "size": "1024x768",
  "return_base64": true,          // 仅文生图 Base64 输出
  "extra_body": {
    "response_format": "url" | "b64_json",
    "image": ["https://..."]      // 仅图生图;支持 URL 或 data:image/...;base64,...
  }
}
```

**铁律**：
- `response_format` **必须**放在 `extra_body` 内，不允许出现在请求体顶层。
- 图生图 **不需要** 传 `tags: ["img2img"]`，只需提供 `extra_body.image`。
- 客户端超时建议 `60s - 360s`，因为高密度图像生成较慢。

**推荐提示词结构**：
- 文生图：`[主体] + [场景/环境] + [风格] + [光照] + [构图] + [质量要求]`
- 图生图：`[改变要求] + [新风格/场景] + [新增/移除元素] + [保留元素]`

### 3.4 视频模型 `agnes-video-v2.0`

视频是**异步任务**，必须两步走：

| 步骤 | 端点 | 说明 |
| --- | --- | --- |
| 1. 创建任务 | `POST /v1/videos` | 返回 `video_id` / `task_id` |
| 2. 获取结果 | `GET /agnesapi?video_id=<VIDEO_ID>` | 推荐用法 |
| 兼容旧版 | `GET /v1/videos/<TASK_ID>` | 仍可用，但不再推荐 |

**任务状态**：`queued` → `in_progress` → `completed` / `failed`。
**完成时返回的视频 URL** 在 `remixed_from_video_id` 字段。

**关键参数约束**：
- `num_frames` 必须 ≤ 441，且满足 `8n + 1`（81 / 121 / 241 / 441）
- 时长计算：`seconds = num_frames / frame_rate`
- `frame_rate` 范围 `1 - 60`，推荐 `24`
- 默认分辨率档位：`480p` / `720p` / `1080p`，支持 `16:9` / `9:16` / `1:1` / `4:3` / `3:4`

**轮询策略（项目内已实现）**：
- 创建任务后进入轮询（建议 3-5 秒间隔）
- 用 `video_id` 查询 `GET /agnesapi?video_id=...`
- `progress` 字段用于 UI 进度条
- `status === 'completed'` 时取 `remixed_from_video_id` 写入节点

**提示词结构**：
- 文生视频：`[主体] + [动作] + [场景] + [镜头运动] + [光线] + [风格]`
- 图生视频：明确"哪些动、哪些保持稳定"

---

## 4. 文件结构与版本管理

### 4.1 当前文件清单

```
D:\fireToken\agnes-studio\
├── README.md                    # 一页速览（新人上手 + 功能亮点 + 常见操作）
├── AGENTS.md                    # 本文件（项目遵循文档）
├── SPEC.md                      # 功能规格文档（数据模型 / 改点速查 / 陷阱清单）
├── agnes-studio.html            # 🔴 主文件 / 活跃版本
├── docs\                        # 官方 API 文档本地副本（详见 API 参考资料节）
│   ├── agnes-2.0-flash.md
│   ├── agnes-image-2.1-flash.md
│   ├── agnes-video-v2.0.md
│   ├── llms-index.md
│   └── video-super-resolution-tutorial.md  # 视频超分辨率实战教程（FFmpeg / ComfyUI+GAN / FlashVSR / Video-Compare）
└── history\                     # 历史版本归档（按 v1、v2 ... 递增）
    ├── agnes-studio-v1.html
    └── agnes-studio-v2.html
```

### 4.2 ⚠️ 版本备份规则（强制）

> **每次对 `agnes-studio.html` 进行任何修改之前，必须先备份当前版本到 `history/`。**

**命名规范**：
```
history\agnes-studio-v<N>.html
```
- `<N>` 从 `1` 开始递增（`v1`、`v2`、`v3` …）
- 同一天多次小步迭代，可在 `vN` 后加小数（`v3.1`、`v3.2`），但 **不要** 把活跃版本号搞乱
- 历史版本统一存放在 `history/` 子目录下，**不要** 散落在项目根目录

**备份流程（每次都要走一遍）**：

```powershell
# 1. 复制当前主文件为带版本号的快照（存到 history/）
$N = 3   # ← 下一次递增的版本号，每次手动 +1
Copy-Item `
  -Path "D:\fireToken\agnes-studio\agnes-studio.html" `
  -Destination "D:\fireToken\agnes-studio\history\agnes-studio-v$N.html" `
  -Force
```

```bash
# Git Bash 等价写法
cp agnes-studio.html history/agnes-studio-v3.html
```

2. 在 `agnes-studio.html` 上做实际修改
3. 修改完成后，人工验证关键功能（节点拖拽 / 图像生成 / 视频生成）
4. **绝不**直接在 `history\agnes-studio-vN.html` 上修改历史版本
5. 跨里程碑改动完成后，**主动**把新增的 `vN.html` 移入 `history/`（如果上面 `Copy-Item` 没指定到 `history\` 路径，需要 `Move-Item` 一下）

### 4.3 历史版本归档原则

- 每个 **里程碑级** 改动（新增模态、重构节点系统、改 API 协议）单独一个 `vN`
- **小修小补**（改文案、调整样式）合并到上一个 `vN`，不需要每次都新建
- 累计保留至少最近 5 个版本；旧版本可用 `mavis-trash` 清理（先和用户确认）
- `history/` 目录不进 git（如果未来加 git 的话），避免仓库膨胀

---

## 5. 关键代码约定

### 5.1 API 调用

- **统一入口**：所有请求走 `callApi(path, opts)` 或 `callApiWithRetry(path, opts, opts)`
- **重试**：网络错误（`Failed to fetch` / `NetworkError`）自动重试；HTTP 4xx/5xx 透传给 UI
- **超时**：视频生成相关请求 60-360s；文本/图像 30-120s
- **错误展示**：用户看到的提示必须是友好中文，不能直接把 `JSON.stringify(err)` 甩出去

### 5.2 节点系统（数据模型）

```js
node = {
  id: string,            // 唯一 id
  type: 'image'|'video'|'text'|'external',
  x: number, y: number,  // 画布坐标
  w: number, h: number,
  prompt: string,        // 原始提示词
  sourceParams: object,  // 生成时的原始参数（size/num_frames/seed...）
  src: string,           // 图像/视频的 URL 或 data URI
  status: 'pending'|'running'|'done'|'failed',
  refs: [nodeId],        // 引用的上游节点 id
  extra: object          // 模态专属字段
}
```

- 节点全部存在 `nodes` 数组里
- 引用关系通过 `id` 维护，**不要**保存 DOM 引用
- 占位符 `{{ref:nodeId}}`、`{{select}}` 在生成前替换

### 5.3 提示词工程

- 文生图必须按 `[主体]+[场景]+[风格]+[光照]+[构图]+[质量]` 结构组织
- 图生图必须显式写"保留原始构图/相机角度"
- 视频提示词必须描述"运动 + 稳定"
- 镜头动作（`cameraMotion`）作为可复用片段在改写时追加到 prompt 末尾，**不**覆盖用户已编辑的内容

### 5.4 样式

- 颜色、间距、圆角必须走 `:root` 下的 CSS 变量，禁止在具体选择器里写魔法值
- 深色主题调色板见 `:root`：`--bg-0/1/2/3/4`、`--text-0/1/2/3`、`--brand/--brand-2`
- 新增组件前先看现有同类组件，保持节奏一致

---

## 6. 开发流程（AI 助手操作 SOP）

接到迭代任务后，按下面顺序执行：

```
1. 读需求 → 若有歧义先和用户确认，不要自己猜
2. 读相关代码段（不要全文重读，按需 grep）
3. 🔴 备份当前 agnes-studio.html 为 agnes-studio-vN.html
4. 在 agnes-studio.html 上实现
5. 自测：
   - 文生图：生成一张，看是否落库 + 占位符正确
   - 图生图：选一张参考图 + prompt，看构图是否保留
   - 文生视频：生成 3s 短视频，看状态轮询 + 进度条
   - 图生视频：同上 + 验证 motion 描述生效
   - 提示词改写：含 {{ref:xxx}} 时替换正确
6. 若自测失败：修复后 **不**再新建备份（已在第 3 步备份过）
7. 若跨模态改动（新增 API 端点 / 改协议）：同步更新本文件第 3 节
```

---

---

## API 参考资料（官方文档 · 离线副本）

> 项目内所有模型协议的"权威源"。第 3 节只是提炼版，遇到边界/异常时**必须回到这里核对**。
> 离线副本存放在 `docs/`，跟代码同步版本管理。

| 模型 / 文档 | 本地副本 | 官方 URL（英文源） | 官方 URL（中文镜像） |
| --- | --- | --- | --- |
| **Agnes 2.0 Flash**（文本） | [`docs/agnes-2.0-flash.md`](docs/agnes-2.0-flash.md) | https://wiki.agnes-ai.com/en/docs/agnes-20-flash.md | https://agnes-ai.com/zh-Hans/docs/agnes-20-flash |
| **Agnes Image 2.1 Flash**（图像，当前在用） | [`docs/agnes-image-2.1-flash.md`](docs/agnes-image-2.1-flash.md) | https://wiki.agnes-ai.com/en/docs/agnes-image-21-flash.md | https://agnes-ai.com/zh-Hans/docs/agnes-image-21-flash |
| **Agnes Video V2.0**（视频，当前在用） | [`docs/agnes-video-v2.0.md`](docs/agnes-video-v2.0.md) | https://wiki.agnes-ai.com/en/docs/agnes-video-v20.md | https://agnes-ai.com/zh-Hans/docs/agnes-video-v20 |
| 官方文档索引（llms.txt） | [`docs/llms-index.md`](docs/llms-index.md) | https://wiki.agnes-ai.com/llms.txt | — |

### 维护约定

- **本地副本优先**：在 `docs/` 里离线查阅，不要每次都上网拉，节省 token 也避免官方改版导致内容漂移。
- **冲突时以本地为准**：项目代码已经按某版本快照调通过；本地副本变了，先看变更记录，再决定是否同步代码。
- **更新流程**：当官方发布新版（新增字段、改端点、改约束），用 `webfetch` 拉最新 `.md` → 覆盖本地副本 → 在本文档"变更记录"里登记日期 + 改动摘要。
- **下载命令范式**（PowerShell + webfetch）：
  ```
  webfetch https://wiki.agnes-ai.com/en/docs/<file>.md  →  保存到 docs/<file>.md
  ```
- **未在本地的相关文档**（按需补）：Agnes Image 2.0 Flash、错误码 `code.md`、FAQ、Quickstart 等。

---

## 7. 常见错误与排查

| 现象 | 原因 | 排查 |
| --- | --- | --- |
| 图生图返回"unknown parameter" | `response_format` 放在了请求体顶层 | 改到 `extra_body.response_format` |
| 图生图完全没变化 | 传了 `tags: ["img2img"]` | 删掉，只用 `extra_body.image` |
| 视频一直 `queued` 不动 | `num_frames` 不满足 `8n + 1` | 改为 81 / 121 / 241 / 441 |
| 401 Unauthorized | API Key 未配置 / 错误 | 检查 localStorage 里 `agnes_api_key` |
| 图像 base64 写入节点后不可见 | 没去掉 `data:image/...;base64,` 前缀 | 渲染 `<img>` 时要保留完整前缀 |
| 跨域 CORS 报错 | 直接 fetch 了 `storage.googleapis.com` 资源 | 用 `<img>` / `<video>` 标签加载，不要 fetch 转存 |

---

## 8. 后续可扩展方向（备忘，不强求）

- 多画布/项目管理（左侧栏树状结构）
- 节点对齐辅助线 + 吸附
- 关键帧动画的可视化编辑器
- 历史版本回滚（一键从 `agnes-studio-vN.html` 恢复）
- 导出为 JSON / 重新导入
- Agnes 新模型接入：保持第 3 节"先文档、再代码"的节奏

---

## 9. 变更记录

| 版本 | 日期 | 变更摘要 |
| --- | --- | --- |
| v1 | 2026-07-04 | 项目初始化文档；首次备份 `agnes-studio.html` → `history/agnes-studio-v1.html` |
| v1.1 | 2026-07-04 | 新增"API 参考资料"节；下载 3 个模型官方文档 + llms 索引到 `docs/`（含 frontmatter 来源元数据）；本地副本路径见上表 |
| v1.2 | 2026-07-04 | 代码审查报告：发现 3 个严重问题（API Key 硬编码、`fetchVideoStatus` 写死 baseUrl、无重试）+ 7 个可精简点；详见会话记录，未落地修复 |
| v1.3 | 2026-07-04 | 落地修复 v1.2 中标注的 2 个 🔴 严重问题：(1) 清空 `DEFAULTS.apiKey` 默认值，加启动引导 toast；(2) `fetchVideoStatus` 改走 `callApi` + `CFG.baseUrl`（精简 19 行→7 行，端点切换到 `/v1/videos/{id}` 兼容版）。同时新增 `history/` 目录规范，所有历史版本归档到 `history\agnes-studio-vN.html`；当前已搬入 v1、v2 两份 |
| v1.4 | 2026-07-04 | 简化提示词建议 UI：7 张 `.prompt-tpl` 卡片改用 `muted small` 行内嵌结构（方案 A）；CSS 删 8 个废弃 class（`.prompt-tpl` / `.tpl-title` / `.tpl-lang` / `.prompt-tpl pre` / `.tpl-formula` / `.tpl-formula-label` / `.tpl-example-label` / `.tpl-example`），保留 `.tpl-use` 并改为透明下划线链接样式；总文件 -1952 字节 / -69 行；JS 0 改动（事件委托 `.tpl-use` 不受影响） |
| v1.5 | 2026-07-04 | 公式补全：顶部"提示词建议"3 行与官方文档对齐（文生图补 `/环境`+`质量要求`，图生图补 2 段，文生视频 `镜头`→`镜头运动`）；5 张缺公式的范例各加 1 行公式（高信息密度 6 元素提炼、文生视频抄官方、图生视频/多图视频/关键帧动画从官方范例提炼）；HTML +5 行；JS/CSS 0 改动 |
| v1.6 | 2026-07-04 | 提示词建议模式联动：7 张范例 div 加 `tpl-block` class + `data-modes` 属性（文生图=t2i、图生图=i2i、高信息密度=t2i,i2i,multi、文生视频=t2v、图生视频=i2v、多图视频=keyframes、关键帧动画=keyframes），顶部 3 行公式加 `tpl-formula` class + `data-mode` 属性；新增 `updatePromptTplVisibility()` 函数（10 行）按当前 `state.mode` + `imageMode/videoMode` 过滤显示，3 处调用点（updateImageMode、updateVideoMode、switchMode）；HTML +17 行 / +1161 字节 |
| v1.7 | 2026-07-04 | 提示词建议改为静态纯展示（按用户决策 1A 2A 3B）：(1) 7 张范例全部去掉 `<button class="tpl-use">→ 填入</button>` 按钮、去掉 `tpl-block` class 和 `data-modes` 属性，公式和范例文字用 `<span class="kbd">` 高亮包起来；(2) 顶部"文生图/图生图/文生视频"3 行公式块保留删除（v6 已删除，v7 不再恢复）；(3) 删除 `updatePromptTplVisibility()` 函数 + 3 处调用 + tpl-use 事件委托 + CSS `.tpl-use` 样式；HTML/CSS/JS 净 -59 行 / +703 字节 |
| v1.8 | 2026-07-04 | 恢复提示词建议模式联动（按用户最新决策）：v1.7 删掉的"按生成模式过滤显示"功能加回来——7 张范例重新加 `tpl-block` class + `data-modes` 属性（多图视频→keyframes），新增 `updatePromptTplVisibility()` 函数定义 + 3 处调用（updateImageMode / updateVideoMode / switchMode）。**保留 v1.7 决定**：无"→ 填入"按钮、无顶部公式块、无 tpl-use 事件委托；HTML/CSS/JS 净 +15 行 / +1411 字节 |
| v1.9 | 2026-07-04 | 默认生成模式调整：`#imageMode` 默认选中从 `i2i`（图生图）改为 `t2i`（文生图）；`#videoMode` 默认选中从 `i2v`（图生视频）改为 `t2v`（文生视频）。**不动** applyCanvas 反序列化逻辑，已保存画布的用户设置仍按 localStorage 恢复 |
| v1.10 | 2026-07-04 | **回退**：修复"按键失效"时引入的改动（createNode 接受 id 参数）被用户撤销，回到 v1.9 状态。AGENTS.md 删除 v1.10 后重新加入本行仅为保留追溯链 |
| v1.11 | 2026-07-05 | 临时恢复默认 API Key（按用户要求"先用默认令牌"）：`DEFAULTS.apiKey` 从 `''` 改回 v1.3 之前的硬编码 Key `sk-N7ai7nAl...`；同步删掉 v1.3 加的"启动引导 toast"（Key 不为空时永远不触发，已成死代码）。**安全提示**：这个 Key 是真实令牌，会随源码/git/分享传播被滥用；用户应尽快从 Agnes 控制台重置，并在 v1.12 把 apiKey 重新置空 |
| v1.12 | 2026-07-05 | 运镜提示词改为中文：marker 里从英文 `CAMERA_MOTION_EN[id]` 改为 `相机${CM_CN_NAME[id]}`（如"相机固定镜头"）；`buildFinalPrompt` fallback 分支同步改为 `${prompt}, 相机${CM_CN_NAME[id]}`；`retryVideoFromParams` 里 `cmEn` 改为 `cmCn` 并用 `相机${cmCn}` 拼接。同步删除现已无人引用的 `CAMERA_MOTION_EN` 常量（30 行英文描述）。**注意**：当前"相机"+短名是机械拼接，对以"镜头"开头的运镜（如"镜头上摇"→"相机镜头上摇"）会有重复字样，可后续优化 |
| v1.13 | 2026-07-05 | 去掉外层 marker 标签 + 精简运镜代码：marker 体系（`CM_MARKER_RE` + `buildFinalPrompt`）全删，运镜直接以 `, 相机XX` 拼接在 prompt 末尾；新增 `state.cmSelectedId` 跟踪当前选中；`applyCameraMotionToPrompt` 重写（剥离旧 + 追加新 + 同步 select 值）；`updateCameraMotionChip` 改从 state 读；`doGenerateVideo` 不再调 `buildFinalPrompt`（prompt 已是最终形态）；`reversePromptFromNode` / `fillPromptFromNodeLang` 清空 `state.cmSelectedId`；videoCameraMotion onchange / btnRemoveCameraMotion / cmClearBtn 清理冗余 `updateCameraMotionChip()` 调用。净 -36 行 |
| v1.14 | 2026-07-05 | 修复 v1.13 引入的 `ReferenceError: cameraMotion is not defined`：脚本替换 `buildFinalPrompt` 那段时把 `const cameraMotion = $('#videoCameraMotion')?.value` 一起删了，但 `sourceParams.cameraMotion`（line 2403 重生成复用场景）还在引用。修复：从 `state.cmSelectedId` 读取 cameraMotion，作为 `sourceParams.cameraMotion` 的值同步保存 |
| v1.15 | 2026-07-05 | 修复视频轮询 `task_not_exist` 错误：Agnes 网关返回的 `video_id` 字段是 LiteLLM 伪 ID（base64 编码 `litellm:custom_llm_provider:openai;...;video_id:video_xxx`），直接传给 `/v1/videos/{id}` 后端找不到。新增 `extractRealVideoId()` 解码提取真正的 video_id；`fetchVideoStatus` 切回 Agnes 官方推荐的 `/agnesapi?video_id=` 端点（不在 /v1 路径下）；`doGenerateVideo` 创建任务后立即解码存到 `task.videoId`（后续轮询/重生成/recover 都拿到的是真 ID） |
| v1.16 | 2026-07-05 | 视频重试复用原节点（与图片逻辑一致）：`doGenerateVideo` 加 `reuseNodeId` 参数支持，失败节点点重试时重置 `src/status/progress` 保留位置 + 复用原 task；`retryVideoFromParams` 改为临时同步 UI（videoMode / videoTier / videoRatio / videoFrames / videoFps / videoSeed / negativePrompt / videoRefIds / cmSelectedId）+ 调用 `doGenerateVideo({ reuseNodeId: origId })` + finally 恢复，不再自己构造 body / 创建新节点 / 调 `createVideoTask` |
| v1.17 | 2026-07-05 | 修复自动恢复画布异常：原 `tryAutoRestore` 用 `confirm()` 弹模态对话框，在某些浏览器（弹窗拦截 / 焦点切换 / 无头模式）下会被静默 dismiss 或返回 false，导致 applyCanvas 不执行或部分执行，画布只显示 1 个不可操作的节点。改为：去掉 confirm，直接 `applyCanvas(data)` 自动恢复 + 延迟 500ms toast 提示用户（不再阻塞脚本）。同时 catch 块增加 toast 报错（之前只 console.warn） |
| v1.18 | 2026-07-05 | 修复 `Cannot access 'promptPopovers' before initialization`：原 `tryAutoRestore` 是脚本顶层 IIFE 立即执行，调用 `applyCanvas → createNode → renderNode → ensurePromptPopover → promptPopovers`，但 `promptPopovers`（const）定义在 line 3457，在 tryAutoRestore（line 2981）之后，处于 TDZ。改为：tryAutoRestore 用 `setTimeout(fn, 0)` 包一层，延迟到下一个事件循环 tick，那时所有顶层 const 都已初始化 |
| v1.19 | 2026-07-05 | 删除左侧顶部"⟲ 反推"按钮；新增**文本生成**功能（`agnes-2.0-flash`）：modeSwitch 改为三模态（图像/视频/文本）；新增 `#textParams` 面板（系统角色 5 选 1 + 自定义 + 温度 0-2 + max_tokens 64-8192 + 多轮对话 toggle + 附加图片理解）；新增 `#textConversation` 区域显示历史对话气泡；`doGenerateText` 函数支持**流式输出**（SSE 解析 + 自动滚动）+ **多轮对话**（`state.textHistory` 累积）+ **图像理解**（user message content 数组含 `image_url`）；节点上的反推按钮（每个 image 节点 ↺）继续可用 `reversePromptFromNode`。新 CSS：`.text-msg` 三角色（user/assistant/system）+ 流式光标闪烁动画。+777 行 |
| v1.20 | 2026-07-05 | 修复 v1.19 漏写 `<input type="file" id="textImageInput" hidden>` 导致 `Cannot set properties of null (setting 'onchange')` 报错：补上 hidden input 元素（`#btnTextUploadImage` 的 click 触发 + `onchange` 监听都依赖它） |
| v1.21 | 2026-07-05 | 加 prompt 安全检查（v1.21）：定义 `PROMPT_RISK_KEYWORDS`（6 类高风险模式：360 度环绕 / 具体年龄段 / 写实脸部细节 / 成人内容 / 血腥暴力 / 儿童）+ `preCheckPrompt()` + `showPromptRiskWarning()`；`doGenerateImage` / `doGenerateVideo` 开头调用，命中时 toast 警告（不阻止生成）。**这是粗筛**，仅供参考；真实拒绝原因需看 Console |
| v1.22 | 2026-07-05 | 加反向提示词模板（v1.22）：`#negativePrompt` 输入框旁新增"📋 模板"按钮，`NEGATIVE_PROMPT_PRESETS` 含 3 个预设（general / face / cinematic，当前用 general），点击一键填入并 toast 提示 |
| v1.23 | 2026-07-05 | 系统角色精简（v1.23）：删除 `copywriter`（文案写手）和 `coder`（代码助手），新增 3 个跟 Agnes Studio 创作场景更贴合的角色——`storyboard`（分镜描述专家：5 要素分镜模板）/ `script_polish`（影视脚本优化专家：5 维度优化）/ `image_reverse`（图片反推：英文 prompt 50-100 词格式，参照 Agnes Image 2.1 Flash 文档）。保留 `assistant` / `translator` / `custom` |
| v1.24 | 2026-07-05 | 角色联动（v1.24）：1）`doGenerateText(prompt, opts={})` 加 `opts.role` 覆盖 + `opts.silent` 跳过"文本生成完成"toast；2）新增 `extractAndApplyCameraMotion(text)`，遍历 `CM_CN_NAME` key 找第一个出现在文本里的英文 id，命中后调 `applyCameraMotionToPrompt` 选中；3）**联动 1**——`storyboard` 输出完成后自动调用提取函数，第一个匹配的运镜 id 自动追加到 `#prompt` 并 toast 提示；4）**联动 2**——`script_polish` 输出完成后 800ms 自动追加一次 `doGenerateText` 用 `role='storyboard'` 拆解分镜（silent=true 避免 toast 重复），自动追加到 chat history；5）`storyboard` system prompt 升级为严格格式（**镜头必须用英文 id**，如 `dolly_in / pan_left / static_shot`），保证提取函数能识别 |
| v1.25 | 2026-07-05 | 文档体系完善（不动 `agnes-studio.html`）：新增 [`README.md`](./README.md)（一页速览 + 快速开始 + 主要功能 highlight + 常见操作）+ 新增 [`SPEC.md`](./SPEC.md)（15 节规格文档：文件结构、布局、state、节点系统、三模式面板、API 集成、角色联动、运镜库、持久化、修改入口索引、已知陷阱、功能清单、Backlog）；AGENTS.md §4.1 同步登记 README.md + SPEC.md |
| v1.26 | 2026-07-05 | 品牌升级：软件名 `Agnes Studio` → `My Agnes Studio`；新增品牌标语"前沿模型 免费畅用 让AI属于每个人"在顶部品牌旁显示（主标题 + 小号副标题双行布局）。变更范围：① `agnes-studio.html` 浏览器标题、顶栏品牌结构；② `README.md` 主标题与简介；③ `AGENTS.md` §1 项目概述 + 顶部标题；④ `docs/SPEC.md` 文档标题。HTML/CSS +19 行（CSS `.brand-text`/`.brand-title`/`.brand-tagline` + 顶栏 div 结构） |
|