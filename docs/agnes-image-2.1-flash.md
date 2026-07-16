<!--
source: https://wiki.agnes-ai.com/en/docs/agnes-image-21-flash.md
mirror (zh-Hans): https://agnes-ai.com/zh-Hans/docs/agnes-image-21-flash
downloaded: 2026-07-04
note: 官方源文档；图生图 image 必须放 extra_body 内，不要传 tags。
-->

# Agnes Image 2.1 Flash

> An upgraded image generation model optimized for high-information-density visuals, with support for text-to-image and image-to-image workflows.

<Info>
  Agnes Image 2.1 Flash is an upgraded image generation model from Sapiens AI. It supports text-to-image and image-to-image workflows. Compared with version 2.0, it is better suited for high-information-density images, complex compositions, and detail-rich visual scenes.
</Info>

<CardGroup cols={2}>
  <Card title="Model" icon="cube">
    `agnes-image-2.1-flash`
  </Card>

  <Card title="API Endpoint" icon="link">
    `POST /v1/images/generations`
  </Card>

  <Card title="Core Optimization" icon="chart-network">
    High-information-density images, complex visual details, and semantic alignment.
  </Card>

  <Card title="Current Price" icon="tag">
    Image generation is currently `$0 / image`.
  </Card>
</CardGroup>

## Overview

Agnes Image 2.1 Flash can generate images from text prompts and transform, redraw, or stylize input images. Results can be returned as image URLs or Base64 data.

<CardGroup cols={2}>
  <Card title="High-density Visuals" icon="mountain-sun">
    Better suited for complex scenes, rich compositions, and multi-layer visual elements.
  </Card>

  <Card title="Composition Preservation" icon="crop">
    Preserves the original composition and subject layout as much as possible during image-to-image editing.
  </Card>
</CardGroup>

## Core Capabilities

<CardGroup cols={2}>
  <Card title="Text-to-image" icon="wand-magic-sparkles">
    Generate high-quality images from natural language prompts.
  </Card>

  <Card title="Image-to-image" icon="images">
    Transform or improve existing images based on prompt instructions.
  </Card>

  <Card title="High-information-density Images" icon="chart-network">
    Optimized for detail-rich images with complex layouts and dense visual elements.
  </Card>

  <Card title="Composition Preservation" icon="crop">
    Preserve the original composition and subject layout when editing input images.
  </Card>

  <Card title="Flexible Size Control" icon="expand">
    Supports custom output sizes such as `1024x768`.
  </Card>

  <Card title="URL / Base64 Output" icon="file-code">
    Return generated images as URLs or Base64 data.
  </Card>
</CardGroup>

## Use Cases

<CardGroup cols={2}>
  <Card title="Creative Design" icon="pen-ruler">
    Concept art, visual exploration, and poster drafts.
  </Card>

  <Card title="Marketing Content" icon="bullhorn">
    Campaign images, product visuals, and social media creatives.
  </Card>

  <Card title="High-density Visual Generation" icon="mountain-sun">
    Detailed scenes, complex environments, and rich compositions.
  </Card>

  <Card title="Image Transformation" icon="paintbrush">
    Style transfer, scene relighting, and background transformation.
  </Card>

  <Card title="Product Visualization" icon="box">
    Product photos, mockups, and commercial visuals.
  </Card>

  <Card title="Social Media Assets" icon="share-nodes">
    Covers, banners, thumbnails, and post images.
  </Card>
</CardGroup>

## API Reference

### Endpoint

```text theme={null}
POST https://apihub.agnes-ai.com/v1/images/generations
```

### Headers

```bash theme={null}
-H "Authorization: Bearer YOUR_API_KEY"
-H "Content-Type: application/json"
```

### Request Parameters

| Parameter                    | Type      | Required                    | Description                                                       |
| ---------------------------- | --------- | --------------------------- | ----------------------------------------------------------------- |
| `model`                      | string    | Yes                         | Model name. Use `agnes-image-2.1-flash`.                          |
| `prompt`                     | string    | Yes                         | Text instruction for image generation or image editing.           |
| `size`                       | string    | Yes                         | Output image size, such as `1024x768`.                            |
| `image`                      | string\[] | Required for image-to-image | Input image array. Supports public image URLs or Data URI Base64. |
| `return_base64`              | boolean   | No                          | Use this when text-to-image output should be returned as Base64.  |
| `extra_body`                 | object    | No                          | Additional parameters for advanced workflows.                     |
| `extra_body.response_format` | string    | No                          | Output format. Common values are `url` and `b64_json`.            |

## Important Notes

<Warning>
  Do not place `response_format` at the top level of the request body. For URL output, use `extra_body.response_format: "url"`. For image-to-image Base64 output, use `extra_body.response_format: "b64_json"`.
</Warning>

<CardGroup cols={2}>
  <Card title="Text-to-image" icon="wand-magic-sparkles">
    Required parameters are `model`, `prompt`, and `size`.
  </Card>

  <Card title="Image-to-image" icon="images">
    Provide an input image URL or Data URI Base64 value in `extra_body.image`.
  </Card>

  <Card title="Base64 Output" icon="file-code">
    For text-to-image, use top-level `return_base64: true`.
  </Card>

  <Card title="No tags Required" icon="ban">
    Image-to-image requests do not require `tags: ["img2img"]`.
  </Card>
</CardGroup>

## Request Examples

<Tabs>
  <Tab title="Text-to-image: URL Output">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/images/generations \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-image-2.1-flash",
        "prompt": "A luminous floating city above a misty canyon at sunrise, cinematic realism",
        "size": "1024x768",
        "extra_body": {
          "response_format": "url"
        }
      }'
    ```
  </Tab>

  <Tab title="Text-to-image: Base64 Output">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/images/generations \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-image-2.1-flash",
        "prompt": "A clean product photo of a glass cube on a white studio background, soft shadows, high detail",
        "size": "1024x768",
        "return_base64": true
      }'
    ```
  </Tab>

  <Tab title="Image-to-image: URL Input + URL Output">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/images/generations \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-image-2.1-flash",
        "prompt": "Transform the scene into a rain-soaked cyberpunk night with neon reflections while preserving the original composition",
        "size": "1024x768",
        "extra_body": {
          "image": [
            "https://example.com/input-image.png"
          ],
          "response_format": "url"
        }
      }'
    ```
  </Tab>

  <Tab title="Image-to-image: Base64 Output">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/images/generations \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-image-2.1-flash",
        "prompt": "Make the object orange while preserving the original composition",
        "size": "1024x768",
        "extra_body": {
          "image": [
            "https://example.com/input-image.png"
          ],
          "response_format": "b64_json"
        }
      }'
    ```
  </Tab>

  <Tab title="Data URI Base64 Input">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/images/generations \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-image-2.1-flash",
        "prompt": "Make the object matte black while preserving the original composition",
        "size": "1024x768",
        "extra_body": {
          "image": [
            "data:image/png;base64,BASE64_HERE"
          ],
          "response_format": "b64_json"
        }
      }'
    ```
  </Tab>
</Tabs>

## Response Format

<Tabs>
  <Tab title="URL Output">
    ```json theme={null}
    {
      "created": 1780000000,
      "data": [
        {
          "url": "https://storage.googleapis.com/agnes-aigc/xxx.png",
          "b64_json": null,
          "revised_prompt": null
        }
      ]
    }
    ```

    Generated image URL path:

    ```text theme={null}
    data[0].url
    ```
  </Tab>

  <Tab title="Base64 Output">
    ```json theme={null}
    {
      "created": 1780000000,
      "data": [
        {
          "url": null,
          "b64_json": "iVBORw0KGgoAAAANSUhEUgAA...",
          "revised_prompt": null
        }
      ]
    }
    ```

    Generated Base64 image path:

    ```text theme={null}
    data[0].b64_json
    ```
  </Tab>
</Tabs>

### Response Fields

| Field                   | Type          | Description                                                   |
| ----------------------- | ------------- | ------------------------------------------------------------- |
| `created`               | integer       | Request creation timestamp.                                   |
| `data`                  | array         | List of generated image results.                              |
| `data[].url`            | string / null | Generated image URL. Usually `null` when using Base64 output. |
| `data[].b64_json`       | string / null | Base64 image data. Usually `null` when using URL output.      |
| `data[].revised_prompt` | string / null | Revised prompt if available. Otherwise `null`.                |

## Prompting Guide

<Tip>
  For better image generation results, use a clear prompt structure that describes the subject, environment, style, lighting, composition, and quality requirements.
</Tip>

<AccordionGroup>
  <Accordion title="Text-to-image Prompt Structure">
    ```text theme={null}
    [Subject] + [Scene / Environment] + [Style] + [Lighting] + [Composition] + [Quality Requirements]
    ```

    ```text theme={null}
    A luminous floating city above a misty canyon at sunrise, cinematic realism, wide-angle composition, rich architectural details, soft golden light, high visual density
    ```
  </Accordion>

  <Accordion title="Image-to-image Prompt Structure">
    Clearly describe what should change and what should remain unchanged.

    ```text theme={null}
    [Change Request] + [New Style / Scene] + [Elements to Add or Remove] + [Elements to Preserve]
    ```

    ```text theme={null}
    Turn the daytime street scene into a cinematic cyberpunk night scene, add neon signs and wet road reflections, while preserving the original street layout, camera angle, and main building shapes.
    ```
  </Accordion>

  <Accordion title="High-information-density Images">
    Agnes Image 2.1 Flash is optimized for complex and detail-rich visuals. For best results, describe the visual hierarchy clearly.

    Recommended elements:

    * Main subject
    * Background environment
    * Important secondary details
    * Style and lighting
    * Composition constraints
    * Elements to preserve for image-to-image tasks

    ```text theme={null}
    A large fantasy harbor city built on cliffs, hundreds of small boats, layered stone bridges, glowing windows, distant mountains, cloudy sunset sky, cinematic fantasy realism, wide-angle composition, rich architectural details, high visual density
    ```
  </Accordion>
</AccordionGroup>

## Common Errors and Troubleshooting

<AccordionGroup>
  <Accordion title="Top-level response_format causes errors">
    Do not place `response_format` at the top level.

    Incorrect:

    ```json theme={null}
    {
      "model": "agnes-image-2.1-flash",
      "prompt": "A futuristic city",
      "size": "1024x768",
      "response_format": "url"
    }
    ```

    Correct:

    ```json theme={null}
    {
      "model": "agnes-image-2.1-flash",
      "prompt": "A futuristic city",
      "size": "1024x768",
      "extra_body": {
        "response_format": "url"
      }
    }
    ```
  </Accordion>

  <Accordion title="Image-to-image does not require tags">
    Do not pass `tags: ["img2img"]`. Image-to-image only requires an input image array.

    ```json theme={null}
    {
      "model": "agnes-image-2.1-flash",
      "prompt": "Make the image blue while preserving the original composition",
      "size": "1024x768",
      "extra_body": {
        "image": ["https://example.com/input.png"],
        "response_format": "url"
      }
    }
    ```
  </Accordion>

  <Accordion title="Input image URL cannot be accessed">
    Use public HTTPS image URLs that do not require login, cookies, or private request headers. If the image cannot be made public, use Data URI Base64 input.
  </Accordion>

  <Accordion title="Request timeout">
    Image generation may take several seconds to tens of seconds depending on prompt complexity, image size, and server load. A client timeout of `60s - 360s` is recommended.
  </Accordion>

  <Accordion title="Missing image parameter for image-to-image">
    Image-to-image generation requires an input image array in `extra_body.image`.
  </Accordion>
</AccordionGroup>

## Pricing

| Type             | Standard Price   | Current Price |
| ---------------- | ---------------- | ------------- |
| Image generation | `$0.003 / image` | `$0 / image`  |

## Integration Checklist

<Check>
  Use `agnes-image-2.1-flash` as the model name.
</Check>

<Check>
  Use `https://apihub.agnes-ai.com/v1/images/generations` as the API endpoint.
</Check>

<Check>
  For text-to-image, include `model`, `prompt`, and `size`.
</Check>

<Check>
  For image-to-image, provide input images through `extra_body.image`.
</Check>

<Check>
  Do not expose temporary or real API keys in public documentation.
</Check>