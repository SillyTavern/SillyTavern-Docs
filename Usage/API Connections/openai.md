---
order: 20
---
# Chat Completions

Chat completion APIs include OpenAI, Claude, and PaLM.
WindowAI & OpenRouter allows connection to these as well.

## OpenAI

### API key

**How to get:**

1. Go to [OpenAI](https://platform.openai.com/) and sign in.
2. Use "[View API keys](https://platform.openai.com/account/api-keys)" option to create a new API key.

**Important!**

*Lost API keys can't be restored! Make sure to keep it safe!*

## Claude

If you have access to Anthropic's Claude API:

- Select 'Claude' for 'Chat Completion Source'.
- Input your API key.
- Click connect.

## Proxy

!!!warning
It is important to note that we do not provide support for possible issues that you may have!
We do not guarantee compatibility with every possible API endpoint!
!!!

!!!
If you intend to use this proxy feature to use a local endpoint, like TabbyAPI, Oobabooga, Aphrodite, or any like those, you might want to check out the [built-in compatibility for those](https://docs.sillytavern.app/usage/api-connections/) instead. This proxy feature is mainly intended for use with other services and programs that expose an OpenAI-compatible API Chat Completion endpoint.
Most Text Completion APIs support far greater customization options than OpenAI's standards allow for. These greater customization options, such as the Min-P sampler, may be worthwhile for SillyTavern users to check out, which can greatly improve the quality of generations.
!!!

It is possible to configure a proxy/alternative endpoint for OpenAI's backend. This custom endpoint can connect to alternative Chat Completion APIs that support the generic OpenAI API schema.

Examples of backends which implement this API are:

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://litellm.ai/)
* [LocalAI](https://localai.io/)

This feature is accessed by:

- Switching to the 'Chat Completion' API type.
- Selecting 'OpenAI' for 'Chat Completion Source'.
- Leaving the details like the API key empty.
- Opening the 'AI Response Configuration' tab and scrolling down to the 'OpenAI / Claude Reverse Proxy' section.

There you may enter the proxy/custom endpoint and optionally an API key under 'Proxy Password' if needed.
For example, TabbyAPI provides you with an API key you have to use.

Back in the 'AI Connections' tab, you can find two optional checkboxes labeled:

- Bypass API status check.
- Show "External" models (provided by API).

Checking 'Bypass API status check' tells SillyTavern to stop alerting you about a non-functioning API endpoint. Check this if your API endpoint works, but SillyTavern keeps warning you anyway.

> **Hint:** If it doesn't work, try adding `/v1` at the end of the endpoint URL!

Checking 'Show "External" models (provided by API)' will show the external available models as reported by your custom API endpoint in the dropdown (scroll down past OpenAI's models). This allows you to select different API models right from SillyTavern without having to go into your custom app and change the model.

**This feature is not required for custom API endpoints to work** and might not be available on every backend.
