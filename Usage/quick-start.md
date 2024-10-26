---
title: Quick Start
icon: rocket
order: -10
---

!!!light
I'm clueless. Just spoonfeed me the easiest and fastest way I can start using SillyTavern. -- *Anonymous*
!!!

You can get started with SillyTavern in just a few minutes. Here are two easy ways to get started:

* You can [use AI Horde](#quick-start-with-ai-horde) for free. AI Horde is a community-driven AI service that provides access to a variety of AI models.

* If you have an OpenAI account or want to register one, you can [use OpenAI](#quick-start-with-openai).

## Quick start with AI Horde

1. Follow the [Installation Guide](/Installation/index.md) to install and start SillyTavern.

2. In SillyTavern's onboarding screen, enter a name for your persona. This name will be used in the chat.

   ![This is an optional caption](/static/quick-start/1_name.png)
3. Click the API Connections button in the top bar.

   ![This is an optional caption](/static/quick-start/2_api_conn.png)
4. Enter an API key for AI Horde. You can use `0000000000` for now, or get a free key from [AI Horde](https://aihorde.net/).

   ![This is an optional caption](/static/quick-start/3_horde_key.png)
5. Select some AI models to use. Just choose a few from the top. You can always change them later.

   ![This is an optional caption](/static/quick-start/4_horde_models.png)
6. Close the API Connections window. Enter a message in the chat box at the bottom and press Enter.

   ![This is an optional caption](/static/quick-start/5_msg.png)
7. Your AI will respond in a few moments. You can continue chatting with it. Success!

   ![This is an optional caption](/static/quick-start/6_success.png)

## Quick start with OpenAI

### Install SillyTavern

Follow the [Installation Guide](/Installation/index.md) to install and start SillyTavern.

### Get access to OpenAI

1. Sign up to OpenAI.
2. Go to <https://platform.openai.com>
3. Click your account icon in the top right, then View API Keys.
4. Click "Create new secret key". Copy it somewhere immediately. **DO NOT SHARE THIS KEY. WHOEVER HAS IT CAN USE YOUR ACCOUNT TO USE GPT AT YOUR EXPENSE.**

### Configure SillyTavern to use your API

1. In SillyTavern's top bar, click API Connections.
2. Under API, select Chat Completion (OpenAI).
3. Under Chat Completion Source, select OpenAI.
4. Paste the API key you saved in the previous step.
5. Click the Connect button. Confirm it says Valid.
6. By default, SillyTavern will use GPT-4 Turbo. You can choose a different model, but educate yourself on the pricing.

### Test your setup

1. In SillyTavern's top bar, click Character Management at the far right.
2. Select an existing character such as Seraphina.
3. In the text box at the bottom, write something to Seraphina, then press Enter or click the Send button.

If you did everything right, after a few seconds, Seraphina should respond.
