# Integrating an External Provider

SillyTavern supports external services through several different integration routes. Choose the route before implementing a named provider so the result has an appropriate maintenance owner and does not add permanent core surface where the existing generic interfaces are sufficient.

!!!warning
Do not use a completed pull request as the first product discussion for a new named provider or service. A working implementation does not by itself establish that SillyTavern should maintain the integration.
!!!

## Choose the integration route

| Proposal | Recommended route |
| --- | --- |
| Fix or update a provider that SillyTavern already supports | Open a normal issue or pull request with reproduction and validation details. |
| Improve compatibility for a class of OpenAI-compatible services | Propose a provider-neutral issue or pull request against the generic **Custom (OpenAI-compatible)** path. |
| Add a model router, reseller, rerouter, gateway, or aggregator | Use **Custom (OpenAI-compatible)**: [build a provider extension](#option-1-build-a-provider-extension), or [publish a setup guide](#option-2-publish-a-setup-guide). Add a server plugin only when server-side behavior is needed. |
| Add a first-party model provider | Start with a public issue explaining the provider-owned models, the SillyTavern use case, and why the generic route is insufficient. |
| Add TTS, speech, web search, image, vectorization, embedding, reranking, or another non-chat service | Start with a public issue showing independent SillyTavern user demand, a clear product advantage, live validation, and a realistic maintenance owner. |

Existing integrations are not precedent for adding another one. New official provider integrations are exceptional because every named source becomes permanent UI, authentication, compatibility, testing, documentation, and support surface.

## Compatible chat providers

An external chat provider that implements the OpenAI Chat Completions API uses SillyTavern's existing generic source, **Custom (OpenAI-compatible)**. There are two ways to offer that to your users:

- [Option 1: Build a provider extension](#option-1-build-a-provider-extension). Your service appears by name in the source list and is configured for the user.
- [Option 2: Publish a setup guide](#option-2-publish-a-setup-guide). Your users configure Custom by hand, following instructions in your own documentation.

Both keep the provider-specific parts with the service that owns them.

## Option 1: Build a provider extension

A UI extension can add a named provider to the **Chat Completion Source** list. The provider is a **Custom (OpenAI-compatible)** connection with its endpoint, model list, key handling, and request adjustments already in place, so users pick it by name, enter a key, and connect.

This is the recommended route for a service with more than a handful of users: nobody has to copy an endpoint or work out which request parameter the service rejects.

!!!info Requirements
The Custom provider API is version 1 and needs SillyTavern 1.20.0 or newer. Until that release is out, use the `staging` branch.
!!!

### Start from the example

The quickest way is to fork the example extension:

<https://github.com/SillyTavern/Extension-ProviderExample>

It registers one provider and shows every part described on this page. Its settings are grouped in commented blocks, and its README ends with a template for your own extension's README.

Read [Writing UI Extensions](Writing-Extensions) for extension structure, the manifest, and lifecycle hooks in general.

### What users see

- Providers added by extensions are listed in the **Chat Completion Source** dropdown, in a **Custom Providers** group.
- Selecting one shows a provider card with its name, the extension that added it, optional documentation and support links, and a short hint.
- Below the card are an API key section and a model dropdown.
- The endpoint, the model ID text field, prompt post-processing, and the reset, detach, and forget actions are behind an **Advanced** switch.
- Selecting a provider never connects on its own. The user chooses a saved key or enters a new one and clicks **Connect**.
- Connection Profiles save and restore the whole connection.

### Register a provider

Register in the extension's `activate` and `enable` lifecycle hooks, and unregister in `disable`, `delete`, and `clean`.

```js
let registration = null;

export function onActivate() {
    if (registration) return; // both hooks can fire

    const { customProviders } = SillyTavern.getContext();
    if (!customProviders || customProviders.apiVersion !== 1) return;

    registration = customProviders.register('my-provider', {
        label: 'My Provider',
        defaults: { custom_url: 'https://api.example.com/v1' },
        models: { mode: 'discover' },
        auth: { mode: 'bearer', required: true },
    });
}

export function onDisable() {
    registration?.unregister();
    registration = null;
}
```

```json
{
    "hooks": {
        "activate": "onActivate",
        "enable": "onActivate",
        "disable": "onDisable",
        "delete": "onDisable",
        "clean": "onDisable"
    }
}
```

`register(providerId, definition)` detects which extension the call comes from. That extension is shown as the provider's origin, and disabling it removes the provider from the list.

`providerId` is a short stable ID: lowercase letters, digits, and dashes, at most 64 characters. Saved connections refer to it, so do not change it after release.

The returned handle has `id` (the full provider ID), `update(definition)`, and `unregister()`.

Registering only describes a connection. It does not select the provider, connect, store a key, or send a request.

### The definition

Unknown fields are rejected, and the error names the object and the field.

| Field | Meaning |
| --- | --- |
| `label` | Name shown in the source list. Plain text. Required. |
| `defaults` | Connection values the provider starts with. Required, see below. |
| `models` | How the model list is obtained. Required, see below. |
| `auth` | Whether and how the service takes a key. Required, see below. |
| `controls` | Chat Completion settings to show or hide while the provider is selected. Optional. |
| `documentationUrl`, `supportUrl` | HTTP(S) links shown in the provider card. Optional. |
| `note` | One short hint in the provider card, for example where to get a key. Optional. |

All types are documented in `public/scripts/custom-providers.js` (`CustomProviderDefinition` and the types it uses). An extension can import them in JSDoc for editor completion:

```js
/** @type {import('../../../custom-providers.js').CustomProviderDefaults} */
const CONNECTION = { custom_url: 'https://api.example.com/v1' };
```

#### defaults

| Field | Meaning |
| --- | --- |
| `custom_url` | The API root. HTTP or HTTPS, without `/chat/completions`, credentials, a query string, or a fragment. Required. |
| `custom_model` | Model ID that is preselected. |
| `custom_include_headers` | Extra request headers, as YAML. Not for keys. |
| `custom_include_body` | Body fields the server adds to every request, or overwrites, as YAML. |
| `custom_exclude_body` | Body fields the server removes from every request, as a YAML list. |
| `custom_prompt_post_processing` | How the prompt is reshaped before sending. Leave it out unless the service rejects the default layout. |

The user can change these values under **Advanced**. Their changes are kept, and **Reset provider defaults** restores yours.

#### models

- `{ mode: 'discover' }`: after connecting, the list is read from `<custom_url>/models`.
- `{ mode: 'manual' }`: the service has no model list. The user picks a suggestion or types an ID.

Add `suggestions: [{ id, label }]` to offer models before the first connection.

#### auth

- `{ mode: 'bearer', required: true }`: a key is needed and is sent as `Authorization: Bearer <key>`.
- `{ mode: 'bearer', required: false }`: a key is optional.
- `{ mode: 'header', name: 'x-api-key', required: true }`: the key is sent as the bare value of the named header.
- `{ mode: 'none' }`: the service takes no key. The key section is hidden.

### Keys

An extension never sees or stores a key.

- The user picks one of their saved Chat Completion keys, or enters a new one. A new key is saved without replacing the active key of its category.
- The chosen key is bound to the exact API root. If the root changes, the user has to choose the key again.
- Requests that carry a key do not follow redirects.
- Credential headers cannot be set through `custom_include_headers`. That includes `Authorization`, `x-api-key`, and the header named in `auth`.

### Fix requests the service rejects

Many services reject a parameter SillyTavern sends, or want it under another name. There are two tools.

**Declare it.** `custom_exclude_body` and `custom_include_body` are applied on the server to every request, including **Test Message** and requests other extensions send through a Connection Profile:

```js
defaults: {
    custom_url: 'https://api.example.com/v1',
    custom_exclude_body: '- max_tokens',
    custom_include_body: 'max_completion_tokens: 4096',
},
```

**Adjust each request.** Listen to `CHAT_COMPLETION_SETTINGS_READY`. The second argument says which provider the request was built for:

```js
eventSource.on(eventTypes.CHAT_COMPLETION_SETTINGS_READY, (request, context) => {
    if (context?.customProviderId !== registration?.id) return;

    // Send the user's response length under the name the service expects
    request.max_completion_tokens = request.max_tokens;
    delete request.max_tokens;
});
```

Compare `context.customProviderId` with your registration's `id`. Do not check which provider is selected at that moment: the user can switch providers while a request is being prepared.

Fields SillyTavern does not send by itself go through the Additional Parameters body, which is YAML text. To add one for a single request, append a line to `request.custom_include_body`.

### Show and hide settings

`controls` lists Chat Completion settings to show or hide while the provider is selected:

```js
controls: {
    hide: ['frequency_penalty', 'presence_penalty'],
    show: [],
},
```

The accepted names are in `SillyTavern.getContext().customProviders.controls`: `temperature`, `top_p`, `top_k`, `top_a`, `min_p`, `repetition_penalty`, `frequency_penalty`, `presence_penalty`, `seed`, `n`, `logit_bias`, `reasoning_effort`, `verbosity`, `show_thoughts`, `function_calling`, `web_search`, `inline_media`, `inline_image_quality`, `request_images`, `system_prompt`.

- Hiding a setting does not stop its value from being sent. Also list the field in `custom_exclude_body`.
- Showing a setting that the Custom source does not send by itself does not send it either. Add its value to the request in your `CHAT_COMPLETION_SETTINGS_READY` listener.

Settings that are not named keep the visibility of the Custom source.

### Add your own inputs

The provider card has an area for a provider's own inputs: the element with the ID `custom_provider_extension_settings`. Fill it while your provider is selected:

```js
function renderInputs() {
    const slot = document.getElementById('custom_provider_extension_settings');
    const isSelected = SillyTavern.getContext().customProviders.active() === registration?.id;
    if (!slot || !isSelected || slot.childElementCount) return;
    // create your inputs and append them to slot
}

eventSource.on(eventTypes.CUSTOM_CONNECTION_CHANGED, renderInputs);
eventSource.on(eventTypes.CHATCOMPLETION_SOURCE_CHANGED, renderInputs);
renderInputs();
```

SillyTavern empties the area when another connection takes over and when your extension is disabled. Save the values with your extension's settings and apply them in the request listener.

`customProviders.active()` returns the ID of the selected provider, or `null` for the Manual connection and every other source. Use it for the interface, not to decide whether a request is yours.

### The rest of the API

| Call | Meaning |
| --- | --- |
| `customProviders.apiVersion` | Version of this API. Check it before registering. |
| `customProviders.get(id)` | A copy of one registered definition, by full ID. |
| `customProviders.list()` | Copies of all registered providers, sorted by label. |
| `customProviders.select(id)` | Selects a provider, or the Manual connection with `null`. Only while Chat Completion with the Custom source is selected. Does not connect. |

Events:

- `CUSTOM_PROVIDER_REGISTRY_CHANGED`: `{ id, ownerExtension, operation }`, where `operation` is `register`, `update`, or `unregister`.
- `CUSTOM_CONNECTION_CHANGED`: `{ oldProviderId, newProviderId, reason }`.

### When the extension is removed

The connection settings stay usable without the extension: endpoint, model, the chosen key, and the declared request fields. The provider is shown as "extension unavailable".

The extension's own inputs and its per-request adjustments stop. If your service only works with a per-request adjustment, say so in your README.

### Limits of version 1

- Authentication is a bearer key, a key in a named header, or none. Signed requests and keys in the URL are not supported.
- Requests that another extension sends through a Connection Profile do not raise `CHAT_COMPLETION_SETTINGS_READY`. They only get the declared request fields.
- The provider uses the Chat Completion request format. A service with another API needs a [server plugin](Server-Plugins).

## Option 2: Publish a setup guide

Without an extension, users connect through the generic source directly:

1. Open **API Connections**.
2. Select **Chat Completion**.
3. Select **Custom (OpenAI-compatible)** as the source.
4. Enter the provider's API root and API key.
5. Select a model returned by `/v1/models`, or enter the model ID manually.
6. Use **Test Message** to verify the connection.

See [Custom OpenAI-compatible endpoint](/usage/api-connections/openai/#custom-openai-compatible-endpoint) for the complete user instructions, prompt post-processing options, and troubleshooting notes.

The provider should publish and maintain its own SillyTavern setup page. Provider-specific endpoint URLs, account steps, model catalogues, regions, and compatibility notes change independently of SillyTavern and should stay with the service that owns them. The official SillyTavern documentation is not a catalogue of every compatible commercial endpoint.

### Copyable provider setup guide

Use this as a starting point in the provider's own documentation. Replace every placeholder and remove unsupported features rather than presenting guesses as compatibility claims.

~~~md
# Use <Provider> with SillyTavern

## Prerequisites

- A <Provider> account
- An API key
- A model ID

## Connect

1. In SillyTavern, open **API Connections**.
2. Select **Chat Completion**.
3. Select **Custom (OpenAI-compatible)** as the source.
4. Enter `<base URL>` as the endpoint. Use the API root, normally ending in `/v1`, not `/chat/completions`.
5. Enter your API key.
6. Select a model from the fetched list, or enter `<model ID format>` manually.
7. Select `<prompt post-processing setting>` if the provider requires one.
8. Click **Test Message**.

## Feature support

- Model discovery through `/v1/models`: <yes/no/limitations>
- Streaming: <yes/no/limitations>
- Tool calling: <yes/no/limitations>
- Vision: <yes/no/limitations>
- Reasoning: <yes/no/limitations>
- Structured output: <yes/no/limitations>

## Known limitations

<List provider-specific compatibility notes and features that were not tested.>

## Support

For provider API, billing, account, or model availability problems, contact <provider support route>.

Last verified with SillyTavern <version or commit> on <date>.
~~~

A setup guide documents a compatible route; it is not an endorsement, a guarantee of complete compatibility, or a transfer of provider support to the SillyTavern maintainers.

## Other provider-specific behavior in a UI extension

A UI extension is also the appropriate home for provider-owned behavior beyond what a registered provider covers, for example:

- guided configuration and connection testing;
- provider-specific optional settings;
- model presentation or discovery helpers;
- account, credit, or usage links;
- defaults for headers, request fields, or prompt post-processing;
- capability and limitation explanations; or
- provider branding and support links.

The extension may integrate with SillyTavern's existing UI, events, context, and request paths as needed. It remains third-party provider-owned surface, so the extension author is responsible for compatibility updates and user support.

Read [Writing UI Extensions](Writing-Extensions) for extension structure, lifecycle, settings, templates, and current API guidance.

## Add a server plugin only for server-side behavior

A normal OpenAI-compatible service does not need a server plugin merely to avoid manual configuration. Use a server plugin when the integration genuinely requires Node-side behavior, such as:

- provider-specific server endpoints;
- request signing or credential exchange that cannot safely occur in the browser;
- protocol translation for an API that is not OpenAI-compatible;
- a server-side proxy or callback flow; or
- Node.js dependencies unavailable to a UI extension.

Server plugins are not sandboxed and must be explicitly enabled by the user. Keep the UI in an extension and the privileged server behavior in the plugin when both are required. Read [Writing Server Plugins](Server-Plugins) before choosing this route.

## Propose provider-neutral compatibility improvements

A named integration may reveal a real limitation in the generic source. Propose that limitation separately when the improvement benefits compatible endpoints as a class.

A provider-neutral proposal should:

- reproduce the behavior through **Custom (OpenAI-compatible)**;
- describe the affected API shape rather than only one brand;
- avoid adding a named source, fixed provider URL, icon, or provider-specific branch;
- preserve existing compatible endpoints; and
- include focused validation for the generic behavior.

Provider-neutral improvements remain welcome even when SillyTavern does not accept the named integration that exposed the problem.

## Discuss an official integration before implementation

A first-party provider may be considered for an official integration when a named source adds meaningful capability, correctness, or user experience that the generic and third-party routes cannot provide proportionately.

For a first-party model provider, the issue should explain:

- which meaningful models the provider develops and operates;
- the concrete SillyTavern use case;
- why **Custom (OpenAI-compatible)** is insufficient;
- the smallest provider-specific surface required;
- how maintainers can validate the live API; and
- who will maintain the integration as the API changes.

For TTS, speech, web search, image, vectorization, embeddings, reranking, translation, and similar services, the issue must also show independent SillyTavern user demand and a clear advantage over existing sources before implementation begins.

Opening an issue, gathering reactions, supplying test access, or offering long-term maintenance makes a proposal discussable; none of those guarantees acceptance.

## Keep ownership and support explicit

Third-party provider documentation, extensions, and plugins should clearly state:

- who maintains the integration;
- where users report provider-specific problems;
- which SillyTavern versions were tested;
- which provider capabilities are supported or unsupported;
- what data and credentials leave the user's installation; and
- the last date or version against which the instructions were verified.

SillyTavern maintainers may help with provider-neutral core defects, but provider account, billing, API availability, model catalogue, and third-party integration support remain with the provider or extension author.
