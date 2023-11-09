# Slash commands

!!!warning
**This is not an exhaustive list as it is updated rarely.**

For the most up-to-date list of commands that will work in your instance, use the `/help slash` chat command in any SillyTavern chat.
!!!

- /help – displays this help message (aliases: /?)
- /name (name) – sets user name and persona avatar (if set) (aliases: /persona)
- /sync – syncs user name in user-attributed messages in the current chat
- /bind – binds/unbinds a persona (name and avatar) to the current chat
- /bg (filename) – sets a background according to the filename, partial names allowed, will set the first one alphabetically if multiple files begin with the provided argument string (aliases: /background)
- /sendas – sends a message as a specific character.

Example:

```
/sendas Chloe
Hello, guys!
will send "Hello, guys!" from "Chloe".
Uses character avatar if it exists in the characters list.
```

- /sys (text) – sends message as a system narrator
- /sysname (name) – sets a name for future system narrator messages in this chat (display only). Default: System. Leave empty to reset.
- /note (text) – sets an author's note for the currently selected chat
- /depth (number) – sets an author's note depth for in-chat positioning
- /freq (number) – sets an author's note insertion frequency (aliases: /interval)
- /pos (chat or scenario) – sets an author's note position (aliases: /position)
- /roll (dice formula) – roll the dice. For example, /roll 2d6 (aliases: /r)
- /taskcheck – checks if the current task is completed
- /lock – locks a background for the currently selected chat
- /unlock – unlocks a background for the currently selected chat
- /autobg – automatically changes the background based on the chat context (aliases: /bgauto)
- /sd (argument) – requests SD to make an image. Supported arguments:

```
you – AI character full body selfie
face – AI character face-only selfie
me – user character full body selfie
scene – visual recap of the whole chat scenario
last – visual recap of the last chat message
raw_last – visual recap of the last chat message with no summary
```

Anything else would trigger a "free mode" to make SD generate whatever you prompted.
example: '/sd apple tree' would generate a picture of an apple tree.
