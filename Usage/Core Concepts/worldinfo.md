# World Info

**World Info (also known as Lorebooks or Memory Books) enhances AI's understanding of the details in your fictional world.**

It functions like a dynamic dictionary that only inserts relevant information from World Info entries when keywords associated with the entries are present in the message text.

The SillyTavern engine activates and seamlessly integrates the appropriate lore into the prompt, providing background information to the AI.

*It is important to note that while World Info helps guide the AI toward your desired lore, it does not guarantee its appearance in the generated output messages.*

### Pro Tips

* The AI does not insert keywords into context, so each World Info entry should be a comprehensive, standalone description.
* To create rich and detailed world lore, entries can be interlinked and reference one another.
* To conserve tokens, it is advisable to keep entry contents concise.
* The World Info engine is a very powerful prompt management tool. Don't fixate on adding character lore alone, feel free to experiment.

### Further reading

* [World Info Encyclopedia](https://rentry.co/world-info-encyclopedia): Exhaustive in-depth guide to World Info and Lorebooks. By kingbri, Alicat, Trappu.

## Character Lore

Optionally, one World Info file could be assigned to a character to serve as a dedicated lore source across all chats with that character (including groups).

To do that, navigate to a Character Management panel and click a globe button, then pick World Info from a dropdown list and click "Ok".

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

#### Optional Filter

A list of supplementary keywords that are used in conjunction with the main keywords. See [Optional Filter](#optional-filter-1).

#### Entry Content

The text that is inserted into the prompt upon entry activation.

#### Insertion Order

Numeric value. Defines a priority of the entry if multiple were activated at once. Entries with higher order numbers will be inserted closer to the end of the context as they will have more impact on the output.

#### Insertion Position

* **Before Char Defs:** World Info entry is inserted before the character's description and scenario. Has a moderate impact on the conversation.
* **After Char Defs:** World Info entry is inserted after the character's description and scenario. Has a greater impact on the conversation.
* **Top of AN:** World Info entry is inserted at the top of Author's Note content. Has a variable impact depending on the Author's Note position.
* **Bottom of AN:** World Info entry is inserted at the bottom of Author's Note content. Has a variable impact depending on the Author's Note position.
* **@ D:** World Info entry is inserted at a specific depth in the chat (Depth 0 being the bottom of the prompt).
    - ⚙️ - as a system role message
    - 👤 - as a user role message
    - 🤖 - as an assistant role message

If your Author's Note is disabled (Insertion Frequency = 0), World Info entries in A/N positions will be ignored!

#### Entry Title / Memo

A text field for your convenience to label your entries, which is not utilized by the AI or any of the trigger logics.

If empty, can be backfilled using the entries' first key by clicking on the "Fill empty memos" button.

#### Status

1. 🔵 (Blue Circle) = The entry would always be present in the prompt.
2. 🟢 (Green Circle) = The entry will be triggered only in the presence of the keyword.
3. 🔗 (Chain Link) = The entry is allowed to be inserted by embedding similarity.
4. ❌ (Red Cross) = The entry is disabled.

#### Vector Storage Matching

!!! warning Warning
This content describes a pre-release version and is subject to change.
!!!

The Vector Storage extension provides an alternative to keyword matching by using the similarity between the recent chats and World Info entry contents.

To enable and use this, the following prerequisites need to be met:

1. Vector Storage extension is enabled and is configured to use one of the available embedding sources.
2. The "Enable for World Info" checkbox is ticked in the Vector Storage extension settings.
3. Either the World Info entries that are allowed for keyless matching have the "Vectorized" (🔗) status or the "Enabled for all entries" option is enabled in the Vector Storage settings.

Vector Storage matching adheres to this set of rules:

- The maximum number of entries that are allowed to be matched with the Vector Storage can be adjusted with the "Max Entries" setting. This number only sets the limit and does not influence the token budget set in the activation settings for World Info. All of the budgeting rules still apply.
- This feature only replaces the check for keywords. All additional checks must be met for the entry to be inserted: trigger%, character filters, inclusion groups, etc.
- The "Scan Depth" setting from Activation Settings or entry overrides is not used, instead, the Vector Storage "Query messages" value is utilized to get the text to match against. This allows for a configuration like "Scan Depth" set to 0, so no regular keyword matches will be made, but entries still can be activated by vectors.
- A "Vectorized" status is only an additional marker. The entry would still behave like a normal, enabled, non-constant record that will be activated by keywords if they are set. Remove the keywords if you want them to be activated only by vectors.

!!! note Note
Since the retrieval quality depends entirely on the outputs of the embedding model, it's impossible to predict exactly what entries will be inserted. If you want deterministic and predictable results, stick to keyword matching.
!!!

#### Optional Filter

Comma-separated list of additional keywords in conjunction with the primary key.
If no arguments are provided, this flag is ignored.
Supports logic for AND ANY, NOT ANY, or NOT ALL

1. AND ANY = Activates the entry only if the primary key and Any one of the optional filter keys are in scanned context.
2. AND ALL = Activates the entry only if the primary key and ALL of the optional filter keys are present.
3. NOT ANY = Activates the entry only if the primary key and None of the optional filter keys are in scanned context.
4. NOT ALL = Prevents activation of the entry despite primary key trigger, if all of the optional filters are in scanned context.

#### Probability

This value acts like an additional filter that adds a chance for the entry NOT to be inserted when it is activated by any means (constant, primary key, recursion).

1. Probability = 100 means that the entry will be inserted on every activation.
2. Probability = 50 means that the entry will be inserted with a 1:1 chance.
3. Probability = 0 means that the entry will NOT be inserted (essentially disabling it).

Use this to create random events in your chats. For example, every message could have a 1% chance of waking up an Elder God if its name is mentioned in the message.

#### Inclusion Group

If multiple entries having the same group label were activated, only one will be inserted into the prompt. Entries are selected based on their Trigger% chance. The higher the number - the higher the probability.

**Grouped entries don't use Trigger% for the likelihood of their activation, only to determine the selection winner.**

#### Automation ID

Allows to integrate World Info entries with [STscripts](https://docs.sillytavern.app/usage/st-script/) from Quick Replies extension. If both the quick reply command and the WI entry have the same Automation ID, the command will be executed automatically when the entry with a matching ID is activated.

The script command will run only once if multiple entries with the same Automation ID are activated.

## Activation Settings

Collapsible menu at the top of the World Info screen.

### Scan Depth

> Can be overridden on an entry level.

Defines how many messages in the chat history should be scanned for World Info keys.

* If set to 0, then only recursed entries and Author's Note are evaluated.
* If set to 1, then SillyTavern only scans the last message.
* 2 = two last messages, etc.

### Context % / Budget

**Defines how many tokens could be used by World Info entries at once.**
You can define a threshold relative to your API's max-context settings (Context %) or an objective token threshold (Budget)

If the budget is exhausted, then no more entries are activated even if the keys are present in the prompt.

Constant entries will be inserted first. Then entries with higher order numbers.

Entries inserted by directly mentioning their keys have higher priority than those that were mentioned in other entries' contents.

### Min Activations

Minimum Activations: If set to a non-zero value, this will disregard the limitation of "scan-depth", seeking all of the chat log backward from the latest message for keywords until as many entries as specified in min activations have been triggered. This will still be limited by the Max Depth setting or your overall Budget cap.

### Max Depth

Maximum Depth to scan for when using the Min Activations setting.

### Recursive scanning

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

### Case-sensitive keys

> Can be overridden on an entry level.

**To get pulled into the context, entry keys need to match the case as they are defined in the World Info entry.**

This is useful when your keys are common words or parts of common words.

For example, when this setting is active, keys 'rose' and 'Rose' will be treated differently, depending on the inputs.

### Match whole words

> Can be overridden on an entry level.

Entries with keys containing only one word will be matched only if the entire word is present in the search text. Enabled by default.

For example, if the setting is enabled and the entry key is "king", then text such as "long live the king" would be matched, but "it's not to my liking" wouldn't.

### Alert on overflow

Shows an alert if the activated World Info exceeds the allocated token budget.
