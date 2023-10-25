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

### Other Group Chat menu options

#### Mute Character

The struck-out speech bubble icon next to the character avatar in the groupchat menu can disable or enable replies from a particular character in the chat.

#### Force Talk

The speech bubble icon icon next to the character avatar in the groupchat menu will trigger a reply from a particular character. 

#### Auto-mode

While auto-mode is enabled, the groupchat will follow the reply order and trigger responses

#### Allow Self Response

Will allow consecutive replies from the current character if they happen to be triggered due to the reply order selected.

#### Group Chat Scenario Override

Will override any selected scenarios for the existing characters.

#### Peek Character Definitions

Clicking on the character card icon next to the avatar in the group chat menu will quickly navigate to the usual character definitions screen. Any changes made here will be saved to the card itself.

#### Member Management

Any of your existing characters can be added, removed or re-ordered within the groupchat. By default a new added member is added to the "Top" of the group, this can be re-ordered using the arrow icons.

#### Group-Chat pop-out

The group-chat menu pop-out can be activated by clicking on the icon next to the "Current Members" field. This creates a popout of the groupchat menu. By enabling MovingUI from user settings, this menu can resized and dragged to any position within the interface, and functions just like the group-chat menu.
