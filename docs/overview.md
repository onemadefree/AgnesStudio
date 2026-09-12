> ## Documentation Index
> Fetch the complete documentation index at: https://wiki.agnes-ai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Overview

> Agnes AI API Online Documentation

## 1. Agnes AI API Introduction

Agnes AI API provides developers with unified, stable, and easy-to-integrate multimodal AI model services, supporting text, image, video, and multimodal generation and understanding capabilities.

With Agnes AI API, developers can quickly build AI-native applications, including but not limited to:

* AI chat and text generation
* Logical reasoning and content understanding
* Text-to-image generation and image editing
* Image-to-video and video generation
* Synchronized audio-video generation
* Agent tools and automated workflows
* Creative content generation and multimodal interactive applications

Agnes AI API is compatible with OpenAI-style interfaces, making it easy for developers to migrate and integrate existing projects with minimal code changes, reducing development costs and improving integration efficiency.

## 2. Core Capabilities

Agnes AI API currently supports the following core capabilities:

<CardGroup cols={2}>
  <Card title="3.1 Text Generation & Reasoning">
    Supports high-quality text generation, content continuation, summarization, logical reasoning, question answering, code assistance, and agent task execution.

    Applicable scenarios include:

    * AI chat assistants
    * Content creation
    * Document summarization
    * Intelligent Q\&A
    * Code generation
    * Agent automation tasks
  </Card>

  <Card title="3.2 Image Generation & Editing">
    Supports generating high-definition images from text prompts, as well as editing, enhancing, and stylizing existing images.

    Applicable scenarios include:

    * Text-to-image generation
    * Image editing
    * Product image generation
    * Creative poster generation
    * Character image generation
    * Social media asset creation
  </Card>

  <Card title="3.3 Video & Synchronized Audio-Video Generation">
    Supports generating high-quality video content and synchronized audio-visual output, reducing post-production complexity and improving content creation efficiency.

    Applicable scenarios include:

    * AI video generation
    * Image-to-video generation
    * Short video creation
    * Creative advertising assets
    * Character animation
    * Synchronized audio-video content generation
  </Card>

  <Card title="3.4 Multimodal Understanding & Creation">
    Supports the combination of text, image, video, and reasoning capabilities, helping developers build smarter and more natural interactive experiences.

    Applicable scenarios include:

    * Multimodal AI assistants
    * Image understanding
    * Video understanding
    * Creative workflows
    * AI social interaction
    * Education, entertainment, and productivity tools
  </Card>
</CardGroup>

## 3. Model Capabilities Overview

Agnes AI provides multiple types of model capabilities, including text models, image models, video models, and multimodal models.

The Agnes AI model suite includes text, image, video, and multimodal models designed for reasoning, creative generation, and production application scenarios.

These models help developers quickly integrate a complete AI capability stack, from content understanding and text generation to image generation, image editing, and video generation.

## 4. API Compatibility

Agnes AI API is compatible with OpenAI-style interfaces.

If you have already used an OpenAI-Compatible API, you usually only need to modify the following configurations:

* `Base URL`
* `API Key`
* `Model Name`

This compatibility allows developers to migrate existing projects quickly, reduce integration costs, and avoid duplicate development work.

## 5. Base URL

All API requests should be sent using the following Base URL:

Please make sure your request path is correctly built based on this address to avoid request failures caused by incorrect paths.

## 6. Authentication

All API requests must be authenticated with an API Key.

Include the following parameter in the request header, replacing `YOUR_API_KEY` with your valid API Key:

<span class="field-row"><code>Authorization: Bearer YOUR\_API\_KEY</code></span>

## 7. Security Notice

<Warning>
  Your API Key is sensitive information. Please keep it secure.

  Do not expose your API Key in:

  * Public code repositories
  * Front-end client code
  * Screenshots or screen recordings
  * Public documents
  * Configuration files accessible by others

  If your API Key is exposed, we recommend deleting or resetting it immediately in the console to prevent unauthorized usage.
</Warning>

## 8. Quick Start

You can start using Agnes AI API by following these steps:

<Steps>
  <Step title="Create an API Key">
    Log in to the Agnes AI console, go to the API Key management page, then create and copy your API Key.
  </Step>

  <Step title="Select a Model">
    Choose the appropriate model based on your use case, such as a text model, image model, video model, or multimodal model.
  </Step>

  <Step title="Configure the Request Base URL">
    Set the API Base URL to:

    <span class="field-row"><code>[https://apihub.agnes-ai.com/v1](https://apihub.agnes-ai.com/v1)</code></span>
  </Step>

  <Step title="Send an API Request">
    Add your API Key to the request header and send requests using the OpenAI-Compatible API format.
  </Step>
</Steps>

## 9. Applicable Development Scenarios

Agnes AI API is suitable for the following product and business scenarios:

* AI chat applications
* AI search and research tools
* AI writing and office productivity tools
* AI image generation tools
* AI video generation tools
* AI character and interactive applications
* AI social products
* Agent automation platforms
* Creative content production platforms
* Education, entertainment, e-commerce, and marketing tools

## 10. Documentation Notes

This document helps developers quickly understand the basic capabilities, integration methods, and usage guidelines of Agnes AI API.

For detailed model parameters, request examples, response formats, and error code descriptions, please refer to the corresponding model-specific integration documents.
