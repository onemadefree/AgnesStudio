> ## Documentation Index
> Fetch the complete documentation index at: https://wiki.agnes-ai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agnes 3.0 Flash

> A new-generation Agnes AI text model with stronger Agnes Code task execution, tool orchestration, and trustworthy delivery.

<Info>
  Agnes 3.0 Flash is Agnes AI's all-new next-generation text model for agentic coding and tool-driven tasks, with a focus on stronger end-to-end execution quality.
</Info>

<CardGroup cols={2}>
  <Card title="Model" icon="cube">
    `agnes-3.0-flash`
  </Card>

  <Card title="API endpoints" icon="link">
    Chat Completions: `POST /v1/chat/completions`

    <br />

    Responses: `POST /v1/responses`

    <br />

    Messages: `POST /v1/messages`
  </Card>

  <Card title="Model type" icon="brain">
    New-generation text model with text and image-URL input.
  </Card>

  <Card title="Core focus" icon="bolt">
    Focused on stable execution, reliable tool use, long-task context retention, and high-quality delivery.
  </Card>

  <Card title="Pricing" icon="tags">
    Cached input: `$0.005 / 1M tokens`; input: `$0.05 / 1M tokens`; output: `$0.15 / 1M tokens`. All three items are currently `$0`.
  </Card>
</CardGroup>

## Overview

Agnes 3.0 Flash is designed for real-world agent tasks and development workflows, covering the full execution path from task understanding and planning to tool use and final delivery. It focuses on stability, instruction following, grounded execution, and output integrity in complex tasks, helping developers build more reliable agent applications.

Use the following API information to integrate the model:

| Item             | Value                            |
| ---------------- | -------------------------------- |
| Base URL         | `https://apihub.agnes-ai.com/v1` |
| Chat Completions | `POST /v1/chat/completions`      |
| Responses API    | `POST /v1/responses`             |
| Messages API     | `POST /v1/messages`              |
| Model name       | `agnes-3.0-flash`                |
| Input modalities | Text and image URL               |
| Output modality  | Text                             |
| Context window   | `512K`                           |
| Maximum output   | `65,536 tokens`                  |

## Core directions

<CardGroup cols={2}>
  <Card title="Agnes Code task execution" icon="terminal">
    Better suited to Agnes Code and coding-agent workflows, strengthening execution from requirements understanding through final delivery.
  </Card>

  <Card title="Tool calling and orchestration" icon="wrench">
    Stronger function calling, tool selection, and multi-step tool orchestration for more stable agent execution.
  </Card>

  <Card title="Instruction and context adherence" icon="list-check">
    Maintains objectives, constraints, and runtime context throughout long-running and multi-turn tasks.
  </Card>

  <Card title="Trustworthy delivery" icon="shield-check">
    Strengthens factual grounding, result verification, and output integrity while reducing unsupported conclusions and incorrect completion claims.
  </Card>
</CardGroup>

## Capability highlights

<AccordionGroup>
  <Accordion title="More reliable end-to-end delivery">
    Improved task planning, execution, and result verification help complex agent tasks reach genuine completion with stronger delivery quality.
  </Accordion>

  <Accordion title="More stable tool orchestration">
    The model interprets tool definitions, selects appropriate tools, and coordinates multi-step calls more reliably, reducing ineffective calls, repetition, and abnormal loops.
  </Accordion>

  <Accordion title="Stronger instruction and context adherence">
    Long-running and multi-turn tasks stay aligned with the original objective, constraints, and runtime context, reducing task drift and missed requirements.
  </Accordion>

  <Accordion title="More trustworthy execution results">
    Greater attention to tool results and factual grounding reduces unsupported conclusions and incorrect completion claims, making execution status more transparent and dependable.
  </Accordion>

  <Accordion title="Cleaner, more complete output">
    Reduced repetition, malformed text, and unnecessary exposure of internal reasoning produce clearer, more coherent responses that are ready for delivery.
  </Accordion>
</AccordionGroup>

## Artificial Analysis Results

As accessed on September 11, 2026, the Artificial Analysis model page reports the following results for Agnes 3.0 Flash:

| Metric                                      |                  Result |
| ------------------------------------------- | ----------------------: |
| Artificial Analysis Intelligence Index v4.3 |                    `36` |
| Intelligence ranking within its model class |                `1 / 61` |
| Output speed                                | `252.7 tokens / second` |
| Output speed ranking within its model class |                `5 / 61` |

<Note>
  The Intelligence Index is currently marked as an estimate, with independent evaluation forthcoming. Scores, rankings, and methodology may change as evaluations are updated. Refer to the source page for the latest results.
</Note>

<Card title="View the full results" icon="chart-line" href="https://artificialanalysis.ai/models/agnes-3-0-flash">
  Explore Agnes 3.0 Flash evaluations, model comparisons, and methodology on Artificial Analysis.
</Card>

## Chat Completions API

### Endpoint and headers

```text theme={null}
POST https://apihub.agnes-ai.com/v1/chat/completions
```

```bash theme={null}
-H "Authorization: Bearer YOUR_API_KEY"
-H "Content-Type: application/json"
```

### Request fields

Agnes 3.0 Flash supports the following request fields.

| Field                  | Type            | Required | Description                                                         |
| ---------------------- | --------------- | -------- | ------------------------------------------------------------------- |
| `model`                | string          | Yes      | Use `agnes-3.0-flash`.                                              |
| `messages`             | array           | Yes      | Conversation messages with `system`, `user`, and `assistant` roles. |
| `messages[].content`   | string / array  | Yes      | Plain text or content blocks containing `text` and `image_url`.     |
| `temperature`          | number          | No       | Controls output randomness.                                         |
| `top_p`                | number          | No       | Controls nucleus sampling.                                          |
| `max_tokens`           | integer         | No       | Maximum number of output tokens.                                    |
| `stream`               | boolean         | No       | Returns a streamed response when `true`.                            |
| `tools`                | array           | No       | Tool definitions for function-calling workflows.                    |
| `tool_choice`          | string / object | No       | Controls whether and how the model calls tools.                     |
| `chat_template_kwargs` | object          | No       | Extension field for Thinking and other compatible features.         |

## Image URL input

Pass public image URLs together with text in `messages[].content`.

```json theme={null}
{
  "role": "user",
  "content": [
    {
      "type": "text",
      "text": "Describe the key information in this image."
    },
    {
      "type": "image_url",
      "image_url": {
        "url": "https://example.com/image.jpg"
      }
    }
  ]
}
```

### Basic request

```bash theme={null}
curl https://apihub.agnes-ai.com/v1/chat/completions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "agnes-3.0-flash",
    "messages": [
      {
        "role": "user",
        "content": "Explain how an agent should choose and call a tool."
      }
    ],
    "max_tokens": 1024
  }'
```

### Response format

Chat Completions responses use an OpenAI-compatible structure:

```json theme={null}
{
  "id": "chatcmpl_xxx",
  "object": "chat.completion",
  "model": "agnes-3.0-flash",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "An agent should select a tool based on the task and the tool's declared capability."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 24,
    "completion_tokens": 18,
    "total_tokens": 42
  }
}
```

### Tool-calling request

```json theme={null}
{
  "model": "agnes-3.0-flash",
  "messages": [
    {
      "role": "user",
      "content": "What is the weather in Shanghai?"
    }
  ],
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_weather",
        "description": "Get the current weather for a city.",
        "parameters": {
          "type": "object",
          "properties": {
            "city": { "type": "string" }
          },
          "required": ["city"]
        }
      }
    }
  ]
}
```

Read generated text from `choices[].message.content`. For a tool call, inspect `choices[].message.tool_calls`, execute the requested function in your application, append its result to `messages`, and submit the next Chat Completions request.

## Responses API

The Responses API accepts text or structured messages through `input`.

```text theme={null}
POST https://apihub.agnes-ai.com/v1/responses
```

```bash theme={null}
curl https://apihub.agnes-ai.com/v1/responses \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "agnes-3.0-flash",
    "input": "Summarize this task and list the tools you would need.",
    "max_output_tokens": 1024
  }'
```

| Field               | Type           | Required | Description                                     |
| ------------------- | -------------- | -------- | ----------------------------------------------- |
| `model`             | string         | Yes      | Use `agnes-3.0-flash`.                          |
| `input`             | string / array | Yes      | Plain-text prompt or structured input messages. |
| `max_output_tokens` | integer        | No       | Maximum output budget.                          |

Read generated text from a message item in `output` where `output[].type` is `message` and `output[].content[].type` is `output_text`. The response object may also contain `usage`, `error`, and `incomplete_details`.

## Messages API

Agnes 3.0 Flash supports the Anthropic-compatible Messages API.

```text theme={null}
POST https://apihub.agnes-ai.com/v1/messages
```

```bash theme={null}
curl https://apihub.agnes-ai.com/v1/messages \
  -H "x-api-key: YOUR_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "agnes-3.0-flash",
    "max_tokens": 1024,
    "messages": [
      {
        "role": "user",
        "content": "Draft a concise product announcement and list the next actions."
      }
    ]
  }'
```

| Field                | Type           | Required | Description                                        |
| -------------------- | -------------- | -------- | -------------------------------------------------- |
| `model`              | string         | Yes      | Use `agnes-3.0-flash`.                             |
| `max_tokens`         | integer        | Yes      | Maximum output-token budget.                       |
| `messages`           | array          | Yes      | Messages with `user` and `assistant` roles.        |
| `messages[].content` | string / array | Yes      | Plain text or Anthropic-compatible content blocks. |
| `system`             | string / array | No       | System instruction.                                |
| `temperature`        | number         | No       | Controls output randomness.                        |
| `stream`             | boolean        | No       | Returns a streamed response when `true`.           |

Read generated text from `content[]` items where `content[].type` is `text`.

## Thinking mode

Enable Thinking mode when your integration needs more deliberate task decomposition or reasoning.

<Tabs>
  <Tab title="OpenAI-compatible format">
    ```json theme={null}
    {
      "model": "agnes-3.0-flash",
      "messages": [
        {
          "role": "user",
          "content": "Plan the implementation steps for this repository task."
        }
      ],
      "chat_template_kwargs": {
        "enable_thinking": true
      }
    }
    ```
  </Tab>

  <Tab title="Anthropic-compatible format">
    ```json theme={null}
    {
      "model": "agnes-3.0-flash",
      "max_tokens": 2048,
      "messages": [
        {
          "role": "user",
          "content": "Plan the implementation steps for this repository task."
        }
      ],
      "thinking": {
        "type": "enabled",
        "budget_tokens": 2048
      }
    }
    ```
  </Tab>
</Tabs>

## Best practices

<AccordionGroup>
  <Accordion title="Agnes Code and agent tasks">
    State the task objective, repository or runtime context, constraints, expected output, and tool permissions. Return tool results to the conversation before asking the model for the next action.
  </Accordion>

  <Accordion title="Tool calling">
    Write narrow tool descriptions and JSON schemas. Validate tool arguments in your application before executing side-effecting actions.
  </Accordion>

  <Accordion title="Long-running tasks">
    Break complex work into verifiable stages, and preserve the objective, constraints, and key tool results across each execution round.
  </Accordion>
</AccordionGroup>

<Note>
  Model availability, rate limits, and billing are determined by your Agnes AI account and API key.
</Note>

## Limits and pricing

Availability and rate limits follow the entitlement shown for your Agnes AI account and API key.

| Item           | Value           |
| -------------- | --------------- |
| Context window | `512K`          |
| Maximum output | `65,536 tokens` |

| Billing item  |           List price |    Current price |
| ------------- | -------------------: | ---------------: |
| Cached input  | `$0.005 / 1M tokens` | `$0 / 1M tokens` |
| Input tokens  |  `$0.05 / 1M tokens` | `$0 / 1M tokens` |
| Output tokens |  `$0.15 / 1M tokens` | `$0 / 1M tokens` |
