---
order: -20
icon: file-added
templating: false
route: /for-contributors/writing-extensions/
---

# UI Extensions

UI extensions expand SillyTavern's functionality by hooking into its events and API. They run in a browser context and have practically unrestricted access to the DOM, JavaScript APIs, and the SillyTavern context. Extensions can modify the UI, call internal APIs, and interact with chat data. This guide explains how to create your own extensions (JavaScript knowledge is required).

!!!tip Just want to install extensions?
Go here: [Extensions](../extensions/index.md).
!!!

To extend the functionality of the Node.js server, see the [Server Plugins](./Server-Plugins.md) page.

**Can't write JavaScript?**

* Consider [STscript](./st-script.md) as a simpler alternative to writing a full-fledged extension.
* Go through the [MDN Course](https://developer.mozilla.org/en-US/docs/Learn/JavaScript) and come back when you're done.

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
/**
 * Import a member from a module by URL, bypassing webpack.
 * @param {string} url URL to import from
 * @param {string} what Name of the member to import
 * @param {any} defaultValue Fallback value
 * @returns {Promise<any>} Imported member
 */
export async function importFromUrl(url, what, defaultValue = null) {
    try {
        const module = await import(/* webpackIgnore: true */ url);
        if (!Object.hasOwn(module, what)) {
            throw new Error(`No ${what} in module`);
        }
        return module[what];
    } catch (error) {
        console.error(`Failed to import ${what} from ${url}: ${error}`);
        return defaultValue;
     }
}

// Import a function from 'script.js' module
const generateRaw = await importFromUrl('/script.js', 'generateRaw');
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
    "minimum_client_version": "1.0.0",
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
* `minimum_client_version` is an optional string that specifies the minimum SillyTavern version required for this extension to work.

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

### Shared libraries

Most of the npm libraries used internally by the SillyTavern frontend are shared in the `libs` property of the `SillyTavern` global object.

* `lodash` - Utility library. [Docs](https://lodash.com/).
* `localforage` - Browser storage library. [Docs](https://localforage.github.io/localForage/).
* `Fuse` - Fuzzy search library. [Docs](https://www.fusejs.io/).
* `DOMPurify` - HTML sanitization library. [Docs](https://github.com/cure53/DOMPurify).
* `Handlebars` - Templating library. [Docs](https://handlebarsjs.com/).
* `moment` - Date/time manipulation library. [Docs](http://momentjs.com/).
* `showdown` - Markdown converter library. [Docs](https://showdownjs.com/).

You can find the full list of exported libraries in the [SillyTavern source code](https://github.com/SillyTavern/SillyTavern/blob/staging/public/lib.js).

**Example:** Using the DOMPurify library.

```js
const { DOMPurify } = SillyTavern.libs;

const sanitizedHtml = DOMPurify.sanitize('<script>"dirty HTML"</script>');
```

### TypeScript notice

If you want access to autocomplete for all methods in the `SillyTavern` global object (and you probably do), including `getContext()` and `libs`, you should add a TypeScript `.d.ts` module declaration. This declaration should import global types from SillyTavern's source, depending on your extension's location. Below is an example that works for both installation types: "all users" and "current user."

**global.d.ts** - place this file in the root of your extension directory (next to `manifest.json`):

```ts
export {};

// 1. Import for user-scoped extensions
import '../../../../public/global';
// 2. Import for server-scoped extensions
import '../../../../global';

// Define additional types if needed...
declare global {
    // Add global type declarations here
}
```

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
    const translated = await generateQuietPrompt({ quietPrompt: text });
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
const defaultSettings = Object.freeze({
    enabled: false,
    option1: 'default',
    option2: 5
});

// Define a function to get or initialize settings
function getSettings() {
    // Initialize settings if they don't exist
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // Ensure all default keys exist (helpful after updates)
    for (const key of Object.keys(defaultSettings)) {
        if (!Object.hasOwn(extensionSettings[MODULE_NAME], key)) {
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
The `CHAT_CHANGED` event is emitted when the chat is switched, so you can listen to this event to update your extension's state accordingly. See more in the [Listening to events](#listening-to-events) section.
!!!

### Character cards

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

### Settings presets

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
The `PRESET_CHANGED` and `MAIN_API_CHANGED` events are emitted when the preset is changed or the main API is switched, so you can listen to these events to update your extension's state accordingly. See more in the [Listening to events](#listening-to-events) section.
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

## Events

### Listening to events

Use `eventSource.on(eventType, eventHandler)` to listen for events:

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

### Emitting events

You can produce any application events from extensions, including custom events, by calling `eventSource.emit(eventType, ...eventData)`:

```js
const { eventSource } = SillyTavern.getContext();

// Can be a built-in event_types field or any string.
const eventType = 'myCustomEvent';

// Use `await` to ensure all event handlers complete before continuing execution.
await eventSource.emit(eventType, { data: 'custom event data' });
```

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

const quietPrompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    quietPrompt,
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
    quietPrompt: prompt,
    jsonSchema,
});
```

## Registering custom macros

You can register custom macros that can be used anywhere where macro substitutions are supported, e.g. in the character card fields, STscript commands, prompt templates, etc.

To register a macro, use the `registerMacro()` function from the `SillyTavern.getContext()` object. The function accepts a macro name that should be a unique string, and a string or a function that returns a string. The function will be called with a unique `nonce` string that will be different between each `substituteParams` call.

```js
const { registerMacro } = SillyTavern.getContext();

// Simple string macro
registerMacro('fizz', 'buzz');
// Function macro
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});
```

When a custom macro is no longer needed, remove it using the `unregisterMacro()` function:

```js
const { unregisterMacro } = SillyTavern.getContext();

// Unregister the 'fizz' macro
unregisterMacro('fizz');
```

**Important details and known limitations regarding custom macros:**

1. Currently only simple string replacement macros are supported. We're working on adding support for more complex macros in the future.
2. Macros that use functions to provide a value *must* be synchronous. Returning a `Promise` will not work.
3. You do not need to wrap the macro name in double curly braces (`{{ }}`) when registering it. SillyTavern will do that for you.
4. Since macros are plain regular expression substitutions, registering a lot of macros will cause performance issues, so use them sparingly.

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

## Best Practices

### Security

**Never store API keys or secrets in `extensionSettings`**

Extension settings are accessible to all other extensions and are stored in plain text. Do not store sensitive data client-side:

```js
// BAD - Don't do this!
extensionSettings[MODULE_NAME].apiKey = 'secret_key_123';

// NOTE: There is no secure way to store secrets in client-side extensions.
// If you need to handle sensitive data, use server plugins instead.
// See: https://docs.sillytavern.app/for-contributors/server-plugins/
```

**Sanitize user inputs**

Always validate and sanitize data from user inputs before using it in commands, API calls, or DOM manipulation:

```js
// Validate input type first
if (typeof userInput !== 'string') {
    toastr.error('Invalid input type');
    return;
}
// Use DOMPurify to sanitize HTML input
const { DOMPurify } = SillyTavern.libs;
const cleanInput = DOMPurify.sanitize(userInput);
```

**Avoid using `eval()` or `Function()` constructors**

These can execute arbitrary code and pose security risks. If you need dynamic evaluation, use safer alternatives or restrict the input carefully.

### Performance

**Don't store large data in `extensionSettings`**

Extension settings are loaded into memory and saved frequently. Large data can cause performance issues:

```js
// BAD - Don't store large data
extensionSettings[MODULE_NAME].largeDataset = { /* megabytes of data */ };

// GOOD - Use localforage (abstraction over IndexedDB/localStorage)
const { localforage } = SillyTavern.libs;
await localforage.setItem(`${MODULE_NAME}_data`, largeData);

// Or use localStorage for smaller data
localStorage.setItem(`${MODULE_NAME}_data`, JSON.stringify(smallData));
```

**Clean up event listeners**

Remove event listeners when they're no longer needed to prevent memory leaks:

```js
function cleanup() {
    eventSource.removeListener(event_types.MESSAGE_RECEIVED, handleMessage);
    document.getElementById('myElement').removeEventListener('click', handleClick);
}
```

**Don't block the UI thread**

For heavy operations, use async/await or web workers:

```js
// Use async for I/O operations
async function processData() {
    const result = await fetch('/api/process');
    return result.json();
}

// Break up heavy computations
async function heavyComputation(data) {
    for (let i = 0; i < data.length; i++) {
        // Process chunk
        if (i % 1000 === 0) {
            await new Promise(resolve => setTimeout(resolve, 0)); // Yield to UI
        }
    }
}
```

### Compatibility

**Prefer `getContext()` over direct imports**

The context API is more stable and less likely to break with SillyTavern updates:

```js
// GOOD - Stable API
const { chat, characters, saveSettingsDebounced } = SillyTavern.getContext();

// AVOID - May break with internal changes
import { chat, characters } from '../../../../script.js';
```

**Use unique module names**

Prevent conflicts with other extensions by using a descriptive, unique module name:

```js
// GOOD - Specific and unique
const MODULE_NAME = 'my_extension_name';

// BAD - Too generic, likely to conflict
const MODULE_NAME = 'settings';
```

### User Experience

**Provide clear feedback**

Use toastr notifications to inform users of actions and errors:

```js
const { Popup } = SillyTavern.getContext();

// Success message
toastr.success('Data imported successfully');

// Error message
toastr.error('Failed to connect to API');

// Warning message
toastr.warning('This feature is experimental');

// For important messages, use popups
const result = await Popup.show.confirm('Confirm', 'Are you sure?');
const userInput = await Popup.show.input('Input', 'Please enter a value:', 'default value');
await Popup.show.text('Info', 'Operation completed successfully.');
```

**Provide helpful console messages**

Use a consistent prefix for your console logs. But do not spam the console with excessive logs in production:

```js
const MODULE_NAME = 'MyExtension';

console.log(`[${MODULE_NAME}] Extension loaded`);
console.debug(`[${MODULE_NAME}] Processing data:`, data);
console.error(`[${MODULE_NAME}] Error occurred:`, error);
```

### Code Quality

**Use bundled libraries from `lib.js`**

SillyTavern bundles several useful libraries that extensions can be used directly:

```js
// Utility functions
const { lodash } = SillyTavern.libs;
const uniqueItems = lodash.uniq([1, 2, 2, 3]);
const grouped = lodash.groupBy([{ category: 'A', name: 'foo' }, { category: 'B', name: 'bar' }], 'category');

// Fuzzy search
const { Fuse } = SillyTavern.libs;
const fuse = new Fuse(items, { keys: ['name', 'description'] });
const results = fuse.search('query');

// Template rendering
const { Handlebars } = SillyTavern.libs;
const template = Handlebars.compile('<div>{{name}}</div>');
const html = template({ name: 'Example' });

// Date/time manipulation
const { moment } = SillyTavern.libs;
const formatted = moment().format('YYYY-MM-DD HH:mm:ss');
```

Other available libraries include: `DOMPurify`, `localforage`, `hljs`, `showdown`, `yaml`, and more. Check [lib.js](https://github.com/SillyTavern/SillyTavern/blob/release/public/lib.js) for the full list.

**Initialize settings properly**

Always provide defaults and handle missing keys:

```js
function loadSettings() {
    // Merge with defaults to handle new keys after updates and initialize if it doesn't exist.
    extensionSettings[MODULE_NAME] = SillyTavern.libs.lodash.merge(
        structuredClone(defaultSettings),
        extensionSettings[MODULE_NAME]
    );
}
```
