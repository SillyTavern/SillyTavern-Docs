---
order: -10
icon: code-review
---

# Function Calling

Function Calling allows adding dynamic functionality to your extensions by letting the LLM use structured data that you then can use to trigger a specific functionality of the extension.

## Example use cases

1. Query external APIs for additional information (news, weather, web search, etc.).
2. Perform calculations or conversions based on user input.
3. Store and recall important memories or facts, including RAG and database queries.
4. Introduce true randomness into the conversation (dice rolls, coin flips, etc.).

## Officially supported extensions using function calling

1. [Image Generation](/extensions/Stable-Diffusion.md) (built-in) - generate images based on user prompts.
2. [Web Search](/extensions/WebSearch.md) - trigger a web search for a query.
3. [RSS](https://github.com/SillyTavern/Extension-RSS/) - fetch the latest news from RSS feeds.
4. [AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather) - fetch the weather information from AccuWeather.
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - roll dice for D&D games.

## Prerequisites and limitations

1. This feature is only available for certain Chat Completion sources: OpenAI, Claude, MistralAI, Groq, Cohere, OpenRouter, AI21, Google AI Studio, Google Vertex AI (Express mode), DeepSeek and Custom API sources.
2. Text Completion APIs don't support function calls, but some locally-hosted backends like Ollama and TabbyAPI may run in Custom OpenAI-compatible mode under Chat Completion.
3. The support for function calling must be explicitly allowed by the user first. This is done by enabling the "Enable function calling" option in the AI Response Configuration panel.
4. There is no guarantee that an LLM will perform any function calls at all. Most of them require an explicit "activation" through the prompt (e.g., the user asking to "Roll a dice", "Get the weather", etc.).
5. Not all prompts can trigger a tool call. Continuations, impersonation, background ('quiet') prompts are not allowed to trigger a tool call. They can still use past successful tool calls in their responses.

## How to make a function tool

### Check if the feature is supported

To determine if the function tool calling feature is supported, you can call `isToolCallingSupported` from the `SillyTavern.getContext()` object. This will check if the current API supports function tool calling and if it's enabled in the settings. Here is an example of how to check if the feature is supported:

```ts
if (SillyTavern.getContext().isToolCallingSupported()) {
    console.log("Function tool calling is supported");
} else {
    console.log("Function tool calling is not supported");
}
```

### Register a function

To register a function tool, you need to call the `registerFunctionTool` function from the `SillyTavern.getContext()` object and pass the required parameters. Here is an example of how to register a function tool:

```ts
SillyTavern.getContext().registerFunctionTool({
    // Internal name of the function tool. Must be unique.
    name: "myFunction",
    // Display name of the function tool. Will be shown in the UI. (Optional)
    displayName: "My Function",
    // Description of the function tool. Must describe what the function does and when to use it.
    description: "My function description. Use when you need to do something.",
    // JSON schema for the parameters of the function tool. See: https://json-schema.org/
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            param1: {
                type: 'string',
                description: 'Parameter 1 description',
            },
            param2: {
                type: 'string',
                description: 'Parameter 2 description',
            },
        },
        required: [
            'param1', 'param2',
        ],
    },
    // Function to call when the tool is triggered. Can be async.
    // If the result is not a string, it will be JSON-stringified.
    action: async ({ param1, param2 }) => {
        // Your function code here
        console.log(`Function called with parameters: ${param1}, ${param2}`);
        return "Function result";
    },
    // Optional function to format the toast message displayed when the function is invoked.
    // If an empty string is returned, no toast message will be displayed.
    formatMessage: ({ param1, param2 }) => {
        return `Function is called with: ${param1} and ${param2}`;
    },
    // Optional function that returns a boolean value indicating whether the tool should be registered for the current prompt.
    // If no shouldRegister function is provided, the tool will be registered for every prompt.
    shouldRegister: () => {
        return true;
    },
    // Optional flag. If set to true, the function call will be performed, but the result won't be recorded to the visible chat history.
    stealth: false,
});
```

### Unregister a function

To deactivate a function tool, you need to call the `unregisterFunctionTool` function from the `SillyTavern.getContext()` object and pass the name of the function tool to disable. Here is an example of how to unregister a function tool:

```ts
SillyTavern.getContext().unregisterFunctionTool("myFunction");
```

## Tips and tricks

1. Successful tool calls are saved as a part of the visible history and will be displayed in the chat UI, so you can inspect the actual parameters and results. If that is not desirable, set the `stealth: true` flag when registering a function tool.
2. If you don't want to see the tool call in the chat history. If you want to stylize or hide them with custom CSS, target a `toolCall` class on `.mes` elements, i.e. `.mes.toolCall { display: none; }` or `.mes.toolCall { color: #999; }`.
