# My Agnes Studio · 功能规格文档

> **基准版本**：v1.24（2026-07-05）
> **配套文件**：`agnes-studio.html`（4063 行 / ≈152 KB · 单文件 SPA）
> **配套遵循**：[`AGENTS.md`](./AGENTS.md)（备份规则 / API 协议 / 变更记录）
> **API 离线副本**：[`docs/`](./docs/)（agne­s-2.0-flash / image-2.1-flash / video-v2.0 + llms-index）
>
> 文档目的：把 `agnes-studio.html` 里**所有的功能边界、数据模型、模块关系、改点入口**梳理成可查阅 / 可修改的规格图，避免每次回到零。

---

## 0. TL;DR

- **形态**：单 HTML 文件，零依赖，深色主题，CSS Grid 3 列布局
- **核心体验**：画布（无限平移 / 缩放）+ 节点（图像 / 视频 / 反推 popover）+ 三模式生成（图像 / 视频 / 文本）
- **调用 3 个模型**（统一 baseUrl `https://apihub.agnes-ai.com/v1`）：
  - `agnes-image-2.1-flash`：文生图 / 图生图 / 多图融合（同步返回 base64 or URL）
  - `agnes-video-v2.0`：文生视频 / 图生视频 / 首尾帧（**异步**：`POST /v1/videos` → 轮询 `GET /agnesapi?video_id=`）
  - `agnes-2.0-flash`：文本聊天补全（**流式 SSE**）+ 多轮 + 图像理解 + 系统角色（含分镜描述 / 脚本优化 / 图片反推）
- **持久化**：4 个 localStorage key（配置 + 画布 JSON + 自动恢复）
- **33 种运镜**：注入到视频 prompt 末尾，节点可展示
- **附加功能**：反推（看图生中英双版 prompt）/ 翻译 / 失败节点一键重试 / 设置面板恢复历史 video_id

---

## 1. 文件结构（4063 行）

```
行  1-857     <style>    CSS 变量体系 + 全部组件样式
行 859-1281   <body>     顶部栏 + 三栏 + 4 个 modal + popover + toast
行 1282-1319  ===== 配置 & 工具 =====   DEFAULTS / CFG / $ / uid / toast
行 1320-1496  ===== API 集成 =====      callApi / retry / generateImage / reverseImageToPrompt / translateToChinese / createVideoTask / extractRealVideoId / fetchVideoStatus / extractVideoUrl / cleanBase64DataUrl
行 1497-1519  （小段）
行 1519-1688  ===== 状态 & 画布 =====    state / viewport / wheel zoom / pan / drag / select / fit / clear
行 1689-1917  ===== 节点 =====          createNode / renderNode / handleNodeAction / renderNodeMeta / copyNodeSeed
行 1918-1946  ===== prompt 安全检查 =====
行 1947-1963  ===== 反向提示词模板 =====
行 1963-2459  ===== 文本生成 =====      TEXT_ROLES / appendTextMsg / doGenerateText（流式 + 多轮 + 角色联动）
行 2460-2567  ===== 参考图 UI + 模式联动 =====  renderImageRefBox / renderVideoRefBox / updateImageMode / updatePromptTplVisibility
行 2568-2837  ===== 生成主入口 =====     switchMode / btnGenerate / doGenerateImage / doGenerateVideo
行 2838-2919  ===== 视频轮询 =====       startPolling / extractVideoUrl / tick
行 2920-2980  ===== 任务面板 =====       createTask / renderTaskCard / updateTask
行 2980-3052  ===== 设置面板 + 恢复 =====
行 3053-3343  ===== 持久化 =====         serializeCanvas / applyCanvas / saveLocal / saveFile / openLocal / openFile / autoRestore
行 3344-3361  ===== 节点排版 =====       nextNodeX / nextNodeY
行 3361-3369  ===== 工具函数 =====       shortTime / escapeHtml
行 3370-3457  ===== 运镜库常量 =====     CAMERA_MOTION_LABELS / GROUPS / NUMBER / CN_NAME / PREVIEW BASE
行 3457-3549  ===== 运镜 prompt 注入 ===== applyCameraMotionToPrompt / extractAndApplyCameraMotion(v1.24) / updateCameraMotionChip / populateCameraMotionSelect / change / remove
行 3549-3792  ===== 运镜库预览 modal =====
行 3660-3810  ===== 分辨率表 + 视频参数联动 =====  RESOLUTION_TABLE / TIER_MAX_FRAMES / getRes / updateResolutionHint / updateDurationOptions / updateDurationHint / VIDEO_MODE_PLACEHOLDERS / updateVideoMode
行 3812-3904  ===== 反推 popover =====   promptPopovers / ensurePromptPopover / positionPromptPopover / updateAllPopoverPositions / removePromptPopover
行 3906-3979  ===== 文件上传 =====       createRefImageNode / uploadLocal / urlInput
行 3979-4010  ===== 图片 URL 探测 =====  probeImage
行 4011-4048  ===== Ctrl+V 粘贴图片 =====
行 4050-4060  ===== 全局错误捕获 + 启动日志 =====
```

---

## 2. 总体布局（CSS Grid）

```
┌─────────────────────────────────────────────────────────────────┐
│ topbar (48px)   💾保存 📂打开 ⊡适配 ✕清空 ⚙设置            │
├──────────────┬──────────────────────────┬─────────────────────┤
│              │                          │                     │
│ left-panel   │   canvas-area（主）       │  right-panel        │
│ (300px)      │                          │  (340px)            │
│              │   ┌──────────────────┐   │                     │
│ 模式切换器   │   │ viewport         │   │  📋 任务队列 N       │
│ ▢图像▢视频▢文本 │   │  + world（节点） │   │                     │
│              │   │  + empty hint    │   │  [task-card]        │
│ 生成参数：   │   │  + hud (缩放控制)│   │  [task-card]        │
│  prompt      │   │                  │   │                     │
│  图像 / 视频 │   │                  │   │                     │
│  / 文本参数  │   └──────────────────┘   │                     │
│              │                          │                     │
│  [✦ 生成]    │                          │                     │
│              │                          │                     │
│ 提示词建议   │                          │                     │
│  (7 范例联动)│                          │                     │
│              │                          │                     │
│ 💬 对话记录   │                          │                     │
│ (文本模式用) │                          │                     │
└──────────────┴──────────────────────────┴─────────────────────┘

overlay: preview / save modal / open modal / camera motion modal / settings modal / toast
```

CSS 类名锚点：`.app` / `.topbar` / `.left-panel` / `.canvas-area` / `.canvas-viewport` / `.canvas-world` / `.right-panel`

### 2.1 颜色变量（`:root`，行 8-29）

| 变量 | 用途 |
|---|---|
| `--bg-0/1/2/3/4` | 5 层背景灰（最暗→次亮） |
| `--line / --line-2` | 边框 / hover 边框 |
| `--text-0/1/2/3` | 4 层文字（主→弱） |
| `--brand / --brand-2 / --brand-soft` | 主色 / 深主色 / 半透明 |
| `--ok / --warn / --err` | 状态色 |
| `--shadow / --radius / --radius-sm` | 阴影 / 圆角 |

**改色统一改 `:root`**，具体选择器内禁止写魔法颜色。

---

## 3. 状态（`state`，行 1501-1518）

```js
state = {
  mode: 'image',            // 当前左栏显示哪块参数面板
  cmSelectedId: '',         // 当前选中的运镜 id（v1.13+），用于 prompt 末尾拼接管理
  nodes: new Map(),         // id -> node
  tasks: new Map(),         // taskId -> task
  view: { x: 0, y: 0, scale: 1 },  // 画布视口（世界坐标）
  videoRefIds: [],          // 图生视频参考图节点 id 列表
  imageRefIds: [],          // 图像参考图节点 id 列表（多图融合按数组顺序传）
  pan: null,                // 画布平移中间态
  nodeDrag: null,           // 节点拖拽中间态
  textHistory: [],          // 文本多轮对话：[{role, content}]
  textImageUrl: '',         // 文本附加图片（dataURL 或 http URL）
};
```

**关键不变量**：
- `state.nodes` 是唯一的真实数据源，DOM 通过 `renderNode` 派生
- `view.scale` 0.1 ~ 8，缩放以鼠标为中心
- `imageRefIds` 按数组顺序传给后端，**首图占主导**
- `state.cmSelectedId` 是 source of truth，select 和 chip 仅做视图同步（v1.13 后所有运镜切入选中走这条字段）

---

## 4. 节点系统（行 1689-2417）

### 4.1 数据模型

```js
node = {
  id: 'n_xxxxxxxx',         // uid() 生成
  type: 'image' | 'video',  // 节点类型
  x, y, w, h,               // 世界坐标 + 尺寸
  prompt: string,           // 生成时的提示词
  src: string | null,       // 媒体 URL / dataURL，node 渲染依据；未生成时为 null
  status: 'pending' | 'running' | 'queued' | 'in_progress' | 'completed' | 'failed' | 'no_url' | 'reversing' | 'uploaded' | 'external',
  progress: number,         // 0-100（视频专用）
  meta: { ... },            // 自由扩展
  selected: false,          // UI 状态，不持久化
  createdAt: number,
}
```

### 4.2 meta 字段约定

| 字段 | 来源节点 | 出现时机 | 用途 |
|---|---|---|---|
| `sourceParams` | image / video | 生成完成时 | 一键重试用（`retryFromNode` 用它恢复所有参数） |
| `seed` | video | 提交 seed 时或后端回传 | 显示在节点 + 可复制 |
| `elapsedMs` | image | 客户端测量耗时 | 节点底部 |
| `size` | image | 响应里 size | 显示 |
| `duration` / `inferSeconds` | video | 轮询 complete | 显示 |
| `error` | 任意 | 失败时 | 红字展示 |
| `enPrompt` / `zhPrompt` / `reversedAt` | image | 反推完成 | popover 展示 + 一键填入 prompt |
| `seedRequested` | video | 提交时 seed 输入 | 区分"用户请求"和"后端回传" |

### 4.3 类型枚举与来源

| status 值 | 节点类型 | 含义 | 来源 |
|---|---|---|---|
| `pending` / `running` | image | 图像生成中（含 queued 阶段） | `doGenerateImage` |
| `queued` / `in_progress` / `completed` | video | 视频异步任务状态（原样回显） | Agnes API status |
| `reversing` | image | 反推中（不阻塞其他生成） | `reversePromptFromNode` |
| `failed` | 任意 | 生成失败 | try/catch |
| `no_url` | video | 完成但响应里没找到 URL 字段 | `startPolling` 完成分支 |
| `uploaded` | image | Ctrl+V / 本地上传 | `createRefImageNode` |
| `external` | image | URL 添加参考 | `createRefImageNode` |

### 4.4 生命周期

```
createNode(type, …) ─┬─ 新生成：doGenerateImage / doGenerateVideo
                     ├─ 重试失败：reuseNodeId + reset src/status/progress
                     └─ 重生参考图：createRefImageNode（uploaded / external）

renderNode(node)  每次状态变化都调一次，幂等更新 DOM

removeNode(id)    从 state.nodes 删 + 移除 DOM + 清理 imageRefIds/videoRefIds/popover
                  引用计数自动收敛
```

### 4.5 节点交互

**拖拽**：`mousedown` 在节点本体 → `state.nodeDrag` → `mousemove` 更新 `x/y` + 同步 popover → `mouseup` 结束
**双击**：全屏预览（`previewOverlay`）
**底部按钮**（图标）：
- `▭` 加入图像参考（仅 image）
- `▶` 加入视频参考（仅 image）
- `⟲` 反推（仅 image，仅 `src` 存在）
- `↻` 重试（仅 failed）
- `⤢` 预览
- `⬇` 下载
- `✕` 删除

**meta 行按钮**：
- `🇨🇳 / 🇺🇸` 切中英反推版 → 填到 prompt 输入框
- `📋 seed`（video only）→ 复制 seed 到 `#videoSeed`
- `↻ 重生成` → 切模式 + 填 prompt + 填 seed（视频） + 按钮闪一下

### 4.6 反推 Popover（行 3816-3904）

- 每个有 `enPrompt/zhPrompt` 的 image 节点**右侧**显示浮动卡片
- 内容：🇨🇳 中文 + → 填入 prompt / 🇺🇸 英文 + → 填入 prompt
- 随节点拖拽、视图缩放、平移自动定位（`positionPromptPopover` 用 `getBoundingClientRect` 重算）
- 用 `popoverLayer` 容器（z-index 50）避免被节点遮住
- **TDZ 修复（v1.18）**：`promptPopovers` Map 必须定义在 `autoRestore` `setTimeout` 之后才能访问

---

## 5. 三模式面板（行 909-1089）

### 5.1 图像参数（`#imageParams`）

| 表单 | id | 说明 |
|---|---|---|
| 模式 | `#imageMode` | t2i / i2i / multi（默认 t2i，v1.19） |
| 尺寸 | `#imageSize` | 5 种预设（含 1:1 / 16:9 / 3:2 等） |
| 参考图盒子 | `#imageRefBox` | 缩略图列表，× 移除 |
| 上传 | `#btnUploadLocal` + `#imageFileInput` | 多选 ≤10MB / 张 |
| URL | `#urlInput` + `#btnAddUrl` | 需 `http://` 或 `https://`，5 秒 `Image()` 探活 |

**`#imageMode`**：
- `t2i`：忽略参考图，body 不带 image
- `i2i`：refs[0]，要 1 张
- `multi`：refs 全部（≥2 张）

### 5.2 视频参数（`#videoParams`）

| 表单 | id | 说明 |
|---|---|---|
| 时长预设 | `#videoDuration` | 3/5/10/17 秒 / custom（自动按档位调整 option 文本） |
| 分辨率档 | `#videoTier` | 480p / 720p / 1080p |
| 宽高比 | `#videoRatio` | 16:9 / 9:16 / 1:1 / 4:3 / 3:4 |
| 帧数 | `#videoFrames` | 8n+1（9~961） |
| 帧率 | `#videoFps` | 12 / 16 / 24 / 30 |
| 反向提示词 | `#negativePrompt` + `#btnFillNegativePrompt` | v1.22 一键填入 `NEGATIVE_PROMPT_PRESETS.general` |
| 生成模式 | `#videoMode` | t2v / i2v / keyframes（默认 t2v，v1.19） |
| 运镜 select | `#videoCameraMotion` | 33 项按 5 组 optgroup |
| 运镜预览库按钮 | `#btnCameraMotionPicker` | modal 视觉预览图 + 视频 |
| 已选运镜 chip | `#cameraMotionChip` + `#btnRemoveCameraMotion` | 显示 + × 移除 |
| 参考图盒子 | `#videoRefBox` | 同 image，多模式共用 |
| 种子 | `#videoSeed` + 🎲/× | 留空 = 后端随机 |

**关键约束（client-side 校验）**：
- `num_frames` 必须 8n+1
- 帧数超出档位上限自动 clip
- `i2v` 要 ≥1 张参考；`keyframes` 要 ≥2 张

### 5.3 文本参数（`#textParams`，v1.19+）

| 表单 | id | 说明 |
|---|---|---|
| 系统角色 | `#textRole` | 6 选：assistant / storyboard / script_polish / image_reverse / translator / custom |
| 自定义系统角色 | `#textCustomRole` | 仅 `custom` 时显示 |
| 温度 | `#textTemp` | 0-2 step 0.1（默认 0.7） |
| max tokens | `#textMaxTokens` | 64-8192（默认 2048） |
| 多轮 | `#textMultiTurn` | 勾选时 `state.textHistory` 全部传入 |
| 历史计数 | `#textHistoryCount` + `#btnClearTextHistory` | 列表清空 |
| 附加图片 | `#btnTextUploadImage` + `#btnTextClearImage` | 单张，`state.textImageUrl` |

**生成按钮文案**随模式切换：`生成图像` / `生成视频` / `生成文本`

---

## 6. API 集成（行 1320-1496）

### 6.1 统一入口

| 函数 | 用途 | 端点 |
|---|---|---|
| `callApi(path, opts)` | 普通 fetch + 鉴权 + JSON 解析 + 错误对象化 | `${CFG.baseUrl}${path}` |
| `callApiWithRetry(path, opts, retryOpts)` | 5xx / 网络错自动重试 N 次 | 同上 |
| `fetch`（裸用） | 流式（SSE）：`doGenerateText` | 同上 |

**鉴权**：每个请求自动加 `'Authorization': 'Bearer ' + CFG.apiKey`
**超时**：未设全局超时；视频轮询用 setTimeout 自管
**错误统一形态**：`Error.message + err.status + err.data`

### 6.2 图像生成（同步）

`generateImage({ prompt, size, image })` → `POST /images/generations`

```js
body = {
  model: 'agnes-image-2.1-flash',
  prompt, size,
  extra_body: { response_format: 'url' },  // ⚠️ 必须在 extra_body 内
}
// i2i/multi 才追加：
if (image) body.extra_body.image = Array.isArray(image) ? image : [image];
```

**绝对禁忌**：
- 不要把 `response_format` 放请求体顶层（实测 422）
- 不要传 `tags: ["img2img"]`（完全没效果也不报错，但浪费 token）

### 6.3 视频生成（异步两步）

**Step 1**：`POST /v1/videos` → 返回 `{video_id, task_id}`
- 视频 v1.15 起使用 `extractRealVideoId()` 解码网关伪 ID
- 真实 ID 格式：`video_<32位hex>`（直接放在网关 ID 里需要 base64 解码提取）

**Step 2**：轮询 `GET /agnesapi?video_id=<REAL_ID>`
- 用 `new URL('/agnesapi', CFG.baseUrl)` 单独拼接 host（不在 `/v1` 路径下）
- 频率：默认 10000ms（可设置）
- 状态：`queued` → `in_progress` → `completed` / `failed`
- completed 时从 `data.remixed_from_video_id` 拿视频 URL（兼容 `url` / `video_url` / `output_url` / `result_url` 兜底）

**`extractVideoUrl(data)`**：4 个字段按优先级取 URL，找不到 → `null`

### 6.4 文本生成（流式）

`doGenerateText(prompt, opts)` → `POST /v1/chat/completions` + `stream: true`

- 走**裸 fetch**（不经过 `callApi`，因为要流）
- 用户消息支持多模态：`content: [{type:'text', text:...}, {type:'image_url', image_url:{url}}]`
- 多轮：`messages = [...systemPrompt, ...state.textHistory, currentUser]`
- 流式解析：SSE chunk → JSON → `choices[0].delta.content` 累加到 DOM

**`opts` 参数**：
- `opts.role`：覆盖 `#textRole`（联动 2：`script_polish` → `storyboard`）
- `opts.silent`：true 时跳过"文本生成完成" toast

### 6.5 工具 API（同步）

- `reverseImageToPrompt(src)`：`POST /chat/completions`，system prompt 是英文 prompt engineer 模板，温度 0.7，max 300 tokens
- `translateToChinese(en)`：`POST /chat/completions`，system prompt 要求只输出中文，温度 0.3，max 500 tokens

---

## 7. 角色联动（v1.24，文本生成专属）

```
script_polish 完成
  ↓ 800ms 后自动
doGenerateText(role='storyboard', silent=true, prompt=`请把下面这段优化后的脚本拆分成多分镜脚本：\n\n${脚本全文}`)
  ↓ 流式完成后
extractAndApplyCameraMotion(fullContent)
  ↓ 遍历 CM_CN_NAME keys，第一个出现在文本里的英文 id
applyCameraMotionToPrompt(id)  → #prompt 末尾追加 ", 相机XX"，state.cmSelectedId 同步
  ↓ toast
"📽️ 已自动识别运镜：相机XX（<id>）"
```

**禁止循环**：
- `script_polish → storyboard` 是单向链
- `storyboard` 自己**不**触发 `storyboard`
- 预留 `_noChain` 字段未来扩展（当前未用）

**关键前提**：`storyboard` 的 system prompt 强制要求 `镜头: <英文id>` 格式（如 `dolly_in`），否则 `extractAndApplyCameraMotion` 找不到匹配（静默失败，不弹错误）

---

## 8. 运镜库（行 3370-3505）

### 8.1 4 个常量表（同步维护）

| 常量 | 用途 |
|---|---|
| `CAMERA_MOTION_LABELS` | 中文长标签 + 语义说明，用于 select option 文字 + 节点 meta |
| `CAMERA_MOTION_GROUPS` | 5 组 optgroup：基础控制 / 人物跟拍 / 揭示转场 / 情绪强化 / 空间航拍 |
| `CM_NUMBER` | 33 个数字编号（对应剪映官方 CDN 命名） |
| `CM_CN_NAME` | 短中文名（用于 `, 相机XX` 拼接 + popover 提示） |

### 8.2 预览素材 CDN

`https://lf-xiaoyunque.jianying.com/obj/pippit-app-buz/pippit_cms/<NN>_<encodeURIComponent(中文名)>.{webp|mp4}`

`cmPreview(id)` 拼接 URL；预览 modal（行 3551-3792）按当前 tab 渲染网格，支持 webp 缩略图 + mp4 hover 播放

### 8.3 注入 prompt 的方式（v1.13）

不再用 marker，直接拼 `, 相机XX` 到 prompt 末尾：

```js
applyCameraMotionToPrompt(id) {
  // 1. 剥离之前选中的运镜（用 state.cmSelectedId 匹配旧的 + CM_CN_NAME[oldId]）
  // 2. 追加 ", 相机XX"（或空 prompt 时不带前导逗号）
  // 3. 同步 state.cmSelectedId = id
  // 4. 同步 select value + chip 显示
}
```

**为什么不用 marker**：marker 包裹会让大模型的 prompt 模板更复杂、还要做正则替换；v1.13 决定纯拼接，简单可靠。

---

## 9. 持久化（行 3053-3343）

### 4 个 localStorage key

| key | 内容 | 何时写 |
|---|---|---|
| `agnes.baseUrl` / `agnes.apiKey` / `agnes.pollMs` | 配置（baseUrl / apiKey / 轮询 ms） | 设置面板保存 |
| `agnes.canvas.v1` | 画布完整快照（见下） | 用户点"保存到浏览器" |
| `LOCAL_KEY = 'agnes.canvas.v1'` | 常量名（同上） | — |

### 9.1 画布 JSON Schema（`CANVAS_SCHEMA_VERSION = 1`）

```jsonc
{
  "version": 1,
  "app": "agnes-studio",
  "savedAt": 0,
  "view": { "x": 0, "y": 0, "scale": 1 },
  "nodes": [
    {
      "id": "n_xxxxxxxx",
      "type": "image" | "video",
      "x": number, "y": number, "w": number, "h": number,
      "prompt": string,
      "src": string | null,
      "status": string,
      "progress": number,
      "createdAt": number,
      "meta": { /* 自由扩展 */ }
    }
  ],
  "imageRefIds": ["n_xxx", ...],
  "videoRefIds": ["n_xxx", ...],
  "ui": {
    "mode": "image" | "video" | "text",
    "imageMode": "t2i" | "i2i" | "multi",
    "videoMode": "t2v" | "i2v" | "keyframes",
    "videoDuration": "3" | "5" | "10" | "17" | "custom",
    "imageSize": "1024x768" | ...,
    "videoTier": "480p" | "720p" | "1080p",
    "videoRatio": "16:9" | ...,
    "videoFrames": "121",
    "videoFps": "24" | "30" | "16" | "12",
    "videoSeed": "" | "12345"
  },
  "tasks": [
    { "id": "t_xxx", "type", "prompt", "status", "progress", "videoId", "nodeId", "createdAt" }
  ]
}
```

### 9.2 自动恢复

启动时 `setTimeout(0)` 后（避开 TDZ）：
- 读 localStorage → JSON.parse
- 有 nodes 直接 `applyCanvas(data)`
- 不弹 confirm 弹窗（v1.17 修复）
- 500ms 后 toast 提示"已自动恢复 N 节点 · T 分钟前"
- 失败 catch 也弹 toast（之前只 console.warn）

### 9.3 applyCanvas 防御

- 清空当前 nodes/tasks/refs（不重启轮询）
- 校验 src 格式：仅 `http(s):` / `data:` 合法；否则跳过并 warn
- 校验 image/videoRefIds 是否仍存在 id
- 加载后 100ms 调 `fitView` + 显示 toast

---

## 10. 提示词建议 + 模板（行 1098-1144）

7 张 `.tpl-block` div，每张 `data-modes` 属性控制可见性：

| 范例 | data-modes | 公式 |
|---|---|---|
| 🖼️ 文生图 | `t2i` | `[主体] + [场景/环境] + [风格] + [光照] + [构图] + [质量要求]` |
| 🔄 图生图 | `i2i` | `[改变要求] + [新风格/场景] + [新增移除] + [保留元素]` |
| 🏙️ 高信息密度图 | `t2i,i2i,multi` | `[主体] + [次要细节] + [背景] + [风格] + [光照] + [构图约束]` |
| 🎬 文生视频 | `t2v` | `[主体] + [动作] + [场景] + [镜头运动] + [光线] + [风格]` |
| 🖼️→🎬 图生视频 | `i2v` | `[动起来的内容] + [保持稳定的主体]` |
| 🎞️ 多图视频 | `keyframes` | `[起始场景 → 目标场景] + [过渡方式] + [一致性约束]` |
| 🎞️ 关键帧动画 | `keyframes` | `[关键帧1 → 关键帧2] + [一致] + [自然过渡]` |

**`updatePromptTplVisibility()`**（行 2526-2533）：按 `state.mode === 'image' ? imageMode.value : videoMode.value` 过滤显示

**反向提示词模板** (`NEGATIVE_PROMPT_PRESETS`)：

| key | 内容 |
|---|---|
| `general` | `blur, distortion, low quality, ugly, deformed, watermark, text, signature, jpeg artifacts` |
| `face` | `blur, distorted face, asymmetric eyes, extra fingers, deformed hands, low quality` |
| `cinematic` | `amateur, shaky camera, overexposed, underexposed, blurry, low quality, jpeg artifacts` |

---

## 11. 快捷键 + 交互

| 操作 | 触发 | 行为 |
|---|---|---|
| 滚轮 | viewport | 以鼠标为中心缩放（0.1-8 倍） |
| 空格 + 拖 / 中键 | viewport | 平移画布 |
| 双击节点 | node | 全屏预览（preview overlay） |
| Delete / Backspace（焦点不在输入框） | — | 删除选中节点 |
| Ctrl+V | body | 粘贴图片自动作为图像参考（≥1张 input 不拦截） |
| Enter | `#urlInput` | 添加 URL 参考图 |
| 关闭 modal | click outside | 自动 close（4 个 modal 都监听） |

---

## 12. 修改入口索引（速查表）

| 想改… | 先找… |
|---|---|
| 加新模型（比如 agnes-audio） | 1286 DEFAULTS / 1320 API 集成段 / 1968 TEXT_ROLES / 2569 switchMode |
| 改运镜库 | 3370-3457 常量段；预览素材 URL 格式不变（CDN 路径稳定） |
| 改色 / 暗色变浅 | `:root` 行 8-29 |
| 改节点位置/尺寸默认 | 2649（image 320x320）、2794（video 360x360） |
| 改反推的 system prompt | 1397（API 函数版）、1970（storyboard 联动版） |
| 改画布 localStorage key | 3058 `LOCAL_KEY` |
| 改 JSON 兼容策略 | 3105 `applyCanvas` 校验、version 字段 |
| 加新 modal | 复制行 1203 模板 + 行 3045 click outside 模式 + 行 2989 open 切换 |
| 加新节点 status | 1746 placeholderText 映射 + 1857 handler act + 1758 状态徽章（按需） |
| 改轮询超时 | 1305 改 `pollMs` 设置项（3000-60000 ms 范围） |
| 改 8 网格下网格列数 | 3350 nextNodeX 的 380 间距 |
| 加新系统角色 | 1053 HTML option + 1968 TEXT_ROLES + 可选联动 hook（行 2112-2123） |
| 改自动恢复策略 | 3328-3344 setTimeout 块 |
| 改任务面板字段 | 2922-2980 createTask/renderTaskCard |

---

## 13. 已知陷阱（踩过的坑）

| 坑 | 修复版本 | 说明 |
|---|---|---|
| `response_format` 放请求体顶层 → 422 | v1.0 | 必须在 `extra_body.response_format` |
| `tags: ["img2img"]` 完全无效 | v1.0 | 只传 `extra_body.image` |
| `num_frames` 不满足 8n+1 | v1.0 | 后端拒绝 → 客户端 `doGenerateVideo` 预校验 |
| `video_id` 是 LiteLLM 伪 ID（base64） | v1.15 | `extractRealVideoId` 解码；存 `task.videoId` 用真 ID |
| `/v1/videos/{id}` 网关找不到任务 | v1.15 | 改用 `/agnesapi?video_id=`（不在 `/v1` 路径下） |
| 图像 base64 后端报 "cannot be 1 more than a multiple of 4" | v1.0 | `cleanBase64DataUrl` 补 padding |
| Ctrl+V 在输入框里误触发生成 | v1.0 | paste listener 主动 ignore INPUT/TEXTAREA |
| localStorage >5MB 写入失败 | v1.0 | `QuotaExceededError` 捕获 + 引导下载 JSON |
| 节点拖拽时反推 popover 不跟 | v1.0 | mousemove 同步 `positionPromptPopover` |
| 节点拖到边缘时 popover 撞 canvas 边框 | — | 未修，未来 `updateAllPopoverPositions` 可以加 clamp |
| `tryAutoRestore` 立即执行撞 TDZ（`promptPopovers` 未定义） | v1.18 | `setTimeout(fn, 0)` 延迟到下一 tick |
| 自动恢复 `confirm()` 在某些浏览器被静默 dismiss | v1.17 | 去掉 confirm，直接 `applyCanvas` + 500ms toast |
| `extractCameraMotion` ↺ 按钮误导（被反推 prompt 提取误命中） | v1.13 | 反推走独立 popover，运镜提取只对 video 节点或用户 prompt 末尾的 `相机XX` 操作 |
| 文本反推/翻译 Card 点 ↻ 旧参数全无 | v1.16 | `sourceParams` 在生成完成时存 meta，重试复用 `reuseNodeId` |
| `applyCanvas` 加载后旧轮询任务继续打 API | v1.0 | 清掉 `pollTimers` 所有 setTimeout |
| 节点 src 是 `blob:` URL 时反推 / 加载失败 | v1.0 | applyCanvas 校验仅接受 `http(s):` / `data:` |

---

## 14. 一句话功能清单（= 可勾选项）

- [x] 无限画布：平移、缩放（0.1-8×）、双击预览、删除键、Ctrl+V 粘图
- [x] 三模式切换：图像 / 视频 / 文本
- [x] 图像：t2i / i2i(1 ref) / multi(≥2 ref)，5 种尺寸
- [x] 视频：t2v / i2v(1 ref) / keyframes(≥2 ref)，480p/720p/1080p × 5 比例
- [x] 视频帧数 8n+1 校验，超档位上限自动 clip
- [x] 视频轮询（10s）按需调间隔（3s-60s），UI 进度条
- [x] 33 种运镜（含 webp/mp4 预览），拼到 prompt 末尾
- [x] 反推：看图 → 英文 prompt → 译中文 → 节点右侧 popover（双语可一键填入）
- [x] 失败节点一键重试（图像/视频都复用原节点 + sourceParams 恢复）
- [x] 提示词建议 7 张按当前模式联动
- [x] 反向提示词模板（general / face / cinematic）
- [x] 系统角色 6 个：assistant / storyboard / script_polish / image_reverse / translator / custom
- [x] 文本生成：流式 SSE + 多轮 + 图像理解 + 温度 / max_tokens
- [x] 角色联动：storyboard 自动选运镜，script_polish 自动调 storyboard 拆解
- [x] prompt 客户端安全预检（6 类高风险关键词粗筛）
- [x] 设置面板：baseUrl / apiKey / 轮询间隔 / 恢复 video_id 任务
- [x] 任务面板：实时状态 / 进度条 / 错误回显 / 计数
- [x] 画布保存：到浏览器（localStorage）+ 导出 JSON 文件
- [x] 画布打开：从浏览器恢复 + 从 JSON 文件加载
- [x] 启动自动恢复本地存档（无 confirm 阻塞）
- [x] 下载节点媒体（图片/视频）
- [x] Toast 提示（ok/warn/err/info 四级，自动消失）

---

## 15. 还想加的功能（Backlog，备忘）

- 多画布/项目管理（左侧栏树状结构）
- 节点对齐辅助线 + 吸附
- 关键帧动画的可视化编辑器
- 历史版本回滚（一键从 `agnes-studio-vN.html` 恢复）
- 提示词模板库（自建模板）
- 节点连线 / DAG 工作流
- 反推结果批量导出
- Agnes 新模型接入（按"先文档、再代码"节奏）

---

> **本文档维护**：每次 vN+1 改动落地后，**同步更新本文件的"修改入口索引"和"已知陷阱"两节**即可，结构章节无需重写。
