---
icon: server
---

# Server Plugins

These plugins allow adding functionality that is impossible to achieve using UI extensions alone, such as creating new API endpoints or using Node.JS packages that are unavailable in a browser environment.

Plugins are contained in the `plugins` directory of SillyTavern and loaded on server startup, but *only* if `enableServerPlugins` is set to true in the `config.yaml` file.

> **Plugins are not sandboxed, be careful what you install and run!**

## Types of plugins

### Files

An executable JavaScript file with ".js" (for CommonJS modules) or ".mjs" (for ES modules) extension containing a module that exports an `init` function that accepts an Express router (created specifically for your plugin) as an argument and returns a Promise.

The module should also export an `info` object containing the information about the plugin (`id`, `name`, and `description` strings). This will provide the information about the plugin to the loader.

You can register routes via the router that will be registered under the `/api/plugins/{id}/{route}` path. For example `router.get('/foo')` for plugin `example` will produce a route like this: `/api/plugins/example/foo`.

A plugin could also *optionally* export an `exit` function that performs clean-up on shutting down the server. It should have no arguments and must return a Promise.

See below for a "Hello world!" plugin example:

```js
/**
 * Initialize plugin.
 * @param {import('express').Router} router Express router
 * @returns {Promise<any>} Promise that resolves when plugin is initialized
 */
async function init(router) {
    // Do initialization here...
    router.get('/foo', req, res, function () {
       res.send('bar');
    });
    console.log('Example plugin loaded!');
    return Promise.resolve();
}

async function exit() {
    // Do some clean-up here...
    return Promise.resolve();
}

module.exports = {
    init,
    exit,
    info: {
        id: 'example',
        name: 'Example',
        description: 'My cool plugin!',
    },
};
```

### Directories

You can load a plugin from a subdirectory in the `plugins` in one of the following ways (in the order of checks):

1. `package.json` file that contains a path to an executable file in the "main" field.
2. `index.js` file for CommonJS modules.
3. `index.mjs` file for ES modules.

A resulting file must export an `init` function and `info` object with the same requirements as for individual files.

It is preferable to use a bundler (such as Webpack or Browserify) that will package all of the requirements into one file. Make sure to set "Node" as a build target.

Example of a directory plugin (with `index.js` file): https://github.com/Cohee1207/SillyTavern-DiscordRichPresence-Server
