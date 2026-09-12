> ## Documentation Index
> Fetch the complete documentation index at: https://wiki.agnes-ai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agnes 2.5 Pro Beta

> Integrate Agnes 2.5 Pro Beta for advanced reasoning, coding, long-context analysis, agentic workflows, and multimodal understanding.

<Info>
  Agnes 2.5 Pro Beta is now available through the Agnes AI API. It uses the same request parameters, response formats, supported text protocols, and context limits as the other Agnes 2.5 Pro models. Its pricing is listed below.
</Info>

<CardGroup cols={2}>
  <Card title="Model" icon="cube">
    `agnes-2.5-pro-beta`
  </Card>

  <Card title="API Endpoints" icon="link">
    Chat Completions: `POST /v1/chat/completions`

    <br />

    Responses: `POST /v1/responses`

    <br />

    Messages: `POST /v1/messages`
  </Card>

  <Card title="Model Type" icon="brain">
    Paid reasoning model with text and image input.
  </Card>

  <Card title="Benchmark Reference" icon="chart-line">
    Evaluated on the Artificial Analysis intelligence and agentic benchmark suite.
  </Card>
</CardGroup>

## Overview

Agnes 2.5 Pro Beta is intended for production workloads that need stronger reasoning depth than flash-class models, including complex code tasks, science and math reasoning, long-context understanding, knowledge-intensive Q\&A, and agentic terminal or workflow tasks.

It uses the same base integration pattern as other Agnes text models:

| Item             | Value                            |
| ---------------- | -------------------------------- |
| Base URL         | `https://apihub.agnes-ai.com/v1` |
| Endpoint         | `POST /v1/chat/completions`      |
| Model name       | `agnes-2.5-pro-beta`             |
| Input modalities | Text and image URL               |
| Output modality  | Text                             |
| Availability     | Available                        |
| Billing          | Paid model                       |

<Note>
  The request and response examples on this page use the live model ID `agnes-2.5-pro-beta`. Before calling it, confirm that the model is enabled for your Agnes AI account.
</Note>

## Core Capabilities

<CardGroup cols={2}>
  <Card title="Advanced Reasoning" icon="brain">
    Suitable for scientific reasoning, knowledge-intensive questions, and multi-step analysis.
  </Card>

  <Card title="Coding and Terminal Tasks" icon="terminal">
    Designed for code generation, debugging, refactoring, test generation, and agentic coding workflows.
  </Card>

  <Card title="Long-context Analysis" icon="file-lines">
    Handles long documents, structured context, and multi-turn reasoning tasks.
  </Card>

  <Card title="Image Understanding" icon="eye">
    Accepts image URL inputs for visual analysis and multimodal reasoning.
  </Card>

  <Card title="Tool Calling" icon="wrench">
    Supports OpenAI-compatible function calling and external tool orchestration.
  </Card>

  <Card title="Streaming" icon="bolt">
    Supports streaming responses for interactive product experiences.
  </Card>
</CardGroup>

## Artificial Analysis Results

The following launch snapshot was measured by Artificial Analysis. Leaderboard positions and scores may change as the evaluation suite and competing models are updated.

| Metric                                    |                                       Score |
| ----------------------------------------- | ------------------------------------------: |
| Intelligence Index                        |                                        `49` |
| Agentic Index                             |                                        `44` |
| GDPval-AA v2 Elo                          |                                      `1456` |
| GDPval-AA v2                              |                                       `48%` |
| τ³-Banking                                |                                       `36%` |
| Terminal-Bench v2.1                       |                                       `70%` |
| SciCode                                   |                                       `48%` |
| Humanity's Last Exam                      |                                       `38%` |
| GPQA Diamond                              |                                       `91%` |
| CritPt                                    |                                       `16%` |
| AA-Omniscience Accuracy                   |                                       `17%` |
| AA-Omniscience Non-Hallucination Rate     |                                       `67%` |
| AA-LCR                                    |                                       `78%` |
| Output tokens per Intelligence Index task | `50k` total (`41k` answer + `9k` reasoning) |

<img src="https://mintcdn.com/agnestech/Z2UPMplt0pRFXA_p/images/agnes-25-pro-beta/intelligence-index.jpg?fit=max&auto=format&n=Z2UPMplt0pRFXA_p&q=85&s=34926e8138993a63340015854d29fd53" alt="Artificial Analysis Intelligence Index for Agnes 2.5 Pro Beta" width="1800" height="757" data-path="images/agnes-25-pro-beta/intelligence-index.jpg" />

<AccordionGroup>
  <Accordion title="Agentic Index">
    <img src="https://mintcdn.com/agnestech/Z2UPMplt0pRFXA_p/images/agnes-25-pro-beta/agentic-index.jpg?fit=max&auto=format&n=Z2UPMplt0pRFXA_p&q=85&s=07a8ae87c444851a831545355b077544" alt="Artificial Analysis Agentic Index" width="1800" height="732" data-path="images/agnes-25-pro-beta/agentic-index.jpg" />
  </Accordion>

  <Accordion title="GDPval-AA v2 Elo leaderboard">
    <img src="https://mintcdn.com/agnestech/Z2UPMplt0pRFXA_p/images/agnes-25-pro-beta/gdpval-aa-v2.jpg?fit=max&auto=format&n=Z2UPMplt0pRFXA_p&q=85&s=7b09e0fc4493e2dfea7a1b008a7a905b" alt="GDPval-AA v2 leaderboard" width="1800" height="798" data-path="images/agnes-25-pro-beta/gdpval-aa-v2.jpg" />
  </Accordion>

  <Accordion title="Intelligence evaluation breakdown">
    <img src="https://mintcdn.com/agnestech/Z2UPMplt0pRFXA_p/images/agnes-25-pro-beta/intelligence-evaluations.jpg?fit=max&auto=format&n=Z2UPMplt0pRFXA_p&q=85&s=3a0a0b738f61c0eec16e6c703b48114e" alt="Artificial Analysis intelligence evaluation breakdown" width="1173" height="1800" data-path="images/agnes-25-pro-beta/intelligence-evaluations.jpg" />
  </Accordion>

  <Accordion title="AA-Omniscience metrics">
    <img src="https://mintcdn.com/agnestech/Z2UPMplt0pRFXA_p/images/agnes-25-pro-beta/omniscience.jpg?fit=max&auto=format&n=Z2UPMplt0pRFXA_p&q=85&s=e7ac5f5b9363b248b82260e17683b490" alt="Artificial Analysis AA-Omniscience metrics" width="1357" height="1800" data-path="images/agnes-25-pro-beta/omniscience.jpg" />
  </Accordion>

  <Accordion title="Output tokens per Intelligence Index task">
    <img src="https://mintcdn.com/agnestech/Z2UPMplt0pRFXA_p/images/agnes-25-pro-beta/output-tokens.jpg?fit=max&auto=format&n=Z2UPMplt0pRFXA_p&q=85&s=8a80034bce7228930135b86bb5fbdb13" alt="Output tokens per Intelligence Index task" width="1800" height="782" data-path="images/agnes-25-pro-beta/output-tokens.jpg" />
  </Accordion>
</AccordionGroup>

<Card title="Artificial Analysis Leaderboard" icon="chart-line" href="https://artificialanalysis.ai/models">
  View the current Artificial Analysis leaderboard. Live scores and ordering may differ from this launch snapshot.
</Card>

## API Reference

### Endpoint

```text theme={null}
POST https://apihub.agnes-ai.com/v1/chat/completions
```

### Headers

```bash theme={null}
-H "Authorization: Bearer YOUR_API_KEY"
-H "Content-Type: application/json"
```

### Request Parameters

| Parameter              | Type            | Required | Description                                                                                            |
| ---------------------- | --------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| `model`                | string          | Yes      | Model name. Use `agnes-2.5-pro-beta`.                                                                  |
| `messages`             | array           | Yes      | Conversation messages, including `system`, `user`, and `assistant` messages.                           |
| `messages[].content`   | string / array  | Yes      | Message content. It can be plain text or an array of content blocks containing `text` and `image_url`. |
| `temperature`          | number          | No       | Controls randomness. Lower values produce more deterministic output.                                   |
| `top_p`                | number          | No       | Controls nucleus sampling.                                                                             |
| `max_tokens`           | number          | No       | Maximum number of tokens to generate in the response.                                                  |
| `stream`               | boolean         | No       | Whether to enable streaming output.                                                                    |
| `tools`                | array           | No       | Tool definitions for tool-calling workflows.                                                           |
| `tool_choice`          | string / object | No       | Controls whether and how the model uses tools.                                                         |
| `chat_template_kwargs` | object          | No       | Extension field for OpenAI-compatible requests.                                                        |
| `thinking`             | object          | No       | Field for enabling Thinking mode in Anthropic-compatible requests.                                     |

## Image URL Input

Agnes 2.5 Pro Beta supports text and image URL inputs in the same `messages` request.

```json theme={null}
{
  "role": "user",
  "content": [
    {
      "type": "text",
      "text": "Analyze this architecture diagram and identify possible failure points."
    },
    {
      "type": "image_url",
      "image_url": {
        "url": "https://example.com/diagram.png"
      }
    }
  ]
}
```

## Request Examples

<Tabs>
  <Tab title="Basic Chat">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/chat/completions \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-2.5-pro-beta",
        "messages": [
          {
            "role": "system",
            "content": "You are a precise technical assistant."
          },
          {
            "role": "user",
            "content": "Explain the tradeoffs between optimistic locking and pessimistic locking in distributed systems."
          }
        ],
        "temperature": 0.3,
        "max_tokens": 1200
      }'
    ```
  </Tab>

  <Tab title="Coding">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/chat/completions \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-2.5-pro-beta",
        "messages": [
          {
            "role": "user",
            "content": "Review this TypeScript API handler for security issues, explain the risks, and provide a corrected version."
          }
        ],
        "temperature": 0.2,
        "max_tokens": 2000
      }'
    ```
  </Tab>

  <Tab title="Streaming">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/chat/completions \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-2.5-pro-beta",
        "messages": [
          {
            "role": "user",
            "content": "Create a step-by-step migration plan for moving a monolith to services."
          }
        ],
        "stream": true
      }'
    ```
  </Tab>

  <Tab title="Image Understanding">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/chat/completions \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-2.5-pro-beta",
        "messages": [
          {
            "role": "user",
            "content": [
              {
                "type": "text",
                "text": "Summarize this chart and call out any anomalies."
              },
              {
                "type": "image_url",
                "image_url": {
                  "url": "https://example.com/chart.png"
                }
              }
            ]
          }
        ]
      }'
    ```
  </Tab>
</Tabs>

## Response Format

```json theme={null}
{
  "id": "chatcmpl_xxx",
  "object": "chat.completion",
  "created": 1784899200,
  "model": "agnes-2.5-pro-beta",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "..."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 120,
    "completion_tokens": 300,
    "total_tokens": 420
  }
}
```

### Response Fields

| Field                       | Type    | Description                             |
| --------------------------- | ------- | --------------------------------------- |
| `id`                        | string  | Unique ID of the completion request.    |
| `object`                    | string  | Object type, usually `chat.completion`. |
| `created`                   | integer | Request timestamp.                      |
| `model`                     | string  | Model used for the request.             |
| `choices`                   | array   | List of generated responses.            |
| `choices[].message.role`    | string  | Role of the message sender.             |
| `choices[].message.content` | string  | Content generated by the model.         |
| `choices[].finish_reason`   | string  | Reason generation stopped.              |
| `usage`                     | object  | Token usage information.                |

## Responses API

In addition to Chat Completions, this model supports the OpenAI Responses API. Use `input` instead of `messages`.

### Responses endpoint

```text theme={null}
POST https://apihub.agnes-ai.com/v1/responses
```

### Responses request parameters

| Parameter           | Type           | Required | Description                                                                                       |
| ------------------- | -------------- | -------- | ------------------------------------------------------------------------------------------------- |
| `model`             | string         | Yes      | Model name. Use `agnes-2.5-pro-beta`.                                                             |
| `input`             | string / array | Yes      | A plain text prompt or an array of structured input messages.                                     |
| `max_output_tokens` | integer        | No       | Maximum output budget. Use a larger value for reasoning models to avoid an `incomplete` response. |

<Tabs>
  <Tab title="Text Input">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/responses \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-2.5-pro-beta",
        "input": "Explain how autonomous agents use tools.",
        "max_output_tokens": 1024
      }'
    ```
  </Tab>

  <Tab title="Structured Input">
    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/responses \
      -H "Authorization: Bearer YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "agnes-2.5-pro-beta",
        "input": [
          {
            "role": "user",
            "content": [
              {
                "type": "input_text",
                "text": "Explain how autonomous agents use tools."
              }
            ]
          }
        ],
        "max_output_tokens": 1024
      }'
    ```
  </Tab>
</Tabs>

### Responses output format

```json theme={null}
{
  "id": "resp_xxx",
  "object": "response",
  "status": "completed",
  "model": "agnes-2.5-pro-beta",
  "output": [
    {
      "type": "reasoning",
      "summary": []
    },
    {
      "type": "message",
      "role": "assistant",
      "status": "completed",
      "content": [
        {
          "type": "output_text",
          "text": "Autonomous agents use tools to retrieve data and perform actions."
        }
      ]
    }
  ],
  "usage": {
    "input_tokens": 40,
    "output_tokens": 80,
    "total_tokens": 120
  },
  "error": null,
  "incomplete_details": null
}
```

| Field                     | Type          | Description                                                         |
| ------------------------- | ------------- | ------------------------------------------------------------------- |
| `id`                      | string        | Unique response ID.                                                 |
| `object`                  | string        | Object type, usually `response`.                                    |
| `status`                  | string        | Response state, such as `completed` or `incomplete`.                |
| `output`                  | array         | Ordered response items, including reasoning and assistant messages. |
| `output[].type`           | string        | Item type, such as `reasoning` or `message`.                        |
| `output[].content[].type` | string        | Content type. Generated text uses `output_text`.                    |
| `output[].content[].text` | string        | Generated assistant text.                                           |
| `usage`                   | object        | Token usage information.                                            |
| `error`                   | object / null | Error details when the request fails.                               |
| `incomplete_details`      | object / null | Explains why a response stopped before completion.                  |

<Warning>
  The current response does not include a top-level `output_text` convenience field. Extract generated text from message items where `output[].type` is `message` and `output[].content[].type` is `output_text`.
</Warning>

<Note>
  Reasoning items are optional and can use either `content[].reasoning_text` or `summary[].summary_text`. Token usage field names can also vary by model: support both `input_tokens` / `output_tokens` and `prompt_tokens` / `completion_tokens`.
</Note>

<Tip>
  If `status` is `incomplete`, inspect `incomplete_details` and retry with a larger `max_output_tokens` value. Reasoning models can consume part of the output budget before producing assistant text.
</Tip>

## Messages API

This model also supports the Anthropic-compatible Messages API. Send conversation input in `messages` and authenticate with `x-api-key`.

### Messages endpoint

```text theme={null}
POST https://apihub.agnes-ai.com/v1/messages
```

### Messages headers

```bash theme={null}
-H "x-api-key: YOUR_API_KEY"
-H "anthropic-version: 2023-06-01"
-H "Content-Type: application/json"
```

### Messages request parameters

| Parameter            | Type           | Required | Description                                                               |
| -------------------- | -------------- | -------- | ------------------------------------------------------------------------- |
| `model`              | string         | Yes      | Model name. Use `agnes-2.5-pro-beta`.                                     |
| `max_tokens`         | integer        | Yes      | Maximum number of output tokens. Use a larger value for reasoning models. |
| `messages`           | array          | Yes      | Conversation messages containing `user` and `assistant` roles.            |
| `messages[].role`    | string         | Yes      | Message role. Use `user` or `assistant`.                                  |
| `messages[].content` | string / array | Yes      | Plain text or an array of Anthropic-compatible content blocks.            |
| `system`             | string / array | No       | System instruction for the request.                                       |
| `temperature`        | number         | No       | Controls output randomness.                                               |
| `stream`             | boolean        | No       | Whether to return a streaming response.                                   |

### Messages request example

```bash theme={null}
curl https://apihub.agnes-ai.com/v1/messages \
  -H "x-api-key: YOUR_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "agnes-2.5-pro-beta",
    "max_tokens": 1024,
    "system": "You are a helpful AI assistant.",
    "messages": [
      {
        "role": "user",
        "content": "Explain how autonomous agents use tools."
      }
    ]
  }'
```

### Messages response format

```json theme={null}
{
  "id": "msg_xxx",
  "type": "message",
  "role": "assistant",
  "model": "agnes-2.5-pro-beta",
  "content": [
    {
      "type": "text",
      "text": "Autonomous agents use tools to retrieve information and perform actions."
    }
  ],
  "stop_reason": "end_turn",
  "usage": {
    "input_tokens": 290,
    "cache_creation_input_tokens": 0,
    "cache_read_input_tokens": 0,
    "output_tokens": 28
  }
}
```

| Field                               | Type    | Description                                                    |
| ----------------------------------- | ------- | -------------------------------------------------------------- |
| `id`                                | string  | Unique message ID.                                             |
| `type`                              | string  | Object type, usually `message`.                                |
| `role`                              | string  | Response role, usually `assistant`.                            |
| `model`                             | string  | Model used for the request.                                    |
| `content`                           | array   | Ordered response content blocks.                               |
| `content[].type`                    | string  | Content block type. Generated text uses `text`.                |
| `content[].text`                    | string  | Generated assistant text.                                      |
| `stop_reason`                       | string  | Reason generation stopped, such as `end_turn` or `max_tokens`. |
| `usage.input_tokens`                | integer | Number of input tokens used.                                   |
| `usage.output_tokens`               | integer | Number of output tokens generated.                             |
| `usage.cache_creation_input_tokens` | integer | Input tokens written to the prompt cache.                      |
| `usage.cache_read_input_tokens`     | integer | Input tokens read from the prompt cache.                       |

<Note>
  Read generated text from content blocks where `content[].type` is `text`. If `stop_reason` is `max_tokens`, retry with a larger `max_tokens` value.
</Note>

## Limits and Pricing

Agnes 2.5 Pro Beta is a paid model. Its token prices are the same as the other Agnes 2.5 Pro models, and usage is billed by cache read, input, and output tokens.

| Item              | Value                        |
| ----------------- | ---------------------------- |
| Context window    | `1M` tokens                  |
| Max output        | `65536` tokens               |
| Input modalities  | Text, image                  |
| Output modalities | Text                         |
| Reasoning         | Yes                          |
| Model weights     | Proprietary, not open source |

| Type                         |          USD Price |
| ---------------------------- | -----------------: |
| Input cache hit / Cache Read | `$0.01 / M tokens` |
| Input cache miss / Input     | `$0.10 / M tokens` |
| Output                       | `$0.30 / M tokens` |

<Note>
  The cached-input price is 10% of the regular input-token price. Pricing and availability may vary by account, region, billing configuration, or later pricing updates. Use the Agnes AI platform dashboard as the source of truth for your account.
</Note>

## Best Practices

<AccordionGroup>
  <Accordion title="Reasoning-heavy Tasks">
    Use Agnes 2.5 Pro Beta for tasks where correctness and multi-step reasoning matter more than raw latency, such as scientific reasoning, complex policy analysis, and long-form technical planning.
  </Accordion>

  <Accordion title="Coding Tasks">
    Provide the target language, framework, existing code, error messages, expected behavior, and constraints. Ask for root-cause analysis before the patch when debugging complex issues.
  </Accordion>

  <Accordion title="Long-context Tasks">
    Use structured sections, filenames, or document labels inside the prompt so the model can refer to sources and produce traceable conclusions.
  </Accordion>
</AccordionGroup>

## Integration Checklist

<Check>
  Use `agnes-2.5-pro-beta` as the model name.
</Check>

<Check>
  Confirm that your Agnes AI account has access to Agnes 2.5 Pro Beta.
</Check>

<Check>
  Basic chat completion requests must include `model` and `messages`.
</Check>

<Check>
  Use publicly accessible `image_url` values for image inputs.
</Check>

<Check>
  Track cache read, input, and output token usage because this is a paid model.
</Check>
