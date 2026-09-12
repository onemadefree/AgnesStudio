> ## Documentation Index
> Fetch the complete documentation index at: https://wiki.agnes-ai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agnes Video 2.5

> Integrate Agnes Video 2.5 through an OpenAI Videos-compatible API for text-to-video, keyframe control, and multimodal reference generation.

<Info>
  Agnes Video 2.5 is now available on the international site and uses an asynchronous video generation API. First call `POST /v1/videos` to create a task, then use the returned `video_id` with `GET /agnesapi?video_id=<VIDEO_ID>&model_name=agnes-video-2.5` to retrieve progress and results. See “Billing” below for prices and billing rules.
</Info>

<CardGroup cols={2}>
  <Card title="Model ID" icon="cube">
    `agnes-video-2.5`
  </Card>

  <Card title="Create Task" icon="video">
    `POST /v1/videos`
  </Card>

  <Card title="Retrieve Task" icon="clock">
    `GET /agnesapi?video_id=<VIDEO_ID>&model_name=agnes-video-2.5`
  </Card>

  <Card title="Pricing" icon="tag">
    720P: `$0.025 / second`; 1080P and 1K: `$0.040 / second`; 2K: `$0.055 / second`.
  </Card>
</CardGroup>

## Core Capabilities

<CardGroup cols={2}>
  <Card title="Text-to-video" icon="clapperboard">
    Generate videos with subject motion, scene dynamics, and camera movement from a text prompt.
  </Card>

  <Card title="First and Last Frame Control" icon="images">
    Constrain the composition and transition with a first frame, a last frame, or both.
  </Card>

  <Card title="Multimodal References" icon="layer-group">
    Use images, audio, and videos as content, style, rhythm, or motion references.
  </Card>

  <Card title="Video-to-video Reference" icon="film">
    Continue or reinterpret motion, visual appearance, and timing from a reference video.
  </Card>

  <Card title="Audio-visual Coordination" icon="waveform">
    Use audio or a video soundtrack as a reference to improve rhythm and audio-visual consistency.
  </Card>

  <Card title="Multiple Aspect Ratios" icon="expand">
    Generate landscape, portrait, square, and ultrawide video outputs.
  </Card>
</CardGroup>

## Quickstart

### 1. Get an API Key

Create an API key in the Agnes AI platform. Store and use the key only on your server. Never expose it in frontend code or a public repository.

### 2. Set the Base URL

International Base URL:

```text theme={null}
https://apihub.agnes-ai.com/v1
```

The examples below use environment variables:

```bash theme={null}
export AGNES_API_KEY="YOUR_API_KEY"
export AGNES_BASE_URL="https://apihub.agnes-ai.com/v1"
```

### 3. Create a Video Task

```bash theme={null}
curl -sS -X POST "$AGNES_BASE_URL/videos" \
  -H "Authorization: Bearer $AGNES_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "agnes-video-2.5",
    "prompt": "A futuristic city street after rain, neon lights reflected on the pavement, a silver sports car passing slowly, cinematic camera movement, natural ambient sound",
    "seconds": "5",
    "mode": "text",
    "size": "720P",
    "aspect_ratio": "16:9"
  }'
```

Save `video_id` from the create response. `id` and `task_id` identify the asynchronous task, while `video_id` is used to retrieve progress and results.

### 4. Retrieve the Result

```bash theme={null}
curl -sS "https://apihub.agnes-ai.com/agnesapi?video_id=VIDEO_ID&model_name=agnes-video-2.5" \
  -H "Authorization: Bearer $AGNES_API_KEY"
```

Use the query form with `model_name` for every mode. Poll every `1–2` seconds until `status` becomes `completed` or `failed`. When the task is complete, use `metadata.url` to play or download the video.

## API Reference

### Create a Video Task

```text theme={null}
POST https://apihub.agnes-ai.com/v1/videos
```

Headers:

```http theme={null}
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

### Common Request Parameters

| Parameter      | Type    | Required | Description                                                                                                    |
| -------------- | ------- | -------- | -------------------------------------------------------------------------------------------------------------- |
| `model`        | string  | Yes      | Model ID. Use `agnes-video-2.5`.                                                                               |
| `prompt`       | string  | Yes      | Video description. In reference mode, use `<Picture N>`, `<Audio N>`, and `<Video N>` to refer to input media. |
| `mode`         | string  | Yes      | Generation mode: `text`, `keyframe`, or `reference`.                                                           |
| `seconds`      | string  | No       | Video duration from `"4"`–`"12"` seconds. The default is `"5"`.                                                |
| `size`         | string  | No       | Output resolution tier. Supported values are `"720P"`, `"1080P"`, `"1K"`, and `"2K"`.                          |
| `aspect_ratio` | string  | No       | Output aspect ratio. The default is `16:9`. See supported values below.                                        |
| `seed`         | integer | No       | Random seed. Reusing a seed can improve reproducibility.                                                       |
| `n`            | integer | No       | Number of outputs. Currently only `1` is supported and is the default.                                         |

### Mode-specific Parameters

| Parameter     | Type      | Mode        | Description                                                 |
| ------------- | --------- | ----------- | ----------------------------------------------------------- |
| `first_frame` | string    | `keyframe`  | First-frame image URL. Provide this, `last_frame`, or both. |
| `last_frame`  | string    | `keyframe`  | Last-frame image URL. Provide this, `first_frame`, or both. |
| `images`      | string\[] | `reference` | Reference image URLs.                                       |
| `audios`      | string\[] | `reference` | Reference audio URLs.                                       |
| `videos`      | object\[] | `reference` | Reference video objects. See the fields below.              |

All media URLs must be publicly reachable by the Agnes AI service. Avoid URLs that require authentication, point to a private network, or expire before the task finishes.

### Generation Mode Rules

| `mode`      | Purpose                                         | Required Media                                               | Media Fields Not Allowed                                  |
| ----------- | ----------------------------------------------- | ------------------------------------------------------------ | --------------------------------------------------------- |
| `text`      | Generate a video from text only                 | None                                                         | `first_frame`, `last_frame`, `images`, `audios`, `videos` |
| `keyframe`  | Control the first frame, last frame, or both    | At least one of `first_frame` or `last_frame`                | `images`, `audios`, `videos`                              |
| `reference` | Generate from image, audio, or video references | At least one non-empty `images`, `audios`, or `videos` array | `first_frame`, `last_frame`                               |

<Tip>
  `keyframe` attempts to preserve the input image as the actual first or last frame, making it suitable for start/end composition control. `reference` treats media as a content, style, motion, or rhythm reference and may recompose or retime the result.
</Tip>

### Reference Video Objects

Each object in the `videos` array supports these fields:

| Parameter       | Type    | Required | Description                                                                    |
| --------------- | ------- | -------- | ------------------------------------------------------------------------------ |
| `url`           | string  | Yes      | Publicly accessible video URL.                                                 |
| `start_seconds` | number  | No       | Start reading the reference video at this offset. The default is `0`.          |
| `require_audio` | boolean | No       | Require the reference video to contain an audio track. The default is `false`. |

When `require_audio` is `false`, a reference video may omit audio. If it contains an audio track, the audio can also participate as a reference. When set to `true`, the source must contain an audio track or the request fails.

### Reference Media Limits

The following limits apply to Agnes Video 2.5 reference media:

| Media  | Limits                                                                                                                                                        |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Images | Up to 8 images; each image must be smaller than 15 MB; the total request must be smaller than 50 MB; each dimension must be in the range `256`–`5760` pixels. |
| Videos | Up to 1 video; total duration `2`–`12` seconds; the video must be smaller than 50 MB; frame rate `24`–`60` FPS.                                               |
| Audio  | Up to 3 audio files; total duration `2`–`12` seconds; each file must be smaller than 15 MB; the total request must be smaller than 64 MB.                     |

The total number of reference media files in one request must not exceed 12.

## Request Examples

<Tabs>
  <Tab title="Text-to-video">
    ```bash theme={null}
    curl -sS -X POST "$AGNES_BASE_URL/videos" \
      -H "Authorization: Bearer $AGNES_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-2.5",
        "prompt": "At night, three cats march through a forest while playing tiny brass instruments. The camera tracks backward smoothly, moonlight passes through the leaves, with natural footsteps and instrument sounds.",
        "seconds": "5",
        "mode": "text",
        "size": "720P",
        "aspect_ratio": "16:9",
        "seed": 1101
      }'
    ```
  </Tab>

  <Tab title="First and Last Frames">
    ```bash theme={null}
    curl -sS -X POST "$AGNES_BASE_URL/videos" \
      -H "Authorization: Bearer $AGNES_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-2.5",
        "prompt": "The person turns naturally from the first-frame pose and walks toward the window. Keep clothing and hair motion realistic, slowly push the camera forward, and transition smoothly to the last-frame composition.",
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
        "model": "agnes-video-2.5",
        "prompt": "Use the character and art style in <Picture 1> as reference. The character runs naturally through a flower field, maintaining a consistent appearance, filmed from a low tracking angle.",
        "seconds": "5",
        "mode": "reference",
        "size": "720P",
        "aspect_ratio": "16:9",
        "images": ["https://example.com/character.png"]
      }'
    ```
  </Tab>

  <Tab title="Image and Audio References">
    ```bash theme={null}
    curl -sS -X POST "$AGNES_BASE_URL/videos" \
      -H "Authorization: Bearer $AGNES_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-2.5",
        "prompt": "Use <Picture 1> as the visual subject and design the actions and camera cuts around the rhythm of <Audio 1>, keeping the sequence natural and coherent.",
        "seconds": "5",
        "mode": "reference",
        "size": "720P",
        "images": ["https://example.com/subject.png"],
        "audios": ["https://example.com/music.mp3"]
      }'
    ```
  </Tab>

  <Tab title="Video Reference">
    ```bash theme={null}
    curl -sS -X POST "$AGNES_BASE_URL/videos" \
      -H "Authorization: Bearer $AGNES_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-2.5",
        "prompt": "Follow the subject motion and camera rhythm of <Video 1>, changing the setting to a moonlit bedroom while preserving coherent timing.",
        "seconds": "5",
        "mode": "reference",
        "size": "720P",
        "aspect_ratio": "16:9",
        "videos": [
          {
            "url": "https://example.com/input.mp4",
            "start_seconds": 35,
            "require_audio": false
          }
        ]
      }'
    ```
  </Tab>
</Tabs>

<Note>
  `<Picture N>`, `<Audio N>`, and `<Video N>` are numbered independently, starting from `1` in their respective arrays. For example, refer to the second item in `images` as `<Picture 2>`.
</Note>

## Create Response

```json theme={null}
{
  "id": "task_YOUR_TASK_ID",
  "task_id": "task_YOUR_TASK_ID",
  "video_id": "video_YOUR_VIDEO_ID",
  "object": "video",
  "model": "agnes-video-2.5",
  "status": "queued",
  "progress": 0,
  "created_at": 1786900000,
  "seconds": "5",
  "size": "720P"
}
```

| Field        | Type    | Description                                                           |
| ------------ | ------- | --------------------------------------------------------------------- |
| `id`         | string  | Task ID. It identifies the same task as `task_id`.                    |
| `task_id`    | string  | ID of the asynchronous generation task.                               |
| `video_id`   | string  | Video ID used to retrieve task progress and results.                  |
| `object`     | string  | Object type. Always `video`.                                          |
| `model`      | string  | Model used for the task.                                              |
| `status`     | string  | `queued`, `in_progress`, `completed`, or `failed`.                    |
| `progress`   | integer | Task progress from `0–100`.                                           |
| `created_at` | integer | Task creation time as a Unix timestamp.                               |
| `seconds`    | string  | Video duration in seconds.                                            |
| `size`       | string  | Output resolution tier returned for the task, such as `720P` or `2K`. |

## Retrieve a Task

<Tabs>
  <Tab title="By video_id + model_name (Recommended)">
    ```bash theme={null}
    curl -sS "https://apihub.agnes-ai.com/agnesapi?video_id=video_YOUR_VIDEO_ID&model_name=agnes-video-2.5" \
      -H "Authorization: Bearer $AGNES_API_KEY"
    ```

    This form works for `text`, `keyframe`, and `reference` modes and is the recommended polling method for Agnes Video 2.5.
  </Tab>

  <Tab title="By video_id (text mode only)">
    ```bash theme={null}
    curl -sS "https://apihub.agnes-ai.com/agnesapi?video_id=video_YOUR_VIDEO_ID" \
      -H "Authorization: Bearer $AGNES_API_KEY"
    ```

    This form is valid only for tasks created with `mode: "text"`. Queries for `keyframe` and `reference` tasks must include `model_name=agnes-video-2.5`.
  </Tab>
</Tabs>

Completed response example:

```json theme={null}
{
  "id": "task_YOUR_TASK_ID",
  "video_id": "video_YOUR_VIDEO_ID",
  "task_id": "task_YOUR_TASK_ID",
  "object": "video",
  "model": "agnes-video-2.5",
  "status": "completed",
  "progress": 100,
  "created_at": 1786900000,
  "completed_at": 1786900120,
  "seconds": "5",
  "size": "720P",
  "metadata": {
    "url": "https://example.com/generated/video.mp4"
  }
}
```

| Field                   | Type            | Description                                                           |
| ----------------------- | --------------- | --------------------------------------------------------------------- |
| `id`                    | string          | Task ID. It identifies the same task as `task_id`.                    |
| `video_id`              | string          | Video ID used to retrieve the task.                                   |
| `task_id`               | string          | ID of the asynchronous generation task.                               |
| `object`                | string          | Object type. Always `video`.                                          |
| `model`                 | string          | Model used for the task.                                              |
| `status`                | string          | `queued`, `in_progress`, `completed`, or `failed`.                    |
| `progress`              | integer         | Task progress from `0–100`.                                           |
| `created_at`            | integer         | Task creation time as a Unix timestamp.                               |
| `completed_at`          | integer \| null | Task completion time, or `null` before completion.                    |
| `seconds`               | string          | Video duration in seconds.                                            |
| `size`                  | string          | Output resolution tier returned for the task, such as `720P` or `2K`. |
| `metadata`              | object \| null  | Result metadata. It may be empty before completion.                   |
| `metadata.url`          | string          | Final video URL after the task completes.                             |
| `metadata.size_mapping` | object \| null  | Size-mapping information returned by the service, when available.     |
| `error`                 | object \| null  | Error details when a task fails.                                      |

<Tip>
  Treat `status` and `metadata.url` as the source of truth. The URL is ready for delivery only when `status` is `completed`. In production, set a maximum polling duration and use backoff for network timeouts and `429` responses.
</Tip>

## Python SDK Example

Pass `mode`, `aspect_ratio`, and media fields through `extra_body`; the SDK merges them into the top level of the request JSON.

```python theme={null}
import os
import time
import requests
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["AGNES_API_KEY"],
    base_url="https://apihub.agnes-ai.com/v1",
)

video = client.videos.create(
    model="agnes-video-2.5",
    prompt="Follow the motion and timing of <Video 1>, while changing the setting to a moonlit room.",
    seconds="5",
    size="720P",
    extra_body={
        "mode": "reference",
        "aspect_ratio": "16:9",
        "videos": [
            {
                "url": "https://example.com/input.mp4",
                "start_seconds": 0,
                "require_audio": False,
            }
        ],
    },
)

video_id = getattr(video, "video_id", None) or video.id

while True:
    time.sleep(1.5)
    response = requests.get(
        "https://apihub.agnes-ai.com/agnesapi",
        params={"video_id": video_id, "model_name": "agnes-video-2.5"},
        headers={"Authorization": f"Bearer {os.environ['AGNES_API_KEY']}"},
        timeout=30,
    )
    response.raise_for_status()
    video = response.json()
    if video.get("status") in ("completed", "failed"):
        break

if video.get("status") == "failed":
    message = video.get("error", {}).get("message", "Video generation failed")
    raise RuntimeError(message)

print(video["metadata"]["url"])
```

## Video Size and Aspect Ratio

`size` selects the output resolution tier and accepts `"720P"`, `"1080P"`, `"1K"`, or `"2K"`. `1K` means `1024x1024`. For `720P` and `1080P`, output dimensions follow the resolution mapping below. The `2K` dimensions are twice the corresponding `720P` dimensions. Use `aspect_ratio` to select the frame shape. `WIDTHxHEIGHT` and `auto` are not supported.

| `size`  | Description                                                            |
| ------- | ---------------------------------------------------------------------- |
| `720P`  | Standard resolution for faster generation and general-purpose content. |
| `1080P` | High-definition output using the 1080P dimension mapping below.        |
| `1K`    | Fixed `1024x1024` output.                                              |
| `2K`    | Highest resolution tier for high-quality delivery.                     |

The following table shows the output pixel mapping for each aspect ratio. Actual dimensions returned by the API are the source of truth.

| `aspect_ratio` | 720P       | 1080P       | 2K (2× 720P) | Recommended Use                                             |
| -------------- | ---------- | ----------- | ------------ | ----------------------------------------------------------- |
| `21:9`         | `1470x630` | `2206x946`  | `2940x1260`  | Ultrawide and cinematic scenes.                             |
| `16:9`         | `1280x720` | `1920x1080` | `2560x1440`  | Landscape video and product showcases. This is the default. |
| `4:3`          | `1112x834` | `1664x1248` | `2224x1668`  | General landscape and traditional video formats.            |
| `1:1`          | `960x960`  | `1440x1440` | `1920x1920`  | Square social feeds and product or character showcases.     |
| `3:4`          | `834x1112` | `1248x1664` | `1668x2224`  | Portrait presentations and character-focused content.       |
| `9:16`         | `720x1280` | `1080x1920` | `1440x2560`  | Mobile short-form and vertical video.                       |

## Parameter Restrictions

The following parameters and request patterns are not supported and return `400`:

* Passing reference videos through `video_url`, `video_path`, or `video_reference`; use `videos[].url` instead.
* Passing media through `input_reference` or `reference_url`; use `first_frame`, `last_frame`, `images`, `audios`, or `videos` according to the selected mode.
* Sending non-configurable fields such as `width`, `height`, `fps`, `num_frames`, `quality`, or `num_inference_steps`.
* Passing pixel dimensions such as `1280x720` directly in `size`, or a value other than `"720P"`, `"1080P"`, `"1K"`, or `"2K"`; select the resolution tier with `size` and the frame shape with `aspect_ratio`.
* Setting `aspect_ratio` to `auto` or a value outside the supported list.
* Setting `n` to any value other than `1`.
* Using media fields that conflict with `mode`, or using `reference` without any reference media.

## Error Handling

| HTTP Status   | Common Cause                                                              | Recommended Action                                            |
| ------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------- |
| `400`         | Missing fields, invalid mode/media combination, duration, or aspect ratio | Check request fields and mode validation rules.               |
| `401` / `403` | Invalid, expired, or unauthorized API key                                 | Check the authorization header, key status, and model access. |
| `404`         | Video ID does not exist                                                   | Use the `video_id` from the create response.                  |
| `429`         | Request rate exceeds the limit                                            | Use exponential backoff and reduce polling frequency.         |
| `500`         | Internal service error                                                    | Retry later and contact support if the error persists.        |

Failed task example:

```json theme={null}
{
  "id": "task_YOUR_TASK_ID",
  "video_id": "video_YOUR_VIDEO_ID",
  "task_id": "task_YOUR_TASK_ID",
  "object": "video",
  "model": "agnes-video-2.5",
  "status": "failed",
  "progress": 100,
  "metadata": null,
  "error": {
    "message": "Invalid reference media"
  }
}
```

## Prompting Recommendations

For more consistent results, structure the prompt in this order:

1. **Subject and setting**: Specify the people, objects, environment, and time.
2. **Action and change**: Describe how the subject moves and how the scene evolves.
3. **Camera language**: Specify push, pull, pan, tilt, tracking, fixed camera, or shot size.
4. **Visual style**: Add lighting, color, material, realism, and atmosphere.
5. **Sound and rhythm**: Describe ambient sound or action sounds, or reference an audio input.
6. **Consistency requirements**: State which character, product, or composition details must remain unchanged.

<Tip>
  In `reference` mode, explicitly name each media placeholder and its purpose, such as “Use `<Picture 1>` as the character reference and follow the rhythm of `<Audio 1>`.” This is more controllable than uploading media without explaining how it should be used.
</Tip>

## Integration Checklist

* Use the model ID `agnes-video-2.5`.
* Use `https://apihub.agnes-ai.com/v1` as the Base URL.
* Save `video_id` from the create response; `id` and `task_id` identify the asynchronous task.
* For every mode, use `GET /agnesapi?video_id=<VIDEO_ID>&model_name=agnes-video-2.5` until the status is `completed` or `failed`. A `video_id` query without `model_name` is valid only for `mode: "text"`.
* Keep all media URLs publicly accessible until the task completes.
* Pass `seconds` as a string from `"4"`–`"12"` and set `n` to `1`.
* Set `size` to `"720P"`, `"1080P"`, `"1K"`, or `"2K"` and use a supported `aspect_ratio`.
* Never expose your API key in logs, client-side code, or public repositories.

## Billing

<Info>
  Agnes Video 2.5 is billed by output resolution and total billable duration. Total billable duration is the sum of output video duration and input video duration. The first 5 input images are free; each input image from the 6th onward costs `$0.005`.
</Info>

### Prices

| Output resolution | List price        |
| ----------------- | ----------------- |
| 720P              | `$0.025 / second` |
| 1080P             | `$0.040 / second` |
| 1K                | `$0.040 / second` |
| 2K                | `$0.055 / second` |

### Billing formula

```text theme={null}
Total video cost = output seconds × output resolution unit price
                 + input video seconds × output resolution unit price
                 + max(0, image count - free image allowance) × excess image unit price
```

For Agnes Video 2.5, the free image allowance is 5 and the excess image unit price is `$0.005 / image`.

| Billing item | Rule                                                                                                          |
| ------------ | ------------------------------------------------------------------------------------------------------------- |
| Output video | Output video duration is included in total billable duration and charged at the output resolution unit price. |
| Input images | The first 5 images are free. Each image from the 6th onward costs `$0.005`.                                   |
| Input video  | Input video duration is added to output video duration and charged at the output resolution unit price.       |

<Note>
  The output resolution unit price is based on the requested `size`: 720P is `$0.025 / second`, 1080P and 1K are `$0.040 / second`, and 2K is `$0.055 / second`.
</Note>

### Points billing

Points use the same metering structure as currency billing, but each points unit price differs from the corresponding currency amount:

```text theme={null}
Points = output seconds × output resolution points rate
       + input video seconds × output resolution points rate
       + max(0, image count - free image allowance) × excess image points rate
```

Points use the same metering structure as currency billing. Refer to the Agnes AI platform for the points rate of each resolution tier and the excess-image points rate.

### Billing example

Suppose you generate an 8-second 720P video using a 3-second input video and 7 input images:

```text theme={null}
Total cost = (8 + 3) × $0.025 + max(0, 7 - 5) × $0.005
           = $0.275 + $0.01
           = $0.285
```

The 3-second input video is added to the 8-second output video, resulting in 11 billable seconds. The first 5 input images are free, so only 2 images incur an additional charge.
