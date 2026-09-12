> ## Documentation Index
> Fetch the complete documentation index at: https://wiki.agnes-ai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Quickstart

> Follow these step-by-step instructions to start using the Agnes AI API quickly and efficiently.

<Note>
  **Prerequisites**

  Before making any API requests, make sure you have the following:

  * An active Agnes AI Platform account
  * A valid API key (generated in the Agnes AI developer dashboard)
</Note>

<Steps>
  <Step title="Create an Account">
    Sign up for a new account, or log in to your existing Agnes AI Platform account. From the developer dashboard, you can manage API keys, billing, and more.
  </Step>

  <Step title="Generate an API Key">
    To authenticate your API requests, generate a secret API key in the Agnes AI Platform:

    Save this key securely. You will use it to authenticate all API requests (as described in the Authentication section):

    <span class="field-row"><code>Authorization: Bearer YOUR\_API\_KEY</code></span>
  </Step>

  <Step title="Make Your First Request">
    Below is a sample request to create a chat completion using `curl` (you can also use tools like Postman, Python requests, or other HTTP clients):

    ```bash theme={null}
    curl https://apihub.agnes-ai.com/v1/chat/completions \
    -H "Authorization: Bearer YOUR_API_KEY" \
    -H "Content-Type: application/json" \
    -d '{
        "model": "agnes-2.5-flash",
        "messages": [
          {
            "role": "user",
            "content": "Hello!"
          }
        ]
      }'
    ```

    <Tip>
      Replace `YOUR_API_KEY` with your actual API key before running the request. A successful response will return a chat completion that matches your input.
    </Tip>
  </Step>

  <Step title="Next Steps">
    After your first request, explore these next steps to get more out of the Agnes AI API:

    * Read the documentation for request parameters, response formats, and error handling for each API endpoint.
    * Integrate advanced features like streaming responses or tool calling to enhance your application's functionality.
  </Step>
</Steps>
