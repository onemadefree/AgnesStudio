> ## Documentation Index
> Fetch the complete documentation index at: https://wiki.agnes-ai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agnes Video 2.5 Flash

> Integrate Agnes Video 2.5 Flash through an OpenAI Videos-compatible API for text-to-video, keyframe control, and image-reference generation.

<Info>
  Agnes Video 2.5 Flash reuses the model capabilities and asynchronous task API of Agnes Video 2.5. Except for the Flash-specific limits documented on this page, request parameters, response fields, and retrieval behavior are the same as [Agnes Video 2.5](/en/docs/agnes-video-25).
</Info>

<CardGroup cols={2}>
  <Card title="Model ID" icon="cube">
    `agnes-video-2.5-flash`
  </Card>

  <Card title="Create Task" icon="video">
    `POST /v1/videos`
  </Card>

  <Card title="Retrieve Task" icon="clock">
    `GET /agnesapi?video_id=<VIDEO_ID>&model_name=agnes-video-2.5-flash`
  </Card>

  <Card title="Current Price" icon="tag">
    List price ~~`$0.025 / second`~~; now `$0 / second`
  </Card>
</CardGroup>

## Differences from Agnes Video 2.5

| Validation            | Flash rule                                  | Failure response                            |
| --------------------- | ------------------------------------------- | ------------------------------------------- |
| `size`                | Only the string `"720P"` is supported       | HTTP 400: `size must be 720P`               |
| Reference image count | `images` supports at most 5 items           | HTTP 400: `images length must not exceed 5` |
| Reference audio count | `audios` supports at most 3 items           | HTTP 400: `audios length must not exceed 3` |
| Reference video input | Non-empty `videos` content is not supported | HTTP 400: `videos is not supported`         |

<Warning>
  Flash-specific validation runs before task creation, queueing, billing, and inference. Invalid requests do not create a video task and are not billed.
</Warning>

All other capabilities and common validation rules are inherited from `agnes-video-2.5`.

## Quickstart

### 1. Set Environment Variables

```bash theme={null}
export AGNES_API_KEY="YOUR_API_KEY"
export AGNES_BASE_URL="https://apihub.agnes-ai.com/v1"
```

### 2. Create a Video Task

```bash theme={null}
curl -sS -X POST "$AGNES_BASE_URL/videos" \
  -H "Authorization: Bearer $AGNES_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "agnes-video-2.5-flash",
    "prompt": "A silver sports car moves slowly through a rain-soaked futuristic city street, neon reflections, cinematic camera motion, natural ambience",
    "seconds": "5",
    "mode": "text",
    "size": "720P",
    "aspect_ratio": "16:9"
  }'
```

Save `video_id` from the create response. `id` and `task_id` identify the asynchronous task, while `video_id` is used for retrieval.

### 3. Retrieve the Result

<Tabs>
  <Tab title="By video_id + model_name (Recommended)">
    ```bash theme={null}
    curl -sS "https://apihub.agnes-ai.com/agnesapi?video_id=VIDEO_ID&model_name=agnes-video-2.5-flash" \
      -H "Authorization: Bearer $AGNES_API_KEY"
    ```

    This form works for `text`, `keyframe`, and `reference` modes and is the recommended polling method for Agnes Video 2.5 Flash.
  </Tab>

  <Tab title="By video_id (text mode only)">
    ```bash theme={null}
    curl -sS "https://apihub.agnes-ai.com/agnesapi?video_id=VIDEO_ID" \
      -H "Authorization: Bearer $AGNES_API_KEY"
    ```

    This form is valid only for tasks created with `mode: "text"`. Queries for `keyframe` and `reference` tasks must include `model_name=agnes-video-2.5-flash`.
  </Tab>
</Tabs>

Poll every `1–2` seconds until `status` becomes `completed` or `failed`.

## Request Parameters

### Common Parameters

| Parameter      | Type    | Required | Description                                                                         |
| -------------- | ------- | -------- | ----------------------------------------------------------------------------------- |
| `model`        | string  | Yes      | Use `agnes-video-2.5-flash`.                                                        |
| `prompt`       | string  | Yes      | Video description. In Reference mode, use `<Picture N>` and `<Audio N>` for inputs. |
| `mode`         | string  | Yes      | `text`, `keyframe`, or `reference`.                                                 |
| `seconds`      | string  | No       | Duration as a string from `"4"`–`"12"`; default `"5"`.                              |
| `size`         | string  | No       | Flash is fixed to `"720P"`; other values return HTTP 400.                           |
| `aspect_ratio` | string  | No       | Default `16:9`. See “Video Size and Aspect Ratio.”                                  |
| `seed`         | integer | No       | Random seed.                                                                        |
| `n`            | integer | No       | Only `1` is supported; default `1`.                                                 |

### Mode-specific Parameters

| Parameter     | Type      | Mode        | Description                                                 |
| ------------- | --------- | ----------- | ----------------------------------------------------------- |
| `first_frame` | string    | `keyframe`  | First-frame image URL. Provide at least one frame URL.      |
| `last_frame`  | string    | `keyframe`  | Last-frame image URL. Provide at least one frame URL.       |
| `images`      | string\[] | `reference` | Reference image URLs. Flash supports at most 5 images.      |
| `audios`      | string\[] | `reference` | Reference audio URLs. Flash supports at most 3 audio clips. |
| `videos`      | object\[] | `reference` | Not supported by Flash; non-empty content returns HTTP 400. |

### Generation Mode Rules

| `mode`      | Use case                                      | Required media                                    | Disallowed media fields                                   |
| ----------- | --------------------------------------------- | ------------------------------------------------- | --------------------------------------------------------- |
| `text`      | Text-to-video                                 | None                                              | `first_frame`, `last_frame`, `images`, `audios`, `videos` |
| `keyframe`  | First-frame, last-frame, or two-frame control | At least one of `first_frame` or `last_frame`     | `images`, `audios`, `videos`                              |
| `reference` | Image- or audio-reference generation          | At least one non-empty `images` or `audios` array | `first_frame`, `last_frame`, `videos`                     |

In `reference` mode, `images` and `audios` may be used separately or together. Provide no more than 5 images and no more than 3 audio clips.

All media URLs must be publicly accessible to Agnes AI and remain valid until the task completes.

## Request Examples

<Tabs>
  <Tab title="Text to Video">
    ```bash theme={null}
    curl -sS -X POST "$AGNES_BASE_URL/videos" \
      -H "Authorization: Bearer $AGNES_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-2.5-flash",
        "prompt": "Three cats form a tiny brass band and march through a moonlit forest, smooth backward tracking shot",
        "seconds": "5",
        "mode": "text",
        "size": "720P",
        "aspect_ratio": "16:9"
      }'
    ```
  </Tab>

  <Tab title="Keyframe Control">
    ```bash theme={null}
    curl -sS -X POST "$AGNES_BASE_URL/videos" \
      -H "Authorization: Bearer $AGNES_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-2.5-flash",
        "prompt": "The character turns naturally and walks toward the window as the camera slowly pushes in, ending on the final-frame composition",
        "seconds": "5",
        "mode": "keyframe",
        "size": "720P",
        "first_frame": "https://example.com/first.png",
        "last_frame": "https://example.com/last.png"
      }'
    ```
  </Tab>

  <Tab title="Image Reference">
    ```bash theme={null}
    curl -sS -X POST "$AGNES_BASE_URL/videos" \
      -H "Authorization: Bearer $AGNES_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-2.5-flash",
        "prompt": "Use the character and art style in <Picture 1> as reference. The character runs naturally through a flower field while preserving appearance",
        "seconds": "5",
        "mode": "reference",
        "size": "720P",
        "aspect_ratio": "16:9",
        "images": ["https://example.com/character.png"]
      }'
    ```
  </Tab>

  <Tab title="Audio Reference">
    ```bash theme={null}
    curl -sS -X POST "$AGNES_BASE_URL/videos" \
      -H "Authorization: Bearer $AGNES_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-2.5-flash",
        "prompt": "Use <Audio 1> as the rhythm and ambience reference while generating a cinematic night drive",
        "seconds": "5",
        "mode": "reference",
        "size": "720P",
        "aspect_ratio": "16:9",
        "audios": ["https://example.com/reference-audio.mp3"]
      }'
    ```
  </Tab>
</Tabs>

## Video Size and Aspect Ratio

`size` must be `"720P"`. Select the exact output dimensions through `aspect_ratio`:

| `aspect_ratio` | Output pixels |
| -------------- | ------------- |
| `21:9`         | `1680x720`    |
| `16:9`         | `1280x704`    |
| `4:3`          | `960x720`     |
| `1:1`          | `720x720`     |
| `3:4`          | `720x960`     |
| `9:16`         | `720x1280`    |

<Note>
  Treat the generated file as the source of truth for `16:9` output dimensions. Tests of `agnes-video-2.5-flash` in September 2026 produced `1280x704` files for `720P` output.
</Note>

## Flash-specific Errors

If one request contains multiple Flash validation errors, the API returns the first detected error in this order: `size`, `images`, `audios`, then `videos`.

<Tabs>
  <Tab title="Non-720P size">
    ```json theme={null}
    {
      "detail": "size must be 720P"
    }
    ```
  </Tab>

  <Tab title="More than 5 images">
    ```json theme={null}
    {
      "detail": "images length must not exceed 5"
    }
    ```
  </Tab>

  <Tab title="More than 3 audio clips">
    ```json theme={null}
    {
      "detail": "audios length must not exceed 3"
    }
    ```
  </Tab>

  <Tab title="Reference video input">
    ```json theme={null}
    {
      "detail": "videos is not supported"
    }
    ```
  </Tab>
</Tabs>

All responses above use HTTP status `400`. Other error codes, task response fields, and failed-task formats are the same as Agnes Video 2.5.

## Integration Checklist

* Use the model ID `agnes-video-2.5-flash`.
* Set `size` to the string `"720P"`.
* In `mode=reference`, provide no more than 5 images.
* In `mode=reference`, provide no more than 3 audio clips.
* In `mode=reference`, do not provide non-empty `videos` content.
* Pass `seconds` as a string from `"4"`–`"12"` and set `n` to `1`.
* For every mode, retrieve tasks with both `video_id` and `model_name=agnes-video-2.5-flash`. A `video_id` query without `model_name` is valid only for `mode: "text"`.
* Never expose the API key in client code, logs, or public repositories.

## Billing

<Info>
  Agnes Video 2.5 Flash uses the same billing formula as Agnes Video 2.5. It is currently free for a limited time.
</Info>

```text theme={null}
Total video cost = output seconds × output resolution unit price
                 + input video seconds × output resolution unit price
                 + max(0, image count - free image allowance) × excess image unit price
```

At list price, the free image allowance is 5. Agnes Video 2.5 Flash supports only 720P and accepts no more than 5 reference images and 3 reference audio clips.

| Output resolution | List price            | Current price     |
| ----------------- | --------------------- | ----------------- |
| 720P              | ~~`$0.025 / second`~~ | **`$0 / second`** |

During the limited-time free promotion, output duration, input video duration, and reference images are billed at `$0`. Promotional pricing may change; refer to the latest Agnes AI platform announcement.
