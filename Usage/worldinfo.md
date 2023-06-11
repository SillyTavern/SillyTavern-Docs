## World Info

**World Info enhances AI's understanding of the details in your world.**

It functions like a dynamic dictionary that only inserts relevant information from World Info entries when keywords associated with the entries are present in the message text.

The SillyTavern engine activates and seamlessly integrates the appropriate lore into the prompt, providing background information to the AI.

*It is important to note that while World Info helps guide the AI towards your desired lore, it does not guarantee its appearance in the generated output messages.*

### Pro Tips

* The AI does not insert keywords into context, so each World Info entry should be a comprehensive, standalone description.
* To create a rich and detailed world lore, entries can be interlinked and reference one another.
* To conserve tokens, it is advisable to keep entry contents concise, with a general recommended limit of 50 tokens per entry.

### World Info Entry

#### Key

A list of keywords that trigger the activation of a World Info entry. Keys are not case-sensitive by default (this is [configurable](#casesensitivekeys)).

#### Secondary Key

A list of supplementary keywords that are used in conjunction with the main keywords. See [Selective](#selective).

#### Entry Content

The text that is inserted into the prompt upon entry activation.

#### Insertion Order

Numeric value. Defines a priority of the entry if multiple were activated at once. Entries with higher order number will be inserted closer to the end of the context as they will have more impact on the output.

#### Insertion Position

* **Before Chara:** World Info entry is inserted before the character's description and scenario. Has moderate impact on the conversation.
* **After Chara:** World Info entry is inserted after the character's description and scenario. Has greater impact on the conversation.

#### Comment

A supplemental text comment for your convenience, which is not utilized by the AI.

#### Constant

If enabled, the entry would always be present in the prompt.

#### Selective

If enabled, the entry would only be inserted when both a Key **AND** a Secondary Key have been activated.

If no secondary keys provided, this flag is ignored.

### Scan Depth

Defines how many messages in the chat history should be scanned for World Info keys.

If set to 1, then SillyTavern only scans the message you send and the most recent reply.

This stacks up to 10 message pairs it total.

### Budget

**Defines how many tokens could be used by World Info entries at once.**

If the budget was exhausted, then no more entries are activated even if the keys are present in the prompt.

Constant entries will be inserted first. Then entries with higher order numbers.

Entries inserted by direct mentioning of their keys have higher priority than those that were mentioned in other entries contents.

### Recursive scanning

**Entries can activate other entries by mentioning their keywords in the content text.**

For example, if your World Info contains two entries:

```txt
Entry #1
Keyword: Bessie
Content: Bessie is a cow and is friend with Rufus.
```

```txt
Entry #2
Keyword: Rufus
Content: Rufus is a dog.
```

**Both** of them will be pulled into the context if the message text mentions **just Bessie**.

### Case-sensitive keys

**To get pulled into the context, entry keys need to match the case as they are defined in the World Info entry.**

This is useful when your keys are common words or parts of common words.

For example, when this setting is active, keys 'rose' and 'Rose' will be treated differently, depending on the inputs.

### Match whole words

Entries with keys containing only one word will be matched only if the entire word is present in the search text.

For example, if the setting is enabled and the entry key is "king", then text such as "long live the king" would be matched, but "it's not to my liking" wouldn't.
