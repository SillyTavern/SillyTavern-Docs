---
icon: file-added
---

# Writing Extensions

Plugins extend the functionality of SillyTavern by hooking into its events and API. You can easily create your own extensions.

### manifest.json

Every extension must have a folder in `public/scripts/extensions` and have a manifest.json file which contains metadata about the plugin and a JS script file.

```js
{
    "display_name": "The name of the plugin",
    "loading_order": 1, // Optional. Higher number loads later
    "requires": [], // Required Extras modules dependecies
    "optional": [], // Optinal Extras dependencies
    "js": "index.js", // Main JS file
    "css": "style.css", // Optional CSS file
    "author": "Your name",
    "version": "1.0.0", 
    "homePage": "https://github.com/your/plugin" 
}
```

The display_name, js and author fields are required.

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

### Importing from other files

You can import variables and functions from other JS files.

For example, this code snipped will generate a reply from the currently selected API in a background:

```js
import { generateQuietPrompt } from "../../../script.js";

function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt(text);
    // ...
}
```

### Registering slash commands

Use `registerSlashCommand()` to register a new slash command:

```js
import { registerSlashCommand } from "../../slash-commands.js";

registerSlashCommand("commandname", commandFunction, ["alias"], "Description shown in /help", true, true);

function commandFunction(args) {
    // Command logic
}
```

### Listening for event types

Use eventSource.on() to listen for events:

```js
import { eventSource, event_types } from "../../../script.js";

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

Main event types are:

* `MESSAGE_RECEIVED`
* `MESSAGE_SENT`
* `CHAT_CHANGED`

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
