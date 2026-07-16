<!--
source: https://wiki.agnes-ai.com/en/docs/agnes-video-v20.md
mirror (zh-Hans): https://agnes-ai.com/zh-Hans/docs/agnes-video-v20
downloaded: 2026-07-04
note: 异步任务模型；num_frames 必须满足 8n+1 且 ≤ 441。
-->

# Agnes Video V2.0

> An asynchronous video generation API for text-to-video, image-to-video, multi-image video generation, and keyframe animation.

<Info>
  Agnes Video V2.0 is a production-oriented video generation model that supports text-to-video, image-to-video, multi-image video generation, and keyframe animation. Video generation uses an asynchronous task API: create a task first, then retrieve the result through `video_id` or `task_id`.
</Info>

<CardGroup cols={2}>
  <Card title="Model" icon="cube">
    `agnes-video-v2.0`
  </Card>

  <Card title="Create Task" icon="video">
    `POST /v1/videos`
  </Card>

  <Card title="Get Result" icon="link">
    `GET /agnesapi?video_id=<VIDEO_ID>`
  </Card>

  <Card title="Current Price" icon="tag">
    Video duration is currently `$0 / second`.
  </Card>
</CardGroup>

## Overview

Developers can generate high-quality videos from text prompts, image URLs, or multiple reference images. The model is suitable for storytelling, marketing videos, product demos, social media content, app motion assets, and AI creative workflows.

## Core Capabilities

<CardGroup cols={2}>
  <Card title="Text-to-video" icon="clapperboard">
    Generate videos directly from text prompts.
  </Card>

  <Card title="Image-to-video" icon="image">
    Turn static images into dynamic videos.
  </Card>

  <Card title="Multi-image Video" icon="layer-group">
    Use multiple reference images to guide video generation.
  </Card>

  <Card title="Keyframe Animation" icon="timeline">
    Generate smooth transitions between multiple keyframes.
  </Card>

  <Card title="Motion Control" icon="camera">
    Control subject actions, camera movement, and scene dynamics through prompts.
  </Card>

  <Card title="Visual Consistency" icon="eye">
    Maintain subject, style, and scene consistency across frames.
  </Card>

  <Card title="Cinematic Output" icon="film">
    Generate high-quality cinematic video content.
  </Card>

  <Card title="Asynchronous API" icon="clock">
    Create a task first, then poll or query the generated result.
  </Card>
</CardGroup>

## Use Cases

<CardGroup cols={2}>
  <Card title="Storytelling" icon="book-open">
    Short films, character scenes, and narrative clips.
  </Card>

  <Card title="Marketing Videos" icon="bullhorn">
    Product ads, promotional videos, and campaign content.
  </Card>

  <Card title="Social Media Content" icon="share-nodes">
    Reels, Shorts, and TikTok-style videos.
  </Card>

  <Card title="Image Animation" icon="wand-magic-sparkles">
    Add motion to portraits, products, characters, or scenes.
  </Card>

  <Card title="Product Demos" icon="box">
    Generate product showcase videos from text or images.
  </Card>

  <Card title="Keyframe Transitions" icon="arrows-left-right">
    Generate smooth transitions between different visual states.
  </Card>
</CardGroup>

## Prerequisites

<Note>
  Before integration, make sure you have a valid Agnes AI API key, network access to the Agnes AI API gateway, and a text prompt for video generation. Image-to-video, multi-image video, and keyframe animation workflows also require publicly accessible image URLs.
</Note>

## API Reference

### Create Video Task

```text theme={null}
POST https://apihub.agnes-ai.com/v1/videos
```

### Get Video Result: Recommended Method

```text theme={null}
GET https://apihub.agnes-ai.com/agnesapi?video_id=<VIDEO_ID>
```

### Get Video Result: Legacy-compatible Method

```text theme={null}
GET https://apihub.agnes-ai.com/v1/videos/<TASK_ID>
```

### Headers

```bash theme={null}
-H "Authorization: Bearer YOUR_API_KEY"
-H "Content-Type: application/json"
```

## Create Task Parameters

| Parameter             | Type    | Required | Description                                                        |
| --------------------- | ------- | -------- | ------------------------------------------------------------------ |
| `model`               | string  | Yes      | Model name. Use `agnes-video-v2.0`.                                |
| `prompt`              | string  | Yes      | Text description of the video content.                             |
| `image`               | string  | No       | Image URL for image-to-video workflows.                            |
| `mode`                | string  | No       | Generation mode, such as `ti2vid` or `keyframes`.                  |
| `height`              | integer | No       | Video height. Default value: `768`.                                |
| `width`               | integer | No       | Video width. Default value: `1152`.                                |
| `num_frames`          | integer | No       | Number of frames. Must be `<= 441` and follow the `8n + 1` rule.   |
| `frame_rate`          | number  | No       | Frame rate. Supported range: `1-60`.                               |
| `num_inference_steps` | integer | No       | Number of inference steps.                                         |
| `seed`                | integer | No       | Random seed for reproducible results.                              |
| `negative_prompt`     | string  | No       | Negative prompt describing content to avoid.                       |
| `extra_body.image`    | array   | No       | Input image URL array for multi-image video or keyframe workflows. |
| `extra_body.mode`     | string  | No       | Additional mode setting, such as `keyframes`.                      |

## Parameter Normalization

<Info>
  Agnes Video V2.0 normalizes some video generation parameters to ensure stable output quality. When the submitted `width`, `height`, or aspect ratio does not exactly match supported model specifications, the system maps the request to the closest standard output configuration.
</Info>

The model currently supports three standard resolution tiers: `480p`, `720p`, and `1080p`.

| Aspect Ratio | Recommended Use Case                                                             |
| ------------ | -------------------------------------------------------------------------------- |
| `16:9`       | Landscape videos, product demos, website showcases, YouTube-style content.       |
| `9:16`       | Vertical short videos, mobile-first content, TikTok / Reels / Shorts.            |
| `1:1`        | Square videos, social media feeds, character or product showcases.               |
| `4:3`        | Traditional landscape format and general presentation content.                   |
| `3:4`        | Vertical presentation videos, portrait-focused content, product-focused content. |

<Note>
  The original `width`, `height`, and `num_frames` values in the request may not exactly match the normalized generation settings. When displaying task information, calculating video duration, or debugging generation results, use the `size` and `seconds` fields returned by the API response as the source of truth.
</Note>

## Create Task Examples

<Tabs>
  <Tab title="Text-to-video">
    ```bash theme={null}
    curl -X POST https://apihub.agnes-ai.com/v1/videos \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-v2.0",
        "prompt": "A cinematic shot of a cat walking on the beach at sunset, soft ocean waves, warm golden lighting, realistic motion",
        "height": 768,
        "width": 1152,
        "num_frames": 121,
        "frame_rate": 24
      }'
    ```
  </Tab>

  <Tab title="Image-to-video">
    ```bash theme={null}
    curl -X POST https://apihub.agnes-ai.com/v1/videos \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-v2.0",
        "prompt": "The woman slowly turns around and looks back at the camera, natural facial expression, cinematic camera movement",
        "image": "https://example.com/image.png",
        "num_frames": 121,
        "frame_rate": 24
      }'
    ```
  </Tab>

  <Tab title="Multi-image Video">
    ```bash theme={null}
    curl -X POST https://apihub.agnes-ai.com/v1/videos \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-v2.0",
        "prompt": "Create a smooth transformation scene between the two reference images, cinematic lighting, consistent character identity, natural motion",
        "extra_body": {
          "image": [
            "https://example.com/image1.png",
            "https://example.com/image2.png"
          ]
        },
        "num_frames": 121,
        "frame_rate": 24
      }'
    ```
  </Tab>

  <Tab title="Keyframe Animation">
    ```bash theme={null}
    curl -X POST https://apihub.agnes-ai.com/v1/videos \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-video-v2.0",
        "prompt": "Generate a smooth cinematic transition between the keyframes, maintaining visual consistency and natural camera movement",
        "extra_body": {
          "image": [
            "https://example.com/keyframe1.png",
            "https://example.com/keyframe2.png"
          ],
          "mode": "keyframes"
        },
        "num_frames": 121,
        "frame_rate": 24
      }'
    ```
  </Tab>
</Tabs>

## Create Task Response

When the video task is created successfully, the API returns task information.

The response includes both `task_id` and `video_id`. `video_id` is the recommended ID for retrieving video results.

```json theme={null}
{
  "id": "task_YOUR_TASK_ID",
  "task_id": "task_YOUR_TASK_ID",
  "video_id": "video_YOUR_VIDEO_ID",
  "object": "video",
  "model": "agnes-video-v2.0",
  "status": "queued",
  "progress": 0,
  "created_at": 1780457477,
  "seconds": "10.0",
  "size": "1280x768"
}
```

### Response Fields

| Field        | Type    | Description                                            |
| ------------ | ------- | ------------------------------------------------------ |
| `id`         | string  | Task ID. Can be used with the legacy query endpoint.   |
| `task_id`    | string  | Task ID. Same purpose as `id`.                         |
| `video_id`   | string  | Video ID. Recommended for retrieving the video result. |
| `object`     | string  | Object type, usually `video`.                          |
| `model`      | string  | Model used for the task.                               |
| `status`     | string  | Current task status.                                   |
| `progress`   | integer | Current task progress percentage.                      |
| `created_at` | integer | Task creation timestamp.                               |
| `seconds`    | string  | Video duration in seconds.                             |
| `size`       | string  | Video resolution.                                      |

## Get Video Result

<Tabs>
  <Tab title="By video_id">
    ```bash theme={null}
    curl --location --request GET 'https://apihub.agnes-ai.com/agnesapi?video_id=<VIDEO_ID>' \
      --header 'Authorization: Bearer <API_KEY>'
    ```
  </Tab>

  <Tab title="By video_id + model_name">
    ```bash theme={null}
    curl --location --request GET 'https://apihub.agnes-ai.com/agnesapi?video_id=<VIDEO_ID>&model_name=agnes-video-v2.0' \
      --header 'Authorization: Bearer <API_KEY>'
    ```

    Use `model_name` when you are using an upstream original video ID, the model is not the default `agnes-video-v2.0`, or you want to explicitly specify the model used to retrieve the result.
  </Tab>

  <Tab title="Legacy: By task_id">
    ```bash theme={null}
    curl --location --request GET 'https://apihub.agnes-ai.com/v1/videos/<TASK_ID>' \
      --header 'Authorization: Bearer <API_KEY>'
    ```
  </Tab>
</Tabs>

## Final Result Response

When the task is completed, the API returns the final video result.

```json theme={null}
{
  "id": "task_YOUR_TASK_ID",
  "video_id": "video_YOUR_VIDEO_ID",
  "model": "agnes-video-v2.0",
  "object": "video",
  "status": "completed",
  "progress": 100,
  "seconds": "10.0",
  "size": "1280x768",
  "remixed_from_video_id": "https://storage.googleapis.com/agnes-aigc/aigc/videos/2026/06/03/video_xxxxxx.mp4",
  "error": null
}
```

### Result Fields

| Field                   | Type          | Description                                                        |
| ----------------------- | ------------- | ------------------------------------------------------------------ |
| `id`                    | string        | Task ID.                                                           |
| `video_id`              | string        | Video ID.                                                          |
| `model`                 | string        | Model used for the task.                                           |
| `object`                | string        | Object type.                                                       |
| `status`                | string        | Task status.                                                       |
| `progress`              | integer       | Task progress percentage.                                          |
| `seconds`               | string        | Video duration in seconds.                                         |
| `size`                  | string        | Video resolution.                                                  |
| `remixed_from_video_id` | string        | Final generated video URL. Available when `status` is `completed`. |
| `error`                 | object / null | Error information if the task fails.                               |

## Task Status

| Status        | Description                           |
| ------------- | ------------------------------------- |
| `queued`      | The task is waiting in the queue.     |
| `in_progress` | The video is being generated.         |
| `completed`   | The video was generated successfully. |
| `failed`      | The video generation task failed.     |

## Video Duration Control

Agnes Video V2.0 supports controlling video duration with `num_frames` and `frame_rate`.

```text theme={null}
seconds = num_frames / frame_rate
```

* `num_frames` is the total number of generated frames.
* `frame_rate` is the playback frame rate in frames per second.
* `num_frames` must be less than or equal to `441`.
* `num_frames` must follow the `8n + 1` rule.
* `frame_rate` supports values from `1` to `60`.

### Common Duration Settings

| Target Duration  | Recommended Parameters              |
| ---------------- | ----------------------------------- |
| About 3 seconds  | `num_frames: 81`, `frame_rate: 24`  |
| About 5 seconds  | `num_frames: 121`, `frame_rate: 24` |
| About 10 seconds | `num_frames: 241`, `frame_rate: 24` |
| About 18 seconds | `num_frames: 441`, `frame_rate: 24` |

<Tip>
  To generate longer videos, increase `num_frames` or reduce `frame_rate`. To make motion smoother, use a higher `frame_rate`, such as `24` or `30`. At the same `num_frames`, a higher `frame_rate` results in a shorter video.
</Tip>

## Recommended Parameters

| Scenario                  | Recommended Settings                                              |
| ------------------------- | ----------------------------------------------------------------- |
| Standard video generation | `width: 1152`, `height: 768`, `num_frames: 121`, `frame_rate: 24` |
| Social short videos       | `num_frames: 81` or `121`, `frame_rate: 24`                       |
| Longer videos             | Increase `num_frames` or reduce `frame_rate`.                     |
| Smoother motion           | Use `frame_rate: 24` or `30`.                                       |
| Reproducible results      | Set a fixed `seed`.                                                      |
| Keyframe transition       | Use `extra_body.mode: "keyframes"`.                                |
| Avoid unwanted content    | Use `negative_prompt`.                                             |

## Prompt Best Practices

<AccordionGroup>
  <Accordion title="Text-to-video Prompts">
    Describe the subject, action, scene, camera movement, lighting, and visual style.

    ```text theme={null}
    [Subject] + [Action] + [Scene] + [Camera Movement] + [Lighting] + [Style]
    ```

    ```text theme={null}
    A young astronaut walking across a red desert planet, dust blowing in the wind, slow cinematic tracking shot, dramatic sunset lighting, realistic sci-fi style
    ```
  </Accordion>

  <Accordion title="Image-to-video Prompts">
    Describe what should move and which key subject elements should remain stable.

    ```text theme={null}
    Animate the character with subtle breathing motion, hair moving gently in the wind, background lights flickering softly, while keeping the face and outfit consistent
    ```
  </Accordion>

  <Accordion title="Multi-image Video Prompts">
    Describe the relationship between the input images and how the scene should transition.

    ```text theme={null}
    Use the first image as the starting scene and the second image as the target scene. Create a smooth transformation with consistent lighting, natural motion, and cinematic pacing
    ```
  </Accordion>

  <Accordion title="Keyframe Animation Prompts">
    Clearly describe the transition relationship between keyframes.

    ```text theme={null}
    Create a smooth transition from the first keyframe to the second keyframe, maintaining character identity, consistent camera angle, and natural motion between scenes
    ```
  </Accordion>
</AccordionGroup>

## Error Codes

| Status Code | Description                                    |
| ----------- | ---------------------------------------------- |
| `400`       | Invalid request. Check the request parameters. |
| `401`       | Unauthorized. Check your API key.              |
| `404`       | Task or video not found.                       |
| `500`       | Server error.                                  |
| `503`       | Service is busy. Try again later.              |

## Pricing

| Type           | Standard Price    | Current Price |
| -------------- | ----------------- | ------------- |
| Video duration | `$0.005 / second` | `$0 / second` |

## Integration Checklist

<Check>
  Use `agnes-video-v2.0` as the model name.
</Check>

<Check>
  Video generation is asynchronous. Create a task first, then retrieve the result.
</Check>

<Check>
  New integrations should use `video_id` to retrieve video results.
</Check>

<Check>
  Use publicly accessible image URLs for image-to-video, multi-image video, and keyframe workflows.
</Check>

<Check>
  Use the response fields `seconds` and `size` as the source of truth after parameter normalization.
</Check>