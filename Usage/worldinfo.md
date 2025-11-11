---
order: 130
icon: globe
route: /usage/core-concepts/worldinfo/
templating: false
---

# World Info

**World Info (also known as Lorebooks or Memory Books) is a powerful tool available in ST to insert prompts dynamically into your chat to help guide the AI replies.**

Commonly, World Info (WI for short) is used to  enhance the AI's understanding of the details in your fictional world, however you could use a World Info entry to insert ANYTHING that you would like to insert into the prompt.

It functions like a dynamic dictionary that only inserts relevant information from World Info entries when keywords associated with the entries are present in the message text.

The SillyTavern engine activates and seamlessly integrates the appropriate lore into the prompt, providing background information to the AI.

*It is important to note that while World Info helps guide the AI toward the desired content, it does not guarantee its appearance in the generated output messages. That depends on how good your model is at making use of additional information!*

## Pro Tips

* The World Info engine is a very powerful prompt management tool. Don't fixate on adding character lore alone, feel free to experiment.
* Activation keywords, titles, and other information that is not in the **Content** field is not inserted into context, so each World Info entry should have a comprehensive, standalone description.
* To create rich and detailed world lore, entries can be interlinked and reference one another by using recursive activation. See more on [Recursion](#recursive-scanning) below.
* SillyTavern offers flexible context budgeting for inserted background information. To conserve prompt tokens, it is advisable to keep entry contents concise.

## Further reading

* [World Info Encyclopedia](https://rentry.co/world-info-encyclopedia): Exhaustive in-depth guide to World Info and Lorebooks. By kingbri, Alicat, Trappu.

## Character Lore

Optionally, World Info files can be assigned to a character to serve as dedicated lore sources across all chats with that character (including groups).

One primary World Info can be bound to the character. To do that, navigate to the Character Management panel and click the globe button, then pick World Info from a dropdown list and click "Ok". When exporting the character, this file will also get embedded in the character card data.

To unbind, change, or assign additional World Info files as character lore, shift-click the globe button or click "More..." then "Link World Info". Note that only the primary World Info file gets exported with the character.

### Character Lore Insertion Strategy

When generating an AI reply, entries from the character World Info will be combined with the entries from a global World Info selector using one of the following strategies:

#### Sorted Evenly (default)

All entries will be sorted according to their Insertion Order as if they a part of one big file, ignoring the source.

#### Character Lore First

Entries from the Character World Info would be included first by their Insertion Order, then entries from the Global World Info.

#### Global Lore First

Entries from the Global World Info Info would be included first by their Insertion Order, then entries from the Character World Info.

### World Info Entry

#### Key

A list of keywords that trigger the activation of a World Info entry. Keys are not case-sensitive by default (this is [configurable](#case-sensitive-keys)).

##### Regular Expression (Regex) as Keys

Keys allow a more flexible approach to matching by supporting regex. This makes it possible to match more dynamic content with optional words or characters, spacing, and all the other utilities that regex provides.  
If a defined key is a valid regex (Javascript regex style, with `/` as delimiters. All flags are allowed), it will be treated as such when checking whether an entry should be triggered. Multiple regexes can be entered as separate keys and will work alongside each other. Inside a regex, commas are possible. Plaintext keys do not support commas, as they are treated as key separators.  

An example of a use-case for advanced regex matching:  
An entry/instruction that should be inserted, when char is doing a weather-related action

```js
/(?:{{char}}|he|she) (?:is talking about|is noticing|is checking whether|observes) (?:the )?(rainy weather|heavy wind|it is going to rain|cloudy sky)/i
```

For more information on Regex syntax and possibilities: [Regular expressions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions)

###### Advanced Regex Per-Message Matching

ST prefixes every chat message in the WI scan buffer with `character name:` and after v1.12.6, concatenates prepends them using the character value 1 (`\x01`).  
This means you can match specific input or output from a certain character using a regex tied to that separation character.

For example, to match only the user saying "hello", you could use the following regex:

```js
/\x01{{user}}:[^\x01]*?hello/
```

##### Key Input

There are two modes to enter keywords, each with a slightly different UI. In ⌨️ *plaintext mode* (default), keys can be entered as a comma-separated list in a single text field. Regexes can be included too, but they don't have any special highlighting. In ✨ *fancy mode*, the keys appear as separate elements and regexes will be highlighted as such. The control supports editing and deleting keys. The mode can be switched via the inline button inside the input control.

#### Optional Filter

Comma-separated list of additional keywords in conjunction with the primary key.
If no arguments are provided, this flag is ignored.
Supports logic for AND ANY, NOT ANY, or NOT ALL

1. AND ANY = Activates the entry only if the primary key and Any one of the optional filter keys are in scanned context.
2. AND ALL = Activates the entry only if the primary key and ALL of the optional filter keys are present.
3. NOT ANY = Activates the entry only if the primary key and None of the optional filter keys are in scanned context.
4. NOT ALL = Prevents activation of the entry despite primary key trigger, if all of the optional filters are in scanned context.

These keys also support [regex](#regular-expression-regex-as-keys).

#### Entry Content

The text that is inserted into the prompt upon entry activation.

#### Insertion Order

Numeric value. Defines a priority of the entry if multiple were activated at once. Entries with larger order numbers will be inserted closer to the end of the context as they will have more impact on the output. For example, an entry with Order number 100 will appear in the context before an entry with Order number 250.

#### Insertion Position

* **Before Char Defs:** World Info entry is inserted before the character's description and scenario. Has a moderate impact on the conversation.
* **After Char Defs:** World Info entry is inserted after the character's description and scenario. Has a greater impact on the conversation.
* **Before Example Messages:** The World Info entry is parsed as an example dialogue block and inserted before the examples provided by the character card.
* **After Example Messages:** The World Info entry is parsed as an example dialogue block and inserted after the examples provided by the character card.
* **Top of AN:** World Info entry is inserted at the top of Author's Note content. Has a variable impact depending on the Author's Note position.
* **Bottom of AN:** World Info entry is inserted at the bottom of Author's Note content. Has a variable impact depending on the Author's Note position.
* **@ D:** World Info entry is inserted at a specific depth in the chat (Depth 0 being the bottom of the prompt).
  * ⚙️ - as a system role message
  * 👤 - as a user role message
  * 🤖 - as an assistant role message
* **Outlet:** World Info entry is not injected automatically. Instead, its content is stored under a named outlet so you can decide exactly where it appears in the prompt by calling it with the [`{{outlet::Name}}` macro](#outlet-name).

Example Message entries will be formatted according to the prompt-building settings: Instruct Mode or Chat Completion prompt manager. They also follow the Example Messages Behavior rules: being gradually pushed out on full context, always kept, or disabled altogether.

If your Author's Note is disabled (Insertion Frequency = 0), World Info entries in A/N positions will be ignored!

#### Outlet Name

When the **Outlet** insertion position is selected, an additional **Outlet Name** field becomes available for the entry. The name that you provide here groups entries together and defines the token that you will use to pull them into the prompt manually.

Use the `{{outlet::YourName}}` macro in the [Prompt Manager](./Prompts/prompt-manager.md) or [Advanced Formatting](./Prompts/advancedformatting.md) prompt fields. When the prompt is built, the macro is replaced with the combined content of every World Info entry that shares the same outlet name, separated by newlines, sorted by their [Insertion Order](#insertion-order) value.

If an outlet entry is missing a name it will be skipped during generation, so make sure to fill in the field. Outlet names support autocomplete based on the names you have already used to make it easy to reuse consistent labels.

##### Limitations and caveats

* Placing outlet macros inside World Info entries is not supported and will not work. This conflicts with the evaluation order of World Info and may lead to infinite loops.
* Nesting outlets is not supported. You cannot place an outlet macro inside another outlet's content. Same as above, this may lead to infinite loops.
* Character card fields (Description, Personality, Scenario, etc.) cannot expand outlets. Those fields are parsed early so they can act as [additional matching sources](#additional-matching-sources) for World Info triggers, which means outlets are not available when their text is processed. Use another macro-aware field if you need to place outlet content in the prompt body instead.
* The Author's Note editor also cannot resolve outlets. To place outlet content around the Author's Note, assign the entries to **Top of AN** or **Bottom of AN** insertion positions instead of relying on the macro.
* Outlet names are case-sensitive. The `{{outlet::}}` macro must use exactly the same capitalization as the entry's **Outlet Name**, otherwise no content is returned.
* Leading or trailing spaces in an outlet name are ignored when you call the macro, so names saved with extra whitespace will not match. Avoid padding names so they can be resolved correctly.
* Outlet macros that have no content assigned to them will be replaced with an empty string.

#### Entry Title / Memo

A text field for your convenience to label your entries, which is not utilized by the AI or any of the trigger logics.

If empty, can be backfilled using the entries' first key by clicking on the "Fill empty memos" button.

#### Strategy

1. 🔵 (Blue Circle) = The entry would always be present in the prompt.
2. 🟢 (Green Circle) = The entry will be triggered only in the presence of the keyword.
3. 🔗 (Chain Link) = The entry is allowed to be inserted by embedding similarity.

Each Entry also has a toggle that allows you to enable or disable the entry.

#### Probability (Trigger %)

This value acts like an additional filter that adds a chance for the entry NOT to be inserted when it is activated by any means (constant, primary key, recursion).

1. Probability = 100 means that the entry will be inserted on every activation.
2. Probability = 50 means that the entry will be inserted with a 1:1 chance.
3. Probability = 0 means that the entry will NOT be inserted (essentially disabling it).

Use this to create random events in your chats. For example, every message could have a 1% chance of waking up an Elder God if its name is mentioned in the message.

#### Inclusion Group

Inclusion groups control how entries are selected when multiple entries with the same group label are triggered simultaneously. If multiple entries having the same group label were activated, only one will be inserted into the prompt.

By default, the chosen entry is selected randomly based on their Group Weight (default is 100 points) — the higher the number, the higher the probability of selection. This allows for a random selection among the triggered entries, adding an element of surprise and variety to interactions.

A single entry can be part of multiple inclusion groups if they are defined as a comma-separated list. The same logic as explained above will apply. If that entry is triggered, it will *disable* all other entries that are part of any of its groups. Therefore, if any of the groups are activated, this entry will not be activated.

#### Prioritize Inclusion

To provide more control over which entries are activated via [Inclusion Group](/Usage/worldinfo.md#inclusion-group), you can use the 'Prioritize Inclusion' setting. This option allows you to specify deterministically which entry to choose instead of randomly rolling Group Weight chances.

If multiple entries having the same group label and this setting turned on were activated, the one with the highest 'Order' value will be selected. This is useful for creating fallback sequences via inclusion groups. For example to prioritize low-depth entries with more emphasis, or to choose a specific instruction on setting the scene over another if both are valid.

#### Use Group Scoring

When this setting is enabled globally or per entry, the number of activated entry keys determines the group winner selection. Only the subset of a group with the highest number of key matches will be left to be activated by Group Weight or Inclusion Priority - the rest will be deactivated and removed from the group.

Use this to give more specificity for individual entries in large groups. For example, they can have a common key and a specific key. A random entry will be inserted when a specific key is not provided, and vice versa.

The score calculation logic for primary keys is 1 match = 1 point.

For secondary keys, the interaction depends on the chosen Selective Logic:

1. AND ANY: 1 secondary match = 1 point.
2. AND ALL: 1 point for every secondary key if they all match.
3. NOT ANY and NOT ALL: no change.

Example:

* Entry 1. Keys: song, sing, Black Cat. Group: songs
* Entry 2. Keys: song, sing, Ghosts. Group: songs

The input `sing me a song` can activate either entry (both activated 2 keys), but `sing me a song about Ghosts` will activate only Entry 2 (activated 3 keys).

#### Automation ID

Allows to integrate World Info entries with [STscripts](/For_Contributors/st-script.md) from Quick Replies extension. If both the quick reply command and the WI entry have the same Automation ID, the command will be executed automatically when the entry with a matching ID is activated.

Automations are executed in the order they are triggered, adhering to your designated sorting strategy, combining the [Character Lore Insertion Strategy](#character-lore-insertion-strategy) with the 'Priority' sorting. Which leads to [Blue Circle](#strategy) entries processed first, followed by others in their specified 'Order'. Recursively triggered entries will be processed after in the same order.

The script command will run only once if multiple entries with the same Automation ID are activated.

#### Character Filter

A list of character names for which this entry can be activated. If this list is not empty, the entry will only be activated for characters whose names are on the list. When a tag is selected, the entry will only be activated for characters that have that specific tag.

"Exclude" mode inverts the filter, meaning that the entry will be activated for all characters except those that are added to the list or that have the selected tag(s).

#### Triggers

The generation types for which this World Info entry can be activated. If nothing is selected, the entry can be activated for all generation types. If one or more are selected, the entry will only be activated for those specific generation types:

* **Normal:** Regular message generation request.
* **Continue:** When the Continue button is pressed.
* **Impersonate:** When the Impersonate button is pressed.
* **Swipe:** When the generation is triggered by swiping.
* **Regenerate:** When the Regenerate button is pressed in solo chats.
* **Quiet:** Background generation requests, usually triggered by [extensions](/extensions/index.md) or [STscript](/For_Contributors/st-script.md) commands.

!!!
The "Regenerate" trigger is not available in group chats as it uses different regeneration logic: all messages from the last reply are deleted, and messages are queued using the "Normal" generation type according to the chosen [Group reply strategy](/Usage/Characters/groupchats.md#reply-order-strategies).
!!!

#### Additional matching sources

By default World Info Entries are matched only against content from the current conversation. These options allow you to match the entry against different character information that does not show up in the chat, or even persona information. This is useful when you want to have a wide range of entries that are to be used between several characters but don't want to have to manage large lists of tags, or don't want to have to update character filter lists every time you create a new one. This also allows you to match entries based on the persona you have active.

* **Character Description**: Matches against the character description.
* **Character Personality**: Matches against the character personality summary, found under Advanced Definitions.
* **Scenario**: Matches against the character specified scenario, found under Advanced Definitions.
* **Persona Description**: Matches against the current selected persona's description.
* **Character's Note**: Matches against the character's note, which can be found under Advanced Definitions.
* **Creator's Notes**: Matches against the character creator's notes, which can be found under Advanced Definitions. The creator's notes are usually not included in the prompt.

## Vector Storage Matching

The Vector Storage extension provides an alternative to keyword matching by using the similarity between the recent chat messages and World Info entry contents.

To enable and use this, the following prerequisites need to be met:

1. Vector Storage extension is enabled and is configured to use one of the available embedding sources.
2. The "Enable for World Info" checkbox is ticked in the Vector Storage extension settings.
3. Either the World Info entries that are allowed for keyless matching have the "Vectorized" (🔗) status or the "Enabled for all entries" option is checked in the Vector Storage settings.

The choice of the vectorization model in the extension and the theoretical meaning behind the term "embeddings" won't be covered here. Check out the [Data Bank](/Usage/Characters/data-bank.md#vector-storage) guide if you require more info on this topic.

Vector Storage matching adheres to this set of rules:

* The maximum number of entries that are allowed to be matched with the Vector Storage can be adjusted with the "Max Entries" setting. This number only sets the limit and does not influence the token budget set in the activation settings for World Info. All of the budgeting rules still apply.
* This feature only replaces the check for keywords. All additional checks must be met for the entry to be inserted: trigger%, character filters, inclusion groups, etc.
* The "Scan Depth" setting from Activation Settings or entry overrides is not used. The Vector Storage "Query messages" value is utilized instead to get the text to match against. This allows for a configuration like "Scan Depth" set to 0, so no regular keyword matches will be made, but entries still can be activated by vectors.
* A "Vectorized" status is only an additional marker. The entry would still behave like a normal, enabled, non-constant record that will be activated by keywords if they are set. Remove the keywords if you want them to be activated only by vectors.

!!!info Note
Since the retrieval quality depends entirely on the outputs of the embedding model, it's impossible to predict exactly what entries will be inserted. If you want deterministic and predictable results, stick to keyword matching.
!!!

## Timed Effects

Usually, World Info evaluation is stateless, meaning that the result of the evaluation is the same, only depending on the current chat context. However, with the introduction of Timed Effects, you can create entries that have an activation delay, stay active after being triggered, or can't be triggered after the activation.

### Timed Effects Rules

1. The time frames for the effects are measured in messages (not pairs of messages/exchanges), with 0 meaning there is no effect.
2. Effects only apply in the chat where the entry was activated. Branches inherit the state of the parent chat.
3. Active timed effects are removed if the chat doesn't advance, e.g. if the last message was swiped or deleted.
4. Making any changes to the entry that is currently on timed effect will cause the effect to be forcibly removed.
5. Consequent triggering of keywords does not refresh the effect duration if it's already active.

### Types of Timed Effects

1. Sticky - the entry stays active for N messages after being activated. Stickied entries ignore probability checks on consequent scans until they expire.
2. Cooldown - the entry can't be activated for N messages after being activated. Can be used together with sticky: the entry goes on cooldown when the sticky duration ends.
3. Delay - the entry can't be activated unless there are at least N messages in the chat at the moment of evaluation.
    * Delay = 0 -> The entry can be activated at any time.
    * Delay = 1 -> The entry can't be activated if the chat is empty (no greeting).
    * Delay = 2 -> The entry can't be activated if there is zero or only one message in the chat, etc.

### Timed Effects Example

Entry configuration: sticky = 3, cooldown = 2, delay = 2.

```txt
Message 0: delay
Message 1: entry activated
Message 2: sticky
Message 3: sticky
Message 4: sticky
Message 5: cooldown
Message 6: cooldown
Message 7: entry can be activated again
```

## Activation Settings

Collapsible menu at the top of the World Info screen.

### Scan Depth

> Can be overridden on an entry level.

Defines how many messages in the chat history should be scanned for World Info keys.

* If set to 0, then only recursed entries and Author's Note are evaluated.
* If set to 1, then SillyTavern only scans the last message.
* 2 = two last messages, etc.

### Include Names

Defines if the names of the chat participants should be included in the scanned text buffer as message prefixes. This allows activating entries that use names as keywords without directly mentioning the names in messages.

See an example of the text to be scanned below, assuming the chat participants are named Alice and Bob.

Enabled (default):

```txt
Alice: Hello! Good to see you.
Bob: How is the weather today?
```

Disabled:

```txt
Hello! Good to see you.
How is the weather today?
```

### Context % / Budget

**Defines how many tokens could be used by World Info entries at once.**
You can define a threshold relative to your API's max-context settings (Context %) or an objective token threshold (Budget)

If the budget is exhausted, then no more entries are activated even if the keys are present in the prompt.

Constant entries will be inserted first. Then entries with larger order numbers.

Entries inserted by directly mentioning their keys have higher priority than those that were mentioned in other entries' contents.

### Min Activations

**This setting is mutually exclusive with Max Recursion Steps.**

Minimum Activations: If set to a non-zero value, this will disregard the limitation of "scan-depth", seeking all of the chat log backward from the latest message for keywords until as many entries as specified in min activations have been triggered. This will still be limited by the Max Depth setting or your overall Budget cap.

*Additional scan sweeps triggered by Min Activations will not check entries added by recursion on previous steps. Only chat messages and extension prompts can trigger these additional activations. However, the entries activated by Min Activations can trigger other entries as usual.*

### Max Depth

Maximum Depth to scan for when using the Min Activations setting.

### Recursive scanning

Recursive scanning allows for entries to activate other entries or be activated by others, enabling complex interactions and dependencies between different World Info entries. This feature can significantly enhance the dynamic nature of your creative scenarios.  
Whether recursive scanning is enabled can be controlled with the global setting **Recursive Scan**.  
There are three options available to control recursion for each entry:

8 **Non-recursable**: When this checkbox is selected, the entry will not be activated by other entries. This is useful for static information that should not change or be influenced by other world info entries.
  
* **Prevent further recursion**: Selecting this option ensures that once this entry is activated, it will not trigger any other entries. This is helpful to avoid unintended chains of activations.

* **Delay until recursion**: This entry will only be activated during recursive checks, meaning it won't be triggered in the initial pass but can be activated by other entries that have recursion enabled. Now, with the added **Recursion Level** for those delays, entries are grouped by levels. Initially, only the first level (smallest number) will match. Once no matches are found, the next level becomes eligible for matching, repeating the process until all levels are checked. This allows for more control over how and when deeper layers of information are revealed during recursion, especially in combination with criteria as NOT ANY or NOT ALL combination of key matches.

**Entries can activate other entries by mentioning their keywords in the content text.**

For example, if your World Info contains two entries:

```txt
Entry #1
Keyword: Bessie
Content: Bessie is a cow and is friends with Rufus.
```

```txt
Entry #2
Keyword: Rufus
Content: Rufus is a dog.
```

**Both** of them will be pulled into the context if the message text mentions **just Bessie**.

### Max Recursion Steps

**This setting is mutually exclusive with Min Activations.**

When set to zero, recursion nesting is only limited by your prompt budget. When set to a non-zero value, limits the total number of scan sweeps to desired maximum "nesting level".

Example values:

* 1 effectively disables recursion as the check stops after the first step.
* 2 can only activate recursive entries once.
* 3 can trigger recursion twice...

### Case-sensitive keys

> Can be overridden on an entry level.

**To get pulled into the context, entry keys need to match the case as they are defined in the World Info entry.**

This is useful when your keys are common words or parts of common words.

For example, when this setting is active, keys 'rose' and 'Rose' will be treated differently, depending on the inputs.

### Match whole words

> Can be overridden on an entry level.

Entries with keys containing only one word will be matched only if the entire word is present in the search text. Enabled by default.

For example, if the setting is enabled and the entry key is "king", then text such as "long live the king" would be matched, but "it's not to my liking" wouldn't.

**Important:** this setting can have a detrimental effect when used with languages that don't use whitespace to separate words (e.g. Japanese or Chinese). If you write entries in these languages, it is advised to keep it off.

### Alert on overflow

Shows an alert if the activated World Info exceeds the allocated token budget.
