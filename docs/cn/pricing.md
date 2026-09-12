> ## Documentation Index
> Fetch the complete documentation index at: https://wiki.agnes-ai.cn/llms.txt
> Use this file to discover all available pages before exploring further.

# 模型定价

> Agnes AI 中国站文本、图片和视频模型的人民币刊例价、现价及优惠计费规则。

本页汇总 Agnes AI 中国站当前已公开模型的人民币价格。所有价格均为 API 调用价格；`M` 表示一百万 Token。

<Info>
  “刊例价（原价）”是优惠结束后可采用的标准价格；“现价（优惠价）”是当前实际计费价格。阶段性优惠的结束时间以 Agnes AI 平台公告和账户账单为准。
</Info>

## 文本模型

<table>
  <thead>
    <tr>
      <th>模型</th>
      <th>计费项</th>
      <th>刊例价（原价）</th>
      <th>现价（优惠价）</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td rowSpan={3} style={{ verticalAlign: "middle" }}><code>agnes-2.5-flash</code></td>
      <td>输入缓存命中</td>
      <td><del><code>¥0.035 / M</code></del></td>
      <td><strong><code>¥0 / M</code></strong></td>
    </tr>

    <tr>
      <td>输入 Token</td>
      <td><del><code>¥0.35 / M</code></del></td>
      <td><strong><code>¥0 / M</code></strong></td>
    </tr>

    <tr>
      <td>输出 Token</td>
      <td><del><code>¥1.00 / M</code></del></td>
      <td><strong><code>¥0 / M</code></strong></td>
    </tr>

    <tr>
      <td rowSpan={3} style={{ verticalAlign: "middle" }}><code>agnes-2.5-pro</code></td>
      <td>输入缓存命中</td>
      <td><code>¥0.30 / M</code></td>
      <td><code>¥0.30 / M</code></td>
    </tr>

    <tr>
      <td>输入 Token</td>
      <td><code>¥3.00 / M</code></td>
      <td><code>¥3.00 / M</code></td>
    </tr>

    <tr>
      <td>输出 Token</td>
      <td><code>¥6.00 / M</code></td>
      <td><code>¥6.00 / M</code></td>
    </tr>

    <tr>
      <td rowSpan={3} style={{ verticalAlign: "middle" }}><code>agnes-2.5-pro-beta</code></td>
      <td>输入缓存命中</td><td><code>¥0.07 / M</code></td><td><code>¥0.07 / M</code></td>
    </tr>

    <tr><td>输入 Token</td><td><code>¥0.70 / M</code></td><td><code>¥0.70 / M</code></td></tr>
    <tr><td>输出 Token</td><td><code>¥2.10 / M</code></td><td><code>¥2.10 / M</code></td></tr>

    <tr>
      <td rowSpan={3} style={{ verticalAlign: "middle" }}><code>agnes-3.0-flash</code></td>
      <td>输入缓存命中</td><td><del><code>¥0.035 / M</code></del></td><td><strong><code>¥0 / M</code></strong></td>
    </tr>

    <tr><td>输入 Token</td><td><del><code>¥0.35 / M</code></del></td><td><strong><code>¥0 / M</code></strong></td></tr>
    <tr><td>输出 Token</td><td><del><code>¥1.00 / M</code></del></td><td><strong><code>¥0 / M</code></strong></td></tr>
  </tbody>
</table>

### 当前优惠

* `agnes-2.5-flash` 和 `agnes-3.0-flash` 的输入缓存命中、输入 Token 和输出 Token 当前均免费。
* `agnes-2.5-pro` 当前按 Pro 系列刊例价计费，暂无价格折扣；`agnes-2.5-pro-beta` 采用上表所列的独立 Beta 价格。

<Note>
  输入缓存命中单价为普通输入 Token 单价的 10%。缓存命中价格只适用于服务端确认命中的输入 Token；未命中的输入按普通输入 Token 单价计算。
</Note>

## 图片模型

<table>
  <thead>
    <tr><th>模型</th><th>计费项</th><th>刊例价（原价）</th><th>现价（优惠价）</th></tr>
  </thead>

  <tbody>
    <tr><td rowSpan={5} style={{ verticalAlign: "middle" }}><code>agnes-image-2.0-flash</code></td><td>1K 输出图片</td><td><del><code>¥70 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>2K 输出图片</td><td><del><code>¥120 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>3K 输出图片</td><td><del><code>¥140 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>4K 输出图片</td><td><del><code>¥160 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>第 4 张起的输入参考图片</td><td><del><code>¥0.02 / 张</code></del></td><td><strong><code>¥0 / 张</code></strong></td></tr>
    <tr><td rowSpan={5} style={{ verticalAlign: "middle" }}><code>agnes-image-2.1-flash</code></td><td>1K 输出图片</td><td><del><code>¥70 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>2K 输出图片</td><td><del><code>¥120 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>3K 输出图片</td><td><del><code>¥140 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>4K 输出图片</td><td><del><code>¥160 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>第 4 张起的输入参考图片</td><td><del><code>¥0.02 / 张</code></del></td><td><strong><code>¥0 / 张</code></strong></td></tr>
    <tr><td rowSpan={5} style={{ verticalAlign: "middle" }}><code>agnes-image-2.5-flash</code></td><td>1K 输出图片</td><td><del><code>¥70 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>2K 输出图片</td><td><del><code>¥120 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>3K 输出图片</td><td><del><code>¥140 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>4K 输出图片</td><td><del><code>¥160 / 千张</code></del></td><td><strong><code>¥0</code></strong></td></tr>
    <tr><td>第 4 张起的输入参考图片</td><td><del><code>¥0.02 / 张</code></del></td><td><strong><code>¥0 / 张</code></strong></td></tr>
  </tbody>
</table>

### 当前优惠

`agnes-image-2.0-flash`、`agnes-image-2.1-flash` 和 `agnes-image-2.5-flash` 的价格及计费方法相同。当前所有输出分辨率档位和输入参考图片均免费。

### 图片刊例价计费规则

按刊例价计算时，输出图片根据分辨率档位计费。图生图或多图参考任务的前 3 张输入参考图片不额外收费，从第 4 张开始按超出张数计费。

```text theme={null}
刊例价费用 = 输出图片张数 × 对应输出分辨率单价
           + max(0, 输入参考图片张数 - 3) × ¥0.02
```

| 计费项           |       单张刊例价 | 每千张输出图片等价价格 |
| ------------- | ----------: | ----------: |
| 1K 输出图片       | `¥0.07 / 张` |       `¥70` |
| 2K 输出图片       | `¥0.12 / 张` |      `¥120` |
| 3K 输出图片       | `¥0.14 / 张` |      `¥140` |
| 4K 输出图片       | `¥0.16 / 张` |      `¥160` |
| 第 4 张起的输入参考图片 | `¥0.02 / 张` |         不适用 |

### 图片当前计费规则

当前价格为 `¥0`：所有支持分辨率档位的输出图片均免费，输入参考图片（包括第 4 张及之后）也不收费。

## 视频模型

<table>
  <thead>
    <tr>
      <th>模型</th>
      <th>计费项</th>
      <th>刊例价（原价）</th>
      <th>现价（优惠价）</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td><code>agnes-video-v2.0</code></td>
      <td>视频时长</td>
      <td><del><code>¥0.035 / 秒</code></del></td>
      <td><strong><code>¥0 / 秒</code></strong></td>
    </tr>

    <tr>
      <td rowSpan={5} style={{ verticalAlign: "middle" }}><code>agnes-video-2.5</code></td>
      <td>720P 输出视频</td>
      <td><code>¥0.15 / 秒</code></td>
      <td><code>¥0.15 / 秒</code></td>
    </tr>

    <tr>
      <td>1080P 输出视频</td>
      <td><code>¥0.25 / 秒</code></td>
      <td><code>¥0.25 / 秒</code></td>
    </tr>

    <tr>
      <td>1K 输出视频</td>
      <td><code>¥0.25 / 秒</code></td>
      <td><code>¥0.25 / 秒</code></td>
    </tr>

    <tr>
      <td>2K 输出视频</td>
      <td><code>¥0.35 / 秒</code></td>
      <td><code>¥0.35 / 秒</code></td>
    </tr>

    <tr>
      <td>第 6 张起的输入图片</td>
      <td><code>¥0.03 / 张</code></td>
      <td><code>¥0.03 / 张</code></td>
    </tr>

    <tr>
      <td><code>agnes-video-2.5-flash</code></td>
      <td>720P 视频时长</td>
      <td><del><code>¥0.15 / 秒</code></del></td>
      <td><strong><code>¥0 / 秒</code></strong></td>
    </tr>
  </tbody>
</table>

### 当前优惠

* `agnes-video-v2.0` 当前免费。
* `agnes-video-2.5` 按输出视频分辨率和总计费时长计费；总计费时长为输出视频时长与输入视频时长之和。输入图片前 5 张免费，第 6 张起按 `¥0.03 / 张` 计费。
* `agnes-video-2.5-flash` 采用与 Agnes Video 2.5 相同的计费公式，当前限时免费。

### Agnes Video 2.5 与 2.5 Flash 计费规则

Agnes Video 2.5 与 2.5 Flash 采用相同的计费公式：

```text theme={null}
视频总金额 = 输出秒数 × 输出分辨率单价
           + 输入视频秒数 × 输出分辨率单价
           + max(0, 图片数 - 免费图片张数) × 图片超额单价
```

| 计费项         |          单价 |
| ----------- | ----------: |
| 720P 输出视频   | `¥0.15 / 秒` |
| 1080P 输出视频  | `¥0.25 / 秒` |
| 1K 输出视频     | `¥0.25 / 秒` |
| 2K 输出视频     | `¥0.35 / 秒` |
| 第 6 张起的输入图片 | `¥0.03 / 张` |

输入图片前 5 张免费。输入视频时长虽然不设置独立单价，但会加入输出视频时长，合计后按输出视频分辨率对应单价计费。

`agnes-video-2.5` 的免费图片张数为 5 张，图片超额单价为 `¥0.03 / 张`。`agnes-video-2.5-flash` 按相同刊例价公式计算，但限时免费期间所有计费项现价均为 `¥0`。

更多细节请参阅 [Agnes Video 2.5 计费规则](/zh-Hans/docs/agnes-video-25#计费规则)。

## 计费说明

* 实际费用以 Agnes AI 平台账户账单为准。
* 当前优惠可能按模型、账户权限或活动阶段调整。
* 请求失败且未产生有效结果时，是否计费以平台最终账单记录为准。
* 本页价格如有更新，将同步反映到各模型文档。
