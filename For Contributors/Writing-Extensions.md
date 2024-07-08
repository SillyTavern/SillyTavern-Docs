---
icon: file-added
---

# UI Extensions

UI extensions expand the functionality of SillyTavern by hooking into its events and API. You can easily create your own extensions.

Want to contribute your extensions to the [official repository](https://github.com/SillyTavern/SillyTavern-Content)? Contact us!

## Examples

See live examples of simple SillyTavern extensions:

* https://github.com/city-unit/st-extension-example - basic extension template. Showcases manifest creation, local script imports, adding a settings UI panel, and persistent extension settings usage.

## Bundling

Extensions can also utilize bundling to isolate themselves from the rest of the modules and use any dependencies from NPM, including UI frameworks like Vue, React, etc.

* https://github.com/SillyTavern/Extension-ReactTemplate - template repository of a barebone extension using React and Webpack.

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

Every extension must have a folder in `data/<user-handle>/extensions` and have a manifest.json file which contains metadata about the plugin and a JS script file.


```js
{
    "display_name": "The name of the plugin",
    "loading_order": 1,
    "requires": [],
    "optional": [],
    "js": "index.js",
    "css": "style.css",
    "author": "Your name",
    "version": "1.0.0",
    "homePage": "https://github.com/your/plugin",
    "auto_update": true
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

Downloadable extensions are mounted into the `/scripts/extensions/third-party` folder, so relative imports should be used based on that. Be careful about where you create your extension during development if you plan on installing it from your GitHub which overwrites the content in the `third-party` folder.

#### `requires` vs `optional`

* `requires` - extension could be installed, but will not be loaded until the user connects to the Extras API that provides all of the specified modules.
* `optional` - extension could be installed and will always be loaded, but any of the missing Extras API modules will be highlighted in the "Manage extensions" menu.

To check which modules are currently provided by the connected Extras API, import the `modules` array from `scripts/extensions.js`.

### Using getContext

The getContext() function gives you access to the SillyTavern context:

```js
import { getContext } from "../../extensions.js";

const context = getContext();
context.chat; // Chat log
context.characters; // Character list
context.groups; // Group list
// And many more...
```

Use this to interact with the main app state.

You can also use a globally defined window object: `SillyTavern.getContext()`.

### Importing from other files

Unless you're building a bundled extension, you can import variables and functions from other JS files.

For example, this code snipped will generate a reply from the currently selected API in the background:

```js
import { generateQuietPrompt } from "../../../script.js";

function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt(text);
    // ...
}
```

### Important note

Using imports from SillyTavern code is unreliable and can break at any time if the internal structure of ST's modules changes. `getContext` provides a more stable API.

If you're missing any of the functions/properties in `getContext`, please get in touch with the developers or send us a Pull Request!

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

## (DEPRECATED) Registering slash commands (old way)

Use `registerSlashCommand()` to register a new slash command:

```js
import { registerSlashCommand } from "../../slash-commands.js";

registerSlashCommand("commandname", commandFunction, ["alias"], "Description shown in /help");

function commandFunction(namedArgs, unnamedArgs) {
    // Command logic
}
```

Example of command execution:
```
/commandname namedArgument=value Unnamed argument
```

Arguments explanation:

1. `command` - the main command name. It will be used in autocompletion and help commands.
2. `callback` - a function that will be executed when the command is triggered. A callback function can accept two arguments:
  * `namedArgs` - an object with named arguments
  * `unnamedArgs` - a string containing the unnamed argument
3. `aliases` - an array of alias strings. The command can be called using any of the aliases, but they won't be shown in the autocomplete but will be listed in the help command.
4. `helpString` - a string that will be displayed when the `/help slash` command is called. Must describe what your command does and which arguments it accepts. May contain HTML markup.

All registered commands can be used in [STscript](https://docs.sillytavern.app/usage/st-script/) in any possible way.

> In rare circumstances, the unnamed command argument can also receive a number so be sure to type check or convert if you expect a concrete type!

### Listening to event types

Use eventSource.on() to listen for events:

```js
import { eventSource, event_types } from "../../../script.js";

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

The main event types are:

* `MESSAGE_RECEIVED`
* `MESSAGE_SENT`
* `CHAT_CHANGED`

The rest can be found [here](https://github.com/SillyTavern/SillyTavern/blob/a410c63333069100aee6d4e4b54196cd6accfa24/public/script.js#L399).

### Do Extras request

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
