# Group Chats

### Reply order strategies

Decides how characters in group chats are drafted for their replies.

#### Natural order

Tries to simulate the flow of a real human conversation. The algorithm is as follows:

1. Mentions of the group member names are extracted from the last message in chat.

Only whole words are recognized as mentions! If your character's name is "Misaka Mikoto", they will reply only activate on "Misaka" or "Mikoto", but never to "Misa", "Railgun", etc.

Unless "Allow bot responses to self" setting is enabled, characters won't reply to mentions of their name in their own message!

2. Characters are activated by the "Talkativeness" factor.

Talkativeness defines how often the character speaks if they were not mentioned. Adjust this value on "Advanced definitions" screen in character editor. Slider values are on a linear scale from **0% / Shy** (character never talks unless mentioned) to **100% / Chatty** (character always replies). Default value for new characters is 50% chance.

3. Random character is selected.

If no characters were activated at previous steps, one speaker is selected randomly, ignoring all other conditions.

#### List order

Characters are drafted based on the order they are presented in group members list. No other rules apply.

### Mute Character

### Force Talk

### Auto-mode

### Allow Self Response

### Group Chat Scenario Override

### Peek Character Definitions

### Member Management
