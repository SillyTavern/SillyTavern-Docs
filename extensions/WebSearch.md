# Web Search

Adds web search results to LLM prompts.

## Available sources

### Selenium Plugin

Requires an official server plugin to be installed and enabled.

See [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium) for more details.

Supports Google and DuckDuckGo engines.

### Extras API

Requires a `websearch` module and Chrome/Firefox web browser installed on the host machine.

Supports Google and DuckDuckGo engines.

### SerpApi

Requires SerpApi key and provides access to Google search.

Get the key here: https://serpapi.com/dashboard

## How to use

1. Make sure you use the latest version of SillyTavern.
2. Install the extension via the "Download Extensions & Assets" menu in SillyTavern.
3. Open the "Web Search" extension settings, set your API key or connect to Extras, and enable the extension.
4. The web search results will be added to the prompt organically as you chat. **Only user messages trigger the search.**
5. To include search results more organically, wrap search queries with single backticks: ```Tell me about the `latest Ryan Gosling movie`.``` will produce a search query `latest Ryan Gosling movie`.
6. Optionally, configure the settings to your liking.

## Settings

### General

1. Enabled - toggles the extension on and off.
2. Sources = sets the search results source.
3. Cache Lifetime - how long (in seconds) the search results are cached for your prompt. Default = one week.

### Prompt Settings

1. Prompt Budget - sets the maximum capacity of the inserted text (in characters of text, NOT tokens). Rule of thumb: 1 token ~ 3-4 characters, adjust according to your model's context limits. Default = 1500 characters.
2. Insertion Template - how the result gets inserted into the prompt. Supports the usual macro + special macro: \{\{query\}\} for search query and \{\{text\}\} for search results.
3. Injection Position - where the result goes in the prompt. The same options as for the Author's Note: as in-chat injection or before/after system prompt.

### Search Activation

1. Use Backticks - enables search activation using words encased in single backticks.
2. Use Trigger Phrases - enables search activation using trigger phrases.
3. Trigger Phrases - add phrases that will trigger the search, one by one. It can be anywhere in the message, and the query starts from the trigger word and spans to "Max Words" total. To exclude a specific message from processing, it must start with a period, e.g. `.What do you think?`. Priority of triggers: first by order in the textbox, then the first one in the user message.
4. Max Words - how many words are included in the search query (including the trigger phrase). Google has a limit of about 32 words per prompt. Default = 10 words.

### Page Scraping

1. Visit Links - text will be extracted from the visited search result pages and saved to a file attachment.
2. Visit Count - how many links will be visited and parsed for text.
3. Visit Domain Blacklist - site domains to be excluded from visiting. One per line.
4. File Header - file header template, inserted at the start of the text file, has an additional \{\{query\}\} macro.
5. Block Header - link block template, inserted with the parsed content of every link. Use \{\{link\}\} macro for page URL and \{\{text\}\} for page content.

## More info

Search results from the latest query will stay included in the prompt until the next valid query is found.
If you want to ask additional questions without accidentally triggering the search, start your message with a period.

If both backticks and trigger phrases search activation are used, backticks have a higher priority.

To discard all previous queries from processing, start the user message with an exclamation mark, for example, a user message `!Now let's talk about...` will discard this and every message above it.

This extension also provides a `/websearch` slash command to use in STscript. More info here: [STscript Language Reference](https://docs.sillytavern.app/usage/st-script/)

```
/websearch (links=on|off snippets=on|off [query]) – performs a web search query. Use named arguments to specify what to return - page snippets (default: on), full parsed pages (default: off) or both.

Example: /websearch links=off snippets=on how to make a sandwich
```

### What can be included in the search result?

#### SerpApi

1. Answer box. Direct answer to the question.
2. Knowledge graph. Encyclopedic knowledge about the topic.
3. Page snippets (max 10). Relevant extracts from the web pages.
4. Relevant questions (max 10). Questions and answers to similar topics.

#### Selenium Plugin and Extras API

1. Google - answer box, knowledge graph, page snippets.
2. DuckDuckGo - page snippets.
