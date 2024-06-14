---
order: 10
---
# Featherless

## Overview

Featherless is a private model service aiming to be the easiest and fastest way to run any model. Featherless allows individuals to run models privately and uncensored: with no interference or logging of prompts or completions. The service's privacy policy is posted [here](https://featherless.ai/privacy)).

At the time of writing, there are 15+ models all geared for RP and ERP, including

* [Gryphe/MythoMax-L2-13b](https://featherless.ai/models/Gryphe/MythoMax-L2-13b)
* Sao10k's [Fimbulvetr](Sao10K/Fimbulvetr-11B-v2) and [Stheno](https://featherless.ai/models/Sao10K/L3-8B-Stheno-v3.2)
* [Lumimaid](https://huggingface.co/NeverSleep/Llama-3-Lumimaid-8B-v0.1)

The service is $10 / month. It does not provide a trial tier. The developer team is responsive on [discord](https://featherless.ai/discord) and [email](hello@featherless.ai)

## Connection Instructions

For a video instruction, see on youtube [here](https://www.youtube.com/watch?v=LxluaUr8DJM).

_Supposing you have subscribed_,

When configuring your API connection, use the following settings:

1. Set `API` to either `Chat Completion` or `Text Completion`
2. Set `Chat Completion Source` to `Custom (OpenAI-compatible)``
3. Set `Custome Endpoint (Base URL)` to the value `https://api.featherless.ai/v1`
4. Set `Custom API Key` to a value retrieved from [your API Key page in the Featherless Web UI](https://featherless.ai/api-keys)
5. Set `Enter a Model ID` to the model of your choice!

![API Key](/static/featherless-api-settings.png)




