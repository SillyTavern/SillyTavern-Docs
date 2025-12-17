---
order: -20
icon: file-added
---

# UI Extensions

UI extensions expand the functionality of SillyTavern by hooking into its events and API. You can easily create your own extensions.

Want to contribute your extensions to the [official repository](https://github.com/SillyTavern/SillyTavern-Content)? Contact us!

## Examples

See live examples of simple SillyTavern extensions:

* <https://github.com/city-unit/st-extension-example> - basic extension template. Showcases manifest creation, local script imports, adding a settings UI panel, and persistent extension settings usage.

## Bundling

Extensions can also utilize bundling to isolate themselves from the rest of the modules and use any dependencies from NPM, including UI frameworks like Vue, React, etc.

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - tempate repoistory of an extension using TypeScript and Webpack (no React).
* <https://github.com/SillyTavern/Extension-ReactTemplate> - template repository of a barebone extension using React and Webpack.

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

Every extension must have a folder in `data/<user-handle>/extensions` and have a manifest.json file which contains metadata about the extension and a path to a JS script file, which is the entry point of the extension.

```json
{
    "display_name": "The name of the extension",
    "loading_order": 1,
    "requires": [],
    "optional": [],
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

* `display_name` is required. Displays in the "Manage Extensions" menu.
* `loading_order` is optional. Higher number loads later.
* `requires` specifies the required Extras modules dependencies. An extension won't be loaded unless the connected Extras API provides all of them.
* `optional` specifies the optional Extras dependencies.
* `js` is the main JS file reference, and is required.
* `css` is an optional style file reference.
* `author` is required. It should contain the name or contact info of the author(s).
* `auto_update` is set to true if the extension should auto-update when the version of the ST package changes.
* `i18n` is an optional object that specifies the supported locales and their corresponding JSON files (see below).

Downloadable extensions are mounted into the `/scripts/extensions/third-party` folder, so relative imports should be used based on that. Be careful about where you create your extension during development if you plan on installing it from your GitHub which overwrites the content in the `third-party` folder.

### `requires` vs `optional`

* `requires` - extension could be installed, but will not be loaded until the user connects to the Extras API that provides all of the specified modules.
* `optional` - extension could be installed and will always be loaded, but any of the missing Extras API modules will be highlighted in the "Manage extensions" menu.

To check which modules are currently provided by the connected Extras API, import the `modules` array from `scripts/extensions.js`.

## Scripting

### Using getContext

The `getContext()` function in a SillyTavern global object gives you access to the SillyTavern context, which is a collection of all the main app state objects, useful functions and utilities.

```js
const context = SillyTavern.getContext();
context.chat; // Chat log - MUTABLE
context.characters; // Character list
context.characterId; // Index of the current character
context.groups; // Group list
// And many more...
```

!!!
If you're missing any of the functions/properties in `getContext`, please get in touch with the developers or send us a Pull Request!
!!!

### Importing from other files

!!! warning
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

When the extension needs to persist its state, it can use `extensionSettings` object from the `getContext()` function to store and retrieve data. An extension can store any JSON-serializable data in the settings object and must use a unique key to avoid conflicts with other extensions.

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
    if (!extension_settings[MODULE_NAME]) {
        extension_settings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // Ensure all default keys exist (helpful after updates)
    for (const key in defaultSettings) {
        if (extension_settings[MODULE_NAME][key] === undefined) {
            extension_settings[MODULE_NAME][key] = defaultSettings[key];
        }
    }

    return extension_settings[MODULE_NAME];
}

// Use the settings
const settings = getSettings();
settings.option1 = 'new value';

// Save the settings
saveSettingsDebounced();
```

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

While `registerSlashCommand` still exists for backward compatibility, new slash commands should now be registered through `SlashCommandParser.addCommandObject()` to provide extended details about the command and its parameters to the parser (and in turn to autocomplete and the command help).

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

### Listening to event types

Use eventSource.on() to listen for events:

```js
import { eventSource, event_types } from "../../../../script.js";

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

The main event types are:

* `MESSAGE_RECEIVED`
* `MESSAGE_SENT`
* `CHAT_CHANGED`

The rest can be found [here](https://github.com/SillyTavern/SillyTavern/blob/44343e8ec954c766d868826a7bed01a1f1e1ffbe/public/script.js#L436).

### Do Extras request

!!! warning
Extras API is deprecated. It's not recommended to use it in new extensions.
!!!

The `doExtrasFetch()` function allows you to make requests to your SillyTavern Extra server.

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

`getApiUrl()` returns the base URL of the Extras serve.

The doExtrasFetch() function:

* Adds the Authorization and Bypass-Tunnel-Reminder headers
* Handles fetching the result
* Returns the result (the response object)

This makes it easy to call your Extra's API from your plugin.

You can specify:

* The request method: GET, POST, etc.
* Additional headers
* The body for POST requests
* And any other fetch options

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
// Use DOMPurify to sanitize HTML input
import { DOMPurify } from '../../../../lib.js';
const cleanInput = DOMPurify.sanitize(userInput);

// Validate input types
if (typeof userInput !== 'string') {
    toastr.error('Invalid input type');
    return;
}
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
import { localforage } from '../../../../lib.js';
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
const { callGenericPopup, Popup, POPUP_TYPE } = SillyTavern.getContext();

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

**Prefer vanilla JavaScript over jQuery**

While SillyTavern still uses jQuery internally, new extensions should use modern vanilla JavaScript and DOM APIs:

```js
// GOOD - Modern vanilla JavaScript
const element = document.getElementById('myElement');
element.classList.add('active');
element.addEventListener('click', handleClick);

// AVOID - jQuery (unless necessary for compatibility)
$('#myElement').addClass('active').on('click', handleClick);

// GOOD - Query selectors
const elements = document.querySelectorAll('.my-class');
elements.forEach(el => el.style.display = 'none');

// AVOID - jQuery
$('.my-class').hide();
```

Vanilla JavaScript is more future-proof, has better performance, and reduces dependencies.

**Use bundled libraries from `lib.js`**

SillyTavern bundles several useful libraries that extensions can import directly:

```js
// Utility functions
import { lodash } from '../../../../lib.js';
const uniqueItems = lodash.uniq(array);
const grouped = lodash.groupBy(items, 'category');

// Fuzzy search
import { Fuse } from '../../../../lib.js';
const fuse = new Fuse(items, { keys: ['name', 'description'] });
const results = fuse.search('query');

// Template rendering
import { Handlebars } from '../../../../lib.js';
const template = Handlebars.compile('<div>{{name}}</div>');
const html = template({ name: 'Example' });

// Date/time manipulation
import { moment } from '../../../../lib.js';
const formatted = moment().format('YYYY-MM-DD HH:mm:ss');
```

Other available libraries include: `DOMPurify`, `localforage`, `hljs`, `showdown`, `yaml`, and more. Check [lib.js](https://github.com/SillyTavern/SillyTavern/blob/release/public/lib.js) for the full list.

**Initialize settings properly**

Always provide defaults and handle missing keys:

```js
function loadSettings() {
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // Merge with defaults to handle new keys after updates
    extensionSettings[MODULE_NAME] = Object.assign(
        structuredClone(defaultSettings),
        extensionSettings[MODULE_NAME]
    );
}
```
