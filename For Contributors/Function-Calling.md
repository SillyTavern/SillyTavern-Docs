---
order: -1
icon: code-review
---

# Function Calling

Function Calling allows adding dynamic functionality to your extensions by letting the LLM use structured data that you then can use to trigger a specific functionality of the extension.

## Limitations and prerequisites

1. This feature is only available for certain Chat Completion sources: OpenAI, Claude, MistralAI, Cohere, Groq, OpenRouter, and Perplexity.
2. Text Completion APIs don't support function calls, but some of them selectively allow JSON grammar schemas. Their use is outside the scope of this document.
3. For requests that use streaming, the function calling feature is not available. Note: background ('quiet') requests are never streamed, even if streaming is enabled.
4. The support for function calling must be explicitly allowed by the user first. This is done by enabling the "Enable function calling" option in the AI Response Configuration panel.
5. When a function is called, the normal response generation will be suppressed, so you will likely receive an empty response when calling `generateQuietPrompt`.
6. There is no guarantee that an LLM will perform any function calls at all. Always provide a backup plan for when the function call is not executed.
7. To determine if the function calling feature is supported, you can call `isFunctionCallingSupported` exported from the `openai.js` module.

## Register a function

When a Chat Completion request is prepared, an event of type `LLM_FUNCTION_TOOL_REGISTER` is emitted through the `eventSource` object. The registrations are ephemeral and do not persist between requests, so you can always whether or not to skip the registration based on the event data.

The event data consists of the following: type of the generation that triggered the event, generation data (including sampling parameters and assembled chat prompt), and a callback function to register a function tool. To register a function, you MUST call the `registerFunctionTool` function with the following parameters:

1. `name`: Name of the function tool to register. This will be later used as an identifier to call the function.
2. `description`: Description of the function tool. It helps the language model to better understand what the tool is supposed to do.
3. `params`: JSON schema for the parameters of the function tool. A model will try to generate a JSON object that matches this schema for the parameters of the function call.
4. `required`: Whether the function tool should be forced to be used. This is just a hint and does not guarantee that your function will be executed, especially if the API doesn't support simultaneous calls or does not provide a way to force it.

```ts
/**
 * Callback data for the `LLM_FUNCTION_TOOL_REGISTER` event type that is triggered when a function tool can be registered.
 */
interface FunctionToolRegister {
    /**
     * The type of generation that is being used
     */
    type?: string;
    /**
     * Generation data, including messages and sampling parameters
     */
    data: Record<string, object>;
    /**
     * Callback to register an LLM function tool.
     */
    registerFunctionTool: typeof registerFunctionTool;
}

/**
 * Callback data for the `LLM_FUNCTION_TOOL_REGISTER` event type that is triggered when a function tool is registered.
 * @param name Name of the function tool to register
 * @param description Description of the function tool
 * @param params JSON schema for the parameters of the function tool
 * @param required Whether the function tool should be forced to be used
 */
declare function registerFunctionTool(name: string, description: string, params: object, required: boolean): Promise<void>;
```

## Handle a successful call

When a function call is detected in the output of the LLM, an event of type `LLM_FUNCTION_TOOL_CALL` is emitted through the `eventSource` object. The event data consists of the following: name of the function tool that was called and arguments that were passed to the function in a JSON-serialized string form.

**Note:** Always check that arguments can be parsed correctly and perform basic validation before using them! Sometimes models do not generate correct JSON.

```ts
/**
 * Callback data for the `LLM_FUNCTION_TOOL_CALL` event type that is triggered when a function tool is called.
 */
interface FunctionToolCall {
    /**
     * Name of the function tool to call
     */
    name: string;
    /**
     * JSON object with the parameters to pass to the function tool
     */
    arguments: string;
}
```

## Example code

Example of the function registration and usage:

```js
// Not supported? Alas, resort to other methods.
if (!isFunctionCallingSupported()) {
    return;
}

// Setup a callback for the `LLM_FUNCTION_TOOL_REGISTER` event.
// Use `on` to always listen to the event, or `once` to listen only once.
eventSource.on(event_types.LLM_FUNCTION_TOOL_REGISTER, (args) => {
    // Only trigger on quiet mode
    if (args.type !== 'quiet') {
        return;
    }

    // Register the function tool
    args.registerFunctionTool(
        // name
        'getWeather',
        // description
        'Get the weather for a specific location',
        // params
        {
            $schema: 'http://json-schema.org/draft-04/schema#',
            type: 'object',
            properties: {
                location: {
                    type: 'string',
                    description: 'The location to get the weather for',
                },
            },
            required: [
                'location',
            ],
        },
        // required
        true,
    );
});

// Setup a callback for the `LLM_FUNCTION_TOOL_CALL` event.
// Respond to a function call if it matches the registered function tool.
eventSource.on(event_types.LLM_FUNCTION_TOOL_CALL, (args) => {
    if (args.name !== 'getWeather') {
        return;
    }

    try {
        // Expecting JSON data like: { "location": "Sacramento" }
        const data = JSON.parse(args?.arguments);

        // No actual data to work with
        if (!data.location) {
            return;
        }

        // Do something with the data
        getWeather(data.location);
    } catch {
        // Model did not generate correct JSON
    }
});

// Make a quiet prompt generation request. This will trigger the `LLM_FUNCTION_TOOL_REGISTER` event.
// Imagine that the prompt is coming from a user input.
const response = await generateQuietPrompt('What is the weather in Sacramento?', false, false);
// Response may contain a plain text response in case no tools were called. Always check for this.

if (response) {
    // Do something with the response
}
```
