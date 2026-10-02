---
order: -10
route: /for-contributors/provider-integrations/
---

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
| Add a model router, reseller, rerouter, gateway, or aggregator | Publish a provider-owned setup guide for **Custom (OpenAI-compatible)**. Add a third-party UI extension or server plugin only when provider-specific behavior is needed. |
| Add a first-party model provider | Start with a public issue explaining the provider-owned models, the SillyTavern use case, and why the generic route is insufficient. |
| Add TTS, speech, web search, image, vectorization, embedding, reranking, or another non-chat service | Start with a public issue showing independent SillyTavern user demand, a clear product advantage, live validation, and a realistic maintenance owner. |

Existing integrations are not precedent for adding another one. New official provider integrations are exceptional because every named source becomes permanent UI, authentication, compatibility, testing, documentation, and support surface.

## Use Custom (OpenAI-compatible) for compatible chat providers

An external chat provider that implements the OpenAI Chat Completions API should normally use SillyTavern's existing generic source:

1. Open **API Connections**.
2. Select **Chat Completion**.
3. Select **Custom (OpenAI-compatible)** as the source.
4. Enter the provider's API root and API key.
5. Select a model returned by `/v1/models`, or enter the model ID manually.
6. Use **Test Message** to verify the connection.

See [Custom OpenAI-compatible endpoint](/usage/api-connections/openai/#custom-openai-compatible-endpoint) for the complete user instructions, prompt post-processing options, and troubleshooting notes.

The provider should publish and maintain its own SillyTavern setup page. Provider-specific endpoint URLs, account steps, model catalogues, regions, and compatibility notes change independently of SillyTavern and should stay with the service that owns them. The official SillyTavern documentation is not a catalogue of every compatible commercial endpoint.

## Copyable provider setup guide

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

## Build a third-party UI extension for provider-specific setup

A UI extension is the appropriate home when a provider needs convenience or provider-owned behavior beyond a written setup guide, for example:

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
