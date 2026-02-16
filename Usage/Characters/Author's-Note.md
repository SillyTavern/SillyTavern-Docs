---
order: 50
route: /usage/core-concepts/authors-note/
---

# Author's Note

## What is it?

Author's Note is a powerful tool for customizing AI responses which inserts a section of text into the prompt at any position and at any frequency you desire.

## Usage

The Author's Note can be found in the Options menu on the left side of the chat input bar.

| Options Menu                          | Author's Note Panel                    |
|---------------------------------------|----------------------------------------|
| ![](/static/extensions/note-menu.png) | ![](/static/extensions/note-panel.png) |

## Configuring Author's Notes

### Chat-specific Author's Note

The box at the top of the Author's Note panel contains the Author's Note for your current chat.

**The Content of this box is not automatically transferred to any new chat.**

### Placement options

#### After Scenario

This places the Author's Note towards the top of the context after the 'Scenario' section of the Character Definition. If no scenario is specified, it will be placed after the last portion of the Character Definition, and before the Example messages.

#### In-chat

This places the Author's Note into the chat history at the specified depth.

Depth 0 = placed at the very end of the chat history.

Depth 4 = placed before the most recent 3 chat history messages, making it become the 4th entity in the chat history.

_The closer the Author's Note is to the bottom of the prompt, the more impact it has on the next AI response._

### Insertion Frequency

This is how often you want the Author's Note to be included in the chat.

Frequency 0 = Author's Note will never be inserted.

Frequency 1 = Author's Note will be inserted with every user input prompt.

Frequency 4 = Author's Note will be inserted into every 4th user input prompt.

### Default Author's Note

The box at the bottom of the panel contains the Default Author's Note which will be applied to each new chat.

## Common Use Cases

### Remind AI of response formatting

The Author's Note can be used to specify how the AI should write its responses.

- [Your next response must be 300 tokens in length.]
- [Write your next reply in the style of Edgar Allan Poe]
- [Use markdown italics to signify unspoken actions, and quotation marks to specify spoken word.]

### Reinforcing Instructions

- [Remember the instructions you were given at the beginning of this chat.]

### As temporary World Info, Character Bias, or Instruct for non-Instruct models

- [\{\{char\}\} is in the library]
- [\{\{user\}\} has a fresh wound to his leg, so won't be able to run away.]
- [\{\{char\}\} cannot speak and must communicate using hand signals.]
