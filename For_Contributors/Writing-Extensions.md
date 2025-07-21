---
order: -20
icon: file-added
---

# UI Extensions

UI extensions expand the functionality of SillyTavern by hooking into its events and API. You can easily create your own extensions.

## I'm not a dev, I just want to install an extension

Go here: [Extensions](../extensions/index.md).

## Extension submissions

Want to contribute your extensions to the [official content repository](https://github.com/SillyTavern/SillyTavern-Content)? Contact us!

To ensure that all extensions are safe and easy to use, we have a few requirements:

1. Your extension must be open-source and have a libre license (see [Choose a License](https://choosealicense.com/licenses/)). If you are unsure, AGPLv3 is a good choice.
2. Extensions must be compatible with the latest release version of SillyTavern. Please be ready to update your extension if something in the core changes.
3. Extensions must be well-documented. This includes a README file with installation instructions, usage examples, and a list of features.
4. Extensions that have a server plugin requirement to function will not be accepted.

## Examples

See live examples of simple SillyTavern extensions:

* <https://github.com/city-unit/st-extension-example> - basic extension template. Showcases manifest creation, local script imports, adding a settings UI panel, and persistent extension settings usage.
* <https://github.com/search?q=topic%3Aextension+org%3ASillyTavern&type=Repositories> - a list of all official SillyTavern extensions on GitHub.

## Bundling

Extensions can also utilize bundling to isolate themselves from the rest of the modules and use any dependencies from NPM, including UI frameworks like Vue, React, etc.

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - template repository of an extension using TypeScript and Webpack (no React).
* <https://github.com/SillyTavern/Extension-ReactTemplate> - template repository of a bare-bones extension using React and Webpack.

To use relative imports from the bundle, you may need to create an import wrapper. Here's an example for Webpack:

```js
// define
async function importFromScript(what) {
    const module = await import(/* webpackIgnore: true */'../../../../../script.js');
    return module[what];
}

// use
const generateRaw = await importFromScript('generateRaw');
```

## manifest.json

Every extension must have a folder in `data/<user-handle>/extensions` and a `manifest.json` file, which contains metadata about the extension and a path to a JS script file that is the entry point of the extension.

Downloadable extensions are mounted into the `/scripts/extensions/third-party` folder when serving over HTTP, so relative imports should be used based on that. To ease local development, consider placing your extension repository in the `/scripts/extensions/third-party` folder (the "Install for all users" option).

```json
{
    "display_name": "The name of the extension",
    "loading_order": 1,
    "requires": [],
    "optional": [],
    "dependencies": [],
    "js": "index.js",
    "css": "style.css",
    "author": "Your name",
    "version": "1.0.0",
    "homePage": "https://github.com/your/extension",
    "auto_update": true,
    "i18n": {
        "de-de": "i18n/de-de.json"
    }
}
```

### Manifest fields

* `display_name` is required. It is displayed in the "Manage Extensions" menu.
* `loading_order` is optional. A higher number loads later.
* `js` is the main JS file reference and is required.
* `css` is an optional style file reference.
* `author` is required. It should contain the name or contact info of the author(s).
* `auto_update` is set to `true` if the extension should auto-update when the version of the ST package changes.
* `i18n` is an optional object that specifies the supported locales and their corresponding JSON files (see below).
* `dependencies` is an optional array of strings specifying other **extensions** that this extension depends on.
* `generate_interceptor` is an optional string that specifies the name of a global function called on text generation requests.

### Dependencies

Extensions can also depend on other SillyTavern extensions. The extension will not load if any of these dependencies are missing or disabled.

Dependencies are specified by their **folder name** as it appears in the `public/extensions` directory.

Examples:

* Built-in extensions: `"vectors"`, `"caption"`
* Third-party extensions: `"third-party/Extension-WebLLM"`, `"third-party/Extension-Mermaid"`

### Deprecated fields

* `requires` is an optional array of strings specifying required **Extras modules**. The extension won't be loaded if the connected Extras API doesn't provide all listed modules.
* `optional` is an optional array of strings specifying optional **Extras modules**. The extension will still load if these are missing, and the extension should handle their absence gracefully.

To check which modules are currently provided by the connected Extras API, import the `modules` array from `scripts/extensions.js`.

## Scripting

### Using getContext

The `getContext()` function in the `SillyTavern` global object gives you access to the SillyTavern context, which is a collection of all the main app state objects, useful functions, and utilities.

```js
const context = SillyTavern.getContext();
context.chat; // Chat log - MUTABLE
context.characters; // Character list
context.characterId; // Index of the current character
context.groups; // Group list
context.groupId; // ID of the current group
// And many more...
```

You can find the full list of available properties and functions in the [SillyTavern source code](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/st-context.js).

!!!
If you're missing any of the functions/properties in `getContext`, please get in touch with the developers or send us a pull request!
!!!

### Importing from other files

!!!warning
Using imports from SillyTavern code is unreliable and can break at any time if the internal structure of ST's modules changes. `getContext` provides a more stable API.
!!!

Unless you're building a bundled extension, you can import variables and functions from other JS files.

For example, this code snippet will generate a reply from the currently selected API in the background:

```js
import { generateQuietPrompt } from "../../../../script.js";

async function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt(text);
    // ...
}
```

## State management

### Persistent settings

When an extension needs to persist its state, it can use the `extensionSettings` object from the `getContext()` function to store and retrieve data. An extension can store any JSON-serializable data in the settings object and must use a unique key to avoid conflicts with other extensions.

To persist the settings, use the `saveSettingsDebounced()` function, which will save the settings to the server.

```js
const { extensionSettings, saveSettingsDebounced } = SillyTavern.getContext();

// Define a unique identifier for your extension
const MODULE_NAME = 'my_extension';

// Define default settings
const defaultSettings = {
    enabled: false,
    option1: 'default',
    option2: 5
};

// Define a function to get or initialize settings
function getSettings() {
    // Initialize settings if they don't exist
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // Ensure all default keys exist (helpful after updates)
    for (const key in defaultSettings) {
        if (extensionSettings[MODULE_NAME][key] === undefined) {
            extensionSettings[MODULE_NAME][key] = defaultSettings[key];
        }
    }

    return extensionSettings[MODULE_NAME];
}

// Use the settings
const settings = getSettings();
settings.option1 = 'new value';

// Save the settings
saveSettingsDebounced();
```

### Chat metadata

To bind some data to a specific chat, you can use the `chatMetadata` object from the `getContext()` function. This object allows you to store arbitrary data associated with a chat, which can be useful for storing extension-specific state.

To persist the metadata, use the `saveMetadata()` function, which will save the metadata to the server.

!!!warning
Do not save the reference to `chatMetadata` in a long-lived variable, as the reference will change when the chat is switched. Always use `SillyTavern.getContext().chatMetadata` to access the current chat metadata.
!!!

```js
const { chatMetadata, saveMetadata } = SillyTavern.getContext();

// Set some metadata for the current chat
chatMetadata['my_key'] = 'my_value';

// Get the metadata for the current chat
const value = chatMetadata['my_key'];

// Save the metadata to the server
await saveMetadata();
```

!!!tip
The `CHAT_CHANGED` event is emitted when the chat is switched, so you can listen to this event to update your extension's state accordingly. See more in the [Listening to event types](#listening-to-event-types) section.
!!!

## Character cards

SillyTavern fully supports [Character Cards V2 Specification](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md), which allows to store arbitrary data in the character card JSON data.

This is useful for extensions that need to store additional data associated with the character and make it shareable when exporting the character card.

To write data to the character card [extensions](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md#extensions) data field, use the `writeExtensionField` function from the `getContext()` function. This function takes a character ID, a string key, and a value to write. The value must be JSON-serializable.

!!!warning Weirdness Ahead
Despite being called `characterId`, it's not a "real" unique identifier but rather an index of the character in the `characters` array.

The index of the current character is provided by the `characterId` property in the context. If you want to write data to the currently selected character, use `SillyTavern.getContext().characterId`. If you need to store data for another character, find the index by searching for the character in the `characters` array.

**Caution: `characterId` is `undefined` in group chats or when no character is selected!**
!!!

```js
const { writeExtensionField, characterId } = SillyTavern.getContext();

// Write some data to the character card
await writeExtensionField(characterId, 'my_extension_key', {
    someData: 'value',
    anotherData: 42
});

// Read the data back from the character card
const character = SillyTavern.getContext().characters[characterId];
// The data is stored in the `extensions` object of the character's data
const myData = character.data?.extensions?.my_extension_key;
```

## Settings presets

Arbitrary JSON data can be stored in the settings presets of the main API types. It will be exported and imported along with the preset JSON, so you can use it to store extension-specific settings for the preset. The following API types support data extensions in settings presets:

* Chat Completion
* Text Completion
* NovelAI
* KoboldAI / AI Horde

To read or write the data, you first need to get the PresetManager instance from the context:

```js
const { getPresetManager } = SillyTavern.getContext();

// Get the preset manager for the current API type
const pm = getPresetManager();

// Write data to the preset extension field:
// - path: the path to the field in the preset data
// - value: the value to write
// - name (optional): the name of the preset to write to, defaults to the currently selected preset
await pm.writePresetExtensionField({ path: 'hello', value: 'world' });

// Read data from the preset extension field:
// - path: the path to the field in the preset data
// - name (optional): the name of the preset to read from, defaults to the currently selected preset
const value = pm.readPresetExtensionField({ path: 'hello' });
```

!!!tip
The `PRESET_CHANGED` and `MAIN_API_CHANGED` events are emitted when the preset is changed or the main API is switched, so you can listen to these events to update your extension's state accordingly. See more in the [Listening to event types](#listening-to-event-types) section.
!!!

## Internationalization

!!!
For general information on providing translations, see the [Internationalization](/For_Contributors/i18n.md) page.
!!!

Extensions can provide additional localized strings for use with the `t`, `translate` functions and the `data-i18n` attribute in HTML templates.

See the list of supported locales here (`lang` key): <https://github.com/SillyTavern/SillyTavern/blob/release/public/locales/lang.json>

### Direct `addLocaleData` call

Pass a locale code and an object with the translations to the `addLocaleData` function. Overrides of existing keys are *NOT* allowed. If the passed locale code is not a currently chosen locale, the data will be silently ignored.

```js
SillyTavern.getContext().addLocaleData('fr-fr', { 'Hello': 'Bonjour' });
SillyTavern.getContext().addLocaleData('de-de', { 'Hello': 'Hallo' });
```

### Via the extension manifest

Add an i18n object with a list of supported locales and their corresponding JSON file paths (relative to your extension's directory) to the manifest.

```json
{
  "display_name": "Foobar",
  "js": "index.js",
  // rest of the fields
  "i18n": {
    "fr-fr": "i18n/french.json",
    "de-de": "i18n/german.json"
  }
}
```

## Registering slash commands (new way)

While `registerSlashCommand` still exists for backward compatibility, new slash commands should now be registered through `SlashCommandParser.addCommandObject()` to provide extended details about the command and its parameters to the parser (and, in turn, to autocomplete and the command help).

```javascript
SlashCommandParser.addCommandObject(SlashCommand.fromProps({ name: 'repeat',
    callback: (namedArgs, unnamedArgs) => {
        return Array(namedArgs.times ?? 5)
            .fill(unnamedArgs.toString())
            .join(isTrueBoolean(namedArgs.space.toString()) ? ' ' : '')
        ;
    },
    aliases: ['example-command'],
    returns: 'the repeated text',
    namedArgumentList: [
        SlashCommandNamedArgument.fromProps({ name: 'times',
            description: 'number of times to repeat the text',
            typeList: ARGUMENT_TYPE.NUMBER,
            defaultValue: '5',
        }),
        SlashCommandNamedArgument.fromProps({ name: 'space',
            description: 'whether to separate the texts with a space',
            typeList: ARGUMENT_TYPE.BOOLEAN,
            defaultValue: 'off',
            enumList: ['on', 'off'],
        }),
    ],
    unnamedArgumentList: [
        SlashCommandArgument.fromProps({ description: 'the text to repeat',
            typeList: ARGUMENT_TYPE.STRING,
            isRequired: true,
        }),
    ],
    helpString: `
        <div>
            Repeats the provided text a number of times.
        </div>
        <div>
            <strong>Example:</strong>
            <ul>
                <li>
                    <pre><code class="language-stscript">/repeat foo</code></pre>
                    returns "foofoofoofoofoo"
                </li>
                <li>
                    <pre><code class="language-stscript">/repeat times=3 space=on bar</code></pre>
                    returns "bar bar bar"
                </li>
            </ul>
        </div>
    `,
}));
```

All registered commands can be used in [STscript](/For_Contributors/st-script.md) in any possible way.

## Listening to event types

Use `eventSource.on()` to listen for events:

```js
const { eventSource, event_types } = SillyTavern.getContext();

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

The main event types are:

* `APP_READY`: the app is fully loaded and ready to use. It will auto-fire every time a new listener is attached after the app is ready.
* `MESSAGE_RECEIVED`: the LLM message is generated and recorded into the `chat` object but not yet rendered in the UI.
* `MESSAGE_SENT`: the message is sent by the user and recorded into the `chat` object but not yet rendered in the UI.
* `USER_MESSAGE_RENDERED`: the message sent by a user is rendered in the UI.
* `CHARACTER_MESSAGE_RENDERED`: the generated LLM message is rendered in the UI.
* `CHAT_CHANGED`: the chat has been switched (e.g., switched to another character, or another chat was loaded).
* `GENERATION_AFTER_COMMANDS`: the generation is about to start after processing slash commands.
* `GENERATION_STOPPED`: the generation was stopped by the user.
* `GENERATION_ENDED`: the generation has been completed or has errored out.
* `SETTINGS_UPDATED`: the application settings have been updated.

The rest can be found [in the source](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js).

!!!info Event data
The way each event passes its data to the listener is not uniform. Some events don't emit any data; some pass an object or a primitive value. Please refer to the source code where the event is emitted to see what data it passes, or check with the debugger.
!!!

## Prompt Interceptors

Prompt Interceptors provide a way for extensions to perform any activity such as modifying the chat data, adding injections, or aborting the generation before a text generation request is made.

Interceptors from different extensions are run sequentially. The order is determined by the `loading_order` field in their respective `manifest.json` files. Extensions with lower `loading_order` values run earlier. If `loading_order` is not specified, `display_name` is used as a fallback. If neither is specified, the order is undefined.

### Registering an Interceptor

To define a prompt interceptor, add a `generate_interceptor` field to your extension's `manifest.json` file. The value should be the name of a global function that will be called by SillyTavern.

```json
{
    "display_name": "My Interceptor Extension",
    "loading_order": 10, // Affects execution order
    "generate_interceptor": "myCustomInterceptorFunction",
    // ... other manifest properties
}
```

### Interceptor Function

The `generate_interceptor` function is a global function that will be called upon generation requests that are not dry runs. It must be defined in the global scope (e.g., `globalThis.myCustomInterceptorFunction = async function(...) { ... }`) and can return a `Promise` if it needs to perform any asynchronous operations.

The interceptor function receives the following arguments:

* `chat`: An array of message objects representing the chat history that will be used for prompt building. You can modify this array directly (e.g., add, remove, or alter messages). Please note that messages are mutable, so any changes you make to the array will be reflected in the actual chat history. If you want the changes to be ephemeral, use `structuredClone` to create a deep copy of the message object.
* `contextSize`: A number indicating the current context size (in tokens) calculated for the upcoming generation.
* `abort`: A function that, when called, will signal to prevent the text generation from proceeding. It accepts a boolean parameter that prevents any subsequent interceptors from running if `true`.
* `type`: A string indicating the type or trigger of the generation (e.g., `'quiet'`, `'regenerate'`, `'impersonate'`, `'swipe'`, etc.). This helps the interceptor apply logic conditionally based on how the generation was initiated.

**Example Implementation:**

```javascript
globalThis.myCustomInterceptorFunction = async function(chat, contextSize, abort, type) {
    // Example: Add a system note before the last user message
    const systemNote = {
        is_user: false,
        name: "System Note",
        send_date: Date.now(),
        mes: "This was added by my extension!"
    };
    // Insert before the last message
    chat.splice(chat.length - 1, 0, systemNote);
}
```

## Generating text

SillyTavern provides several functions to generate text in different contexts using the currently chosen LLM API. These functions allow you to generate text in the context of a chat, raw generation without any context, or with structured outputs.

### Within a chat context

The `generateQuietPrompt()` function is used to generate text in the context of a chat with an added "quiet" prompt (post-history instruction) in the background (the output is not rendered in the UI). This is useful for generating text without interrupting the user experience while also keeping the relevant chat and character data intact, such as generating a summary or an image prompt.

```js
const { generateQuietPrompt } = SillyTavern.getContext();

const prompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    prompt,
});
```

### Raw generation

The `generateRaw()` function is used to generate text without any chat context. It is useful when you want to fully control the prompt-building process.

It accepts a `prompt` as a Text Completion string or an array of Chat Completion objects, constructing the request with an appropriate format depending on the selected API type, e.g., converting between chat/text modes, applying instruct formatting, etc. You can also pass an additional `systemPrompt` and a `prefill` to the function for even more control over the generation process.

```js
const { generateRaw } = SillyTavern.getContext();

const systemPrompt = 'You are a helpful assistant.';
const prompt = 'Generate a story about a brave knight.';
const prefill = 'Once upon a time,';

/*
In Chat Completion mode, will produce a prompt like this:
[
  {role: 'system', content: 'You are a helpful assistant.'},
  {role: 'user', content: 'Generate a story about a brave knight.'},
  {role: 'assistant', content: 'Once upon a time,'}
]
*/

/*
In Text Completion mode (no instruct), will produce a prompt like this:
"You are a helpful assistant.\nGenerate a story about a brave knight.\nOnce upon a time,"
*/

const result = await generateRaw({
    systemPrompt,
    prompt,
    prefill,
});
```

### Structured Outputs

!!!info
Currently only supported by the Chat Completion API. The availability varies based on the selected source and model. If the selected model does not support structured outputs, the generation will either fail or will return an empty object (`'{}'`). Check the documentation for the specific API you are using to see if structured outputs are supported.
!!!

You can use the structured outputs feature to ensure the model produces a valid JSON object that adheres to a provided [JSON Schema](https://json-schema.org/learn). This is useful for extensions that require structured data, such as state tracking, data classification, etc.

To use structured outputs, you must pass a JSON schema object to `generateRaw()` or `generateQuietPrompt()`. The model will then generate a response that matches the schema, and it will be returned as a stringified JSON object.

!!!warning
The outputs are not validated against the schema, you must handle the parsing and validation of the generated output yourself. If the model fails to generate a valid JSON object, the function will return an empty object (`'{}'`).

[Zod](https://zod.dev/json-schema) is a popular library to generate and validate JSON schemas. Its use will not be covered here.
!!!

```js
const { generateRaw, generateQuietPrompt } = SillyTavern.getContext();

// Define a JSON schema for the expected output
const jsonSchema = {
    // Required: a name for the schema
    name: 'StoryStateModel',
    // Optional: a description of the schema
    description: 'A schema for a story state with location, plans, and memories.',
    // Optional:  the schema will be used in strict mode, meaning that only the fields defined in the schema will be allowed
    strict: true,
    // Required: a definition of the schema
    value: {
        '$schema': 'http://json-schema.org/draft-04/schema#',
        'type': 'object',
        'properties': {
            'location': {
                'type': 'string'
            },
            'plans': {
                'type': 'string'
            },
            'memories': {
                'type': 'string'
            }
        },
        'required': [
            'location',
            'plans',
            'memories'
        ],
    },
};

const prompt = 'Generate a story state with location, plans, and memories. Output as a JSON object.';

const rawResult = await generateRaw({
    prompt,
    jsonSchema,
});

const quietResult = await generateQuietPrompt({
    prompt,
    jsonSchema,
});
```

## Do Extras request

!!!warning
The Extras API is deprecated. It's not recommended to use it in new extensions.
!!!

The `doExtrasFetch()` function allows you to make requests to your SillyTavern Extras API server.

For example, to call the `/api/summarize` endpoint:

```js
import { getApiUrl, doExtrasFetch } from "../../extensions.js";

const url = new URL(getApiUrl());
url.pathname = '/api/summarize';

const apiResult = await doExtrasFetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Bypass-Tunnel-Reminder': 'bypass',
    },
    body: JSON.stringify({
        // Request body
    })
});
```

`getApiUrl()` returns the base URL of the Extras server.

The `doExtrasFetch()` function:

* Adds the `Authorization` and `Bypass-Tunnel-Reminder` headers
* Handles fetching the result
* Returns the result (the response object)

This makes it easy to call your Extras API from your extension.

You can specify:

* The request method: GET, POST, etc.
* Additional headers
* The body for POST requests
* Any other fetch options
