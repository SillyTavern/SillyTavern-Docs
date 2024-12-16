---
templating: false
---

# Chat Translation

## Overview

The Chat Translation Extension enables real-time translation of chat messages between
different languages using various translation providers. It supports both manual and
automatic translation modes.

![Character message translated from English to Chinese using 'Translate Message/翻譯訊息' message action button](../static/extensions/translation/sensei.png)
 
+++ English
!["Translate Chat", "Translate Input"](../static/extensions/translation/wand-menu-en.png)
+++ 简体中文
!["翻译聊天", "翻译输入"](../static/extensions/translation/wand-menu-zh-cn.png)
+++ 繁體中文
!["翻譯聊天內容", "翻譯輸入內容"](../static/extensions/translation/wand-menu-zh-tw.png)
+++ 한국어
!["채팅 번역하기", "입력 번역하기"](../static/extensions/translation/wand-menu-ko.png)
+++ Русский
!["Перевести чат", "Перевести моё сообщение"](../static/extensions/translation/wand-menu-ru.png)
+++

## Usage

All the ways to translate chat messages:

**<i class="fa-solid fa-language"></i> Translate Chat** button in the **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions** menu

- Translates the entire chat history at once

**<i class="fa-solid fa-keyboard"></i> Translate Input** button in the **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions** menu

- Translates just the current input text
- Useful before sending a message

**<i class="fa-solid fa-language"></i> Translate Message** icon in the **<i class="fa-solid fa-ellipsis"></i> Message Actions**
toolbar of any message

- Click to translate just that message
- Click again to revert to original text

**Auto-mode** configuration in the **Chat Translation** drawer of the **<i class="fa-solid fa-cubes"></i>
Extensions** panel

- Automatically translates user inputs, AI responses, or both

**/translate** slash command

- Use `/translate [target=language_code] text` to translate text

## Configuration

Configuration options are available in the **Chat Translation** drawer of the **<i class="fa-solid fa-cubes"></i>
Extensions** panel.

#### Provider

- Choose your preferred [translation service](#translation-providers)
- Click the **<i class="fa-solid fa-key"></i> API Key** icon, if it appears, to enter an API key
- Click the **<i class="fa-solid fa-link"></i> Custom URL** icon, if it appears, to enter a custom API URL

#### Target Language

Select the language you want to write your messages in, or read AI responses in.

#### Auto-mode

Configure automatic translation behavior.

- **None**: No automatic translation
- **Translate responses**: Automatically translates AI responses into the target language
- **Translate inputs**: Automatically translates user inputs into English
- **Translate both**: Translates both user inputs and AI responses

#### Clear Translations

The **<i class="fa-solid fa-trash-can"></i> Clear Translations** button removes all translations from messages in the
current chat. The original messages are preserved.

### Configuration Example: Chinese to English Chatting

To set up a workflow where a Chinese-speaking user can chat in Chinese with an AI that operates in English:

1. Set Auto-mode to "Translate both"
2. Set Target Language to "Chinese (Simplified)" or "Chinese (Traditional)"
3. Choose a translation provider with good language auto-detection (e.g., Google or DeepL)

This setup will:

- Translate user's Chinese input to English for the AI
- Translate AI's English responses back to Chinese for the user

This setup relies on automatic language detection for input. For more precise control, future updates may include
explicit source language selection.

## Translation providers

**:icon-cloud:** Cloud-based
**<i class="fa-solid fa-link"></i>** Local, custom URL
**<i class="fa-solid fa-key"></i>** Requires API key

| Provider                                                            | Location                                                                      | Features                                                                                               |
|---------------------------------------------------------------------|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| [Libre Translate](https://libretranslate.com/)                      | :icon-cloud: <i class="fa-solid fa-key"></i> <i class="fa-solid fa-link"></i> | Self-hosted (AGPL-3.0) alternative to proprietary translation services, with cloud-hosted Pro tier     | 
| [Google Translate](https://cloud.google.com/translate)              | :icon-cloud:                                                                  | Widely used, supports many languages, good accuracy                                                    |
| [Lingva Translate](https://lingva.ml/)                              | <i class="fa-solid fa-link"></i>                                              | Alternative front-end for Google Translate, open source (AGPL-3.0), privacy-focused                    |
| [DeepL](https://www.deepl.com/)                                     | :icon-cloud: <i class="fa-solid fa-key"></i>                                  | High-quality translations, especially for European languages                                           |
| [DeepLX](https://github.com/OwO-Network/DeepLX)                     | <i class="fa-solid fa-link"></i>                                              | Self-hosted DeepL proxy, open source (MIT), free but proxying DeepL Pro requires DeepL API key         |
| [Bing Translator](https://www.bing.com/translator)                  | :icon-cloud:                                                                  | Microsoft's translation service, integrates with Azure services                                        |
| [OneRing Translator](https://github.com/janvarev/OneRingTranslator) | <i class="fa-solid fa-link"></i>                                              | Self-hosted front-end to Google Translate and other providers, privacy-focused, open source (AGPL-3.0) |
| [Yandex Translate](https://translate.yandex.com/)                   | :icon-cloud:                                                                  | Good for Russian and Eastern European languages                                                        |

### DeepL-specific configuration

- Formality levels available for German, French, Italian, Spanish, Dutch, Japanese, and Russian
- Configure via `deepl.formality` in [config.yaml](/Administration/config-yaml.md#deepl-configuration)

## Slash Commands

Use `/translate` command for quick translations. Syntax: `/translate [target=language_code] text`. If target language is
not provided, the value from the extension settings will be used.   

### Basic usage

Translate text to the current target language and show it in a popup:

```
/translate Welcome to the Tavern | /echo 
```

![Popup in Chinese (Simplified), '欢迎来到酒馆/Welcome to the Tavern'](../static/extensions/translation/welcome-tavern.png)

Translate text to Spanish and add it to the chat:

```
/translate target=es Hello world | /send
```

![User message in Spanish, 'Hola Mundo/Hello world'](/static/extensions/translation/hola-mundo.png)

### Testing, pipeline translation, localization

Prompt the user for a message and a language, translate the message into that language, then re-translate it into the
configured target language and show both translations in a popup. This example uses the `/input` and `/buttons` commands to
gather user input:

```shell
/input default="Hello, world!" <span data-i18n="Test Message">Sample text</span> | 
/let key=input ||
/buttons labels=["zh-CN", "zh-TW", "es", "hu", "en"] <span data-i18n="UI Language">Language</span> | 
/let key=lang ||
/translate target={{var::lang}} {{var::input}} | /let key=tx_target | 
/translate | /let key=tx_orig ||
/echo escapeHtml=false cssClass=wider_dialogue_popup
<b data-i18n="Test Message">Test message</b>: {{var::input}} <br/>
<b data-i18n="Output">Output</b> ({{var::lang}}): {{var::tx_target}} <br/>
<b data-i18n="Output">Output</b> (<span data-i18n="ext_translate_target_lang">target language</span>): {{var::tx_orig}} <br/>
```

This is useful for checking the quality of a translation into a language that you don't speak, before writing it
somewhere important.

![Popup, 'Welcome to the Tavern/欢迎来到酒馆/welcome to the pub', en, zh-CN, en](../static/extensions/translation/welcome-tavern-en-cn.png)
![Popup, 'My hovercraft is full of eels/我的氣墊船裡裝滿了鰻魚/My hovercraft is filled with eels', en, zh-TW, en](../static/extensions/translation/eels-out-zh-tw.png)

The UI controls are shown in the current locale, independent of the configured target language.

| `/input`                                                                                        | `/buttons`                                                                          |
|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| ![Input dialog, '发送测试消息/Send Test Message'](../static/extensions/translation/eels-input-zh.png) | ![Buttons dialog, '语言/Language'](../static/extensions/translation/eels-lang-zh.png) |

![Popup, '我的氣墊船裡裝滿了鰻魚/My hovercraft is full of eels', zh-TW -> en -> zh-TW](../static/extensions/translation/eels-out-tw-en.png)

Input language detection is relatively effective in the following examples:

![Popup, '(My hovercraft is full of eels)/A légpárnás hajóm tele van angolnával/我的氣墊船裡裝滿了鰻魚', zh-TW -> hu -> zh-TW](../static/extensions/translation/eels-out-tw-hu.png)
![Popup, '我的氣墊船裡裝滿了鰻魚/Mi aerodeslizador está lleno de anguilas/My hovercraft is full of eels', zh-TW -> es -> en](../static/extensions/translation/eels-out-tw-es-en.png)
![Popup, 'Il mio hovercraft è pieno di anguille/我的气垫船里装满了鳗鱼/My hovercraft is filled with eels', it -> zh-CN -> en](../static/extensions/translation/eels-out-it-zhCN-en.png)

## Technical Notes

- UTF-8 encoding, special characters, and emojis are supported
- Handles large messages by splitting into chunks when needed
- Preserves formatting and embedded images in messages
- Caches translations to avoid redundant API callschecking

### AI input language

`internal_language` controls the language into which user messages are auto-translated before being sent to the AI. It is hardcoded to 'en' in the default settings and cannot be changed through the UI. Thus, the translation target language for messages *to the AI* is always English. Previous testing showed that AI performance was better when receiving English messages, but this may change as more LLMs are being trained on more varied language data. I suppose one could change `internal_language` in `settings.json` and find out.

### Chinese variant handling

The extension supports both Simplified and Traditional Chinese, but not all translation providers do. The UI presents these as 'Chinese (Simplified)' and 'Chinese (Traditional)' respectively, with language codes 'zh-CN' and 'zh-TW'. They are mapped to the following language codes for translation providers:

* Libre Translate: 'zh-CN' to 'zh' and 'zh-TW' to 'zt'.
* DeepL and DeepLX: both variants to 'ZH'.
* Bing: 'zh-CN' to 'zh-Hans', 'zh-TW' as-is.
* Other providers use 'zh-CN' and 'zh-TW' as provided.

### Text length limits

Some providers have character limits per request:

- Yandex: 5000 characters
- DeepLX: 1500 characters
- Bing: 1000 characters
- Google: 5000 characters

Longer texts are automatically split into chunks for translation.
