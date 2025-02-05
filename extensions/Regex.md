# RegEx

## What is it?

The RegEx extension lets the user automatically detect specific patterns in a strings of text (called 'sequences') and apply manipulations to them. It can be a powerful tool when used in conjuction with other SillyTavern features such as QuickReplies or STSCript, or simply a way to remove certain words from a chat.

**This document will not explain process of writing a RegEx sequence in depth. There are many online resources to assist you with that.**

- [https://regexr.com](https://regexr.com)

- [https://regex101.com](https://regex101.com)

- [https://en.wikipedia.org/wiki/Regular_expression](https://en.wikipedia.org/wiki/Regular_expression)

## Prerequisites

- Install the "RegEx" extension from the "Download Extensions & Assets" menu in the Extensions panel (stacked blocks icon).

## Common Use Cases

RegEx is often used to apply a find-replace function on certain words in the chat, to add markdown styles to certain words or sentence types, or to return a boolean value to an STScript.

## Script List

![RegEx Extension Script List](/static/extensions/regex-listview.png)

- The buttons at the top are used to make a new script.
  - 'Global' scripts will apply to all characters and will be saved into `settings.json`.
  - 'Scoped' scripts will only apply to the currently active character, and will be saved into the character card's data.
- 'Import' lets you import RegEx scripts which were exported from another instance of SillyTavern.

Below this is a list of your scripts with some action buttons.

- Drag handles (three horizontal bars to the left of the script name) let you drag/drop the scripts into any order you like.
- Primary on/off switch can be quickly toggled to enable or disable the script without changing anything else. Disabled scripts are shown with ~~strikethrough~~ styling. **If a script is disabled here, it will be untriggerable by a QuickReply or STScript.**
- 'Edit' (pencil) button will open the RegEx script editor.
- 'Move to scoped' (down arrow) will convert a global script to a scoped script and apply it to the current character. In reverse (up arrow), it would convert a scoped script to global.
- 'Export' will cause your browser to download an exported `.json` file of the Script, which can then be shared and imported into another instance of SillyTavern.
- 'Delete' (trashcan) deletes the script.

## RegEx Editor

![RegEx Editor](/static/extensions/regex-editor.png)

- **Name** : The label for the script shown on the extension's script list.

- **Find Regex** : This is the Regular Expression that is used to detect your targeted text pattern. This is usually the most complex part of any RegEx script, and is the easiest place to make mistakes. Refer to the links at the top of the page for information how to write a RegEx sequence.

- **Test Mode** : This will open a comparison view at the top of the editor. Type some text into the 'Input' box, and the results of your RegEx script will be shown in the Output box.

### Replace With

This what will replace the matched sequence. In a very simple example, if your 'Find Regex' is `apple`, and your 'Repalce With' is `orange`, all instances of the 'apple' would be automatically changed to 'orange' in any text where the script is applied.

The extension-specific macro `\{\{match\}\}\` in this box will insert the full matched sequence of text. This is commonly used to apply styles to specific words. Going back to the above example, if `**\{\{match\}\}**` were put into the 'Replace With' box instead, all occurences of the word 'apple' would be replaced with `**apple**`, which would apply the bold markdown style to it.

Variables such as $1, $2, $3 etc can be used to insert what are called 'Capture Groups'. These are substrings located in the text sequence matched by the 'Find Regex' sequence. **Note that using these variables requires the matching expression to contain sets of parentheses to define which part of the matched string counts as a captured group.** Refer to the links at the top for reference on how to set up Capture Groups.

- **Trim Out** : text put in this box will be removed from the matched text sequence before the 'Replace With' process is applied. For example, if our match was 'apple', and the Trim Out box contains 'le', then the letters 'le' would be removed first before the 'Replace With' process is applied. Since our 'Replace With' box contains `**\{\{match\}\}**` it would result in `**app**` being put in as the replacement for 'apple' (first 'le' is removed, and the remaining matched text is given the bold markdown style). Multiple trims can be applied by adding a newline between each string you want to remove.

- **Affects** : This list of checkboxes defines the text sources to which the RegEx script will be applied. If everthing here is unchecked the script will never activate during normal chatting, but it can still be activated via slash command or STScript.

- **Min/Max Depth** : How far back in the chat history to look for strings to match with. Leave both blank to apply the script to the whole chat.

- **Other Options** :
  - 'Disabled' prevents the script from running. This is used as an override to prevent the script from running when you simply don't want to change any of the values and/or don't want to disable it entirely via the switch on the script list (as doing so would prevent STScripts from triggering it).
  - 'Run on Edit' makes the script also run after a chat message has been edited. If this is unchecked, the contents of edited chat messages will not trigger the script.

- **Macros in Find Regex** : Select whether or not to replace macros (such as `{{user}}`, `{{char}}`, etc) that are present in the Find Regex box's sequence.
  - 'Don't Substitute' will cause any SillyTavern macros to be ignored so the RegEx script will treat them literally when searching.
  - 'Raw' will send in the value of hte macro verbatim.
  - 'Escaped' will add a RegEx escape slash `\` before each character to ensure they do not accidentally alter the overall RegEx sequence. This can be useful if you have certain special characters in the values of the macro.

### Ephemerality

By default (when neither box here is checked) a RegEx script will directly edit the text values stored inside the chat's JSONL file. This ensures both the outgoing prompt and the chat display will always contain the same values. However, these changes to the chat file are irreversible.

If you do not want this to happen, you can enable either of the checkboxes here to limit the RegEx script's affects to only the display or the outgoing prompt.

If only one of the boxes is checked, there will be no changes made to the chat file, but **only the checked item** will be get the changes. This means you will be seeing one thing, but the LLM will be seeing another. Use this carefully.

If both are selected, the script will function as normal in all ways EXCEPT it will not write any changes to the chat file.

## Advanced Use

While RegEx is commonly used as a simple Find/Replace tool, it can also be used in more complex ways.

For example the 'Replace With' box could include a set of CSS rules and HTML to add a specific styled HTML element into your chat whenever a certain word is found. This will require the `Show <tags> in responses` box to be unchecked in the User Settings panel.

The script can also be set to never trigger during normal use, but could be instead be triggered via slashcommand as part of a logic check inside an STScript. The 'Replace With' box would include a unique value the script recognizes to indicate if a logic check is true or false. This expands the utility of RegEx to the full capabilities of all slashcommands, allowing for truly unlimited levels of control and automation based on the contents of the chat.
