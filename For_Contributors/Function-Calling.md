---
order: -10
icon: code-review
route: /for-contributors/function-calling/
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
4. [Weather](https://github.com/SillyTavern/Extension-Weather) - fetch the weather information from weather APIs.
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - roll dice for D&D games.

## Prerequisites and limitations

1. This feature is only available for certain Chat Completion sources: OpenAI, Claude, MistralAI, Groq, Cohere, OpenRouter, AI21, Google AI Studio, Google Vertex AI, DeepSeek, AI/ML API, NanoGPT and Custom API sources.
2. Text Completion APIs don't support function calls, but some locally-hosted backends like Ollama and TabbyAPI may run in Custom OpenAI-compatible mode under Chat Completion.
3. The support for function calling must be explicitly allowed by the user first. This is done by enabling the "Enable function calling" option in the AI Response Configuration panel.
4. There is no guarantee that an LLM will perform any function calls at all. Most of them require an explicit "activation" through the prompt (e.g., the user asking to "Roll a dice", "Get the weather", etc.).
5. Not all prompts can trigger a tool call. Continuations, impersonation, background ('quiet') prompts are not allowed to trigger a tool call. They can still use past successful tool calls in their responses.

## How to make a function tool

### Check if the feature is supported

Use `isToolCallingSupported()` from `SillyTavern.getContext()` to check if the current API supports function tool calling and if it's enabled in the settings:

```js
const { isToolCallingSupported } = SillyTavern.getContext();

if (isToolCallingSupported()) {
    console.log('Function tool calling is supported');
}
```

You can also check whether tool calls can be performed for a specific generation type. Continuations, impersonation, and background ('quiet') prompts are not allowed to trigger tool calls:

```js
const { canPerformToolCalls } = SillyTavern.getContext();

if (canPerformToolCalls('normal')) {
    console.log('Can perform tool calls for this generation');
}
```

### Register a function tool

Use `registerFunctionTool()` from `SillyTavern.getContext()` to register a tool. The tool definition follows the [JSON Schema](https://json-schema.org/) format for its parameters:

```js
const { registerFunctionTool } = SillyTavern.getContext();

registerFunctionTool({
    name: 'get_weather',
    displayName: 'Get Weather',
    description: 'Get the current weather for a given location',
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            location: {
                type: 'string',
                description: 'The city name, e.g. "London"',
            },
            unit: {
                type: 'string',
                enum: ['celsius', 'fahrenheit'],
                description: 'Temperature unit',
            },
        },
        required: ['location'],
    },
    action: async ({ location, unit }) => {
        // Perform your logic here (API calls, computations, etc.)
        const data = await fetchWeatherData(location, unit);
        return JSON.stringify(data);
    },
    formatMessage: ({ location }) => `Checking weather for ${location}...`,
    shouldRegister: () => isWeatherFeatureEnabled(),
    stealth: false,
});
```

### Registration fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Unique identifier for the tool |
| `displayName` | No | User-friendly display name shown in the UI |
| `description` | Yes | Description sent to the LLM to explain what the tool does and when to use it |
| `parameters` | Yes | JSON Schema defining the tool's input parameters |
| `action` | Yes | Function called when the LLM invokes the tool. Receives parsed parameters as an object. Can be async. Must return a string result (non-string values will be JSON-stringified). |
| `formatMessage` | No | Function returning a string shown as a toast while the tool executes. Return an empty string to suppress the toast. |
| `shouldRegister` | No | Function returning a boolean; if `false`, the tool is excluded from the current request. If not provided, the tool is registered for every prompt. |
| `stealth` | No | If `true`, the tool call result is not recorded to visible chat history and no follow-up generation is triggered |

### Unregister a function tool

To deactivate a function tool, call `unregisterFunctionTool()` with the tool's name:

```js
const { unregisterFunctionTool } = SillyTavern.getContext();

unregisterFunctionTool('get_weather');
```

## Tips and tricks

1. Successful tool calls are saved as part of the visible chat history and displayed in the chat UI, so you can inspect the actual parameters and results. If that is not desirable, set `stealth: true` when registering the tool.
2. To stylize or hide tool call messages with custom CSS, target the `toolCall` class on `.mes` elements, e.g. `.mes.toolCall { display: none; }` or `.mes.toolCall { opacity: 0.5; }`.
3. Write clear, specific descriptions for your tools — the LLM uses these to decide when and how to call them. Include guidance on when the tool should be used.
4. Keep the parameter schema simple and well-documented. Each property's `description` helps the LLM fill in the correct values.
5. Tool calls have a recursion limit (default: 5 rounds). If the LLM keeps calling tools repeatedly, execution will stop after the limit is reached.
