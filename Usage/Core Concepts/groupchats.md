# Group Chats

### Reply order strategies

Decides how characters in group chats are drafted for their replies.

#### Natural Order

Tries to simulate the flow of a real human conversation. The algorithm is as follows:

1. Mentions of the group member names are extracted from the last message in chat.

Only whole words are recognized as mentions! If your character's name is "Misaka Mikoto", they will reply only activate on "Misaka" or "Mikoto", but never to "Misa", "Railgun", etc.

Unless the "Allow Self Responses" setting is enabled, characters won't reply to mentions of their name in their own message!

2. Characters are activated by the "Talkativeness" factor.

Talkativeness defines how often the character speaks if they were not mentioned. Adjust this value on the "Advanced Definitions" screen in the character editor. Slider values are on a linear scale from **0% / Shy** (character never talks unless mentioned) to **100% / Chatty** (character always replies). The default value for new characters is 50% chance.

3. A random character is selected.

If no characters were activated at previous steps, one speaker is selected randomly, ignoring all other conditions.

#### List Order

Characters are drafted based on the order they are presented in the group members list. No other rules apply.

### Other Group Chat menu options

#### Mute Character

The struck-out speech bubble icon next to the character avatar in the group chat menu can disable or enable replies from a particular character in the chat.

#### Force Talk

The speech bubble icon next to the character avatar in the group chat menu will trigger a reply only from a particular character, bypassing the reply order strategy. It will work even if the group member is muted.

#### Auto-mode

While auto-mode is enabled, the group chat will follow the reply order and trigger the message generation without user interaction. The next auto-mode turn is triggered after a 5-second delay when the last drafted character sends its message. When the user starts typing into the send message text area, the auto-mode will be disabled, but already queued generations are not stopped automatically.

#### Allow Self Responses

Will allow consecutive replies from the character who sent the latest message of each turn if they happen to be triggered due to being self-mentioned when the Natural Order is selected.

#### Group Chat Scenario Override

Will override any selected scenarios for the existing characters.

#### Peek Character Definitions

Clicking on the character card icon next to the avatar in the group chat menu will quickly navigate to the usual character definitions screen. Any changes made here will be saved to the card itself.

To return back to the group chat, click the Group Name title link.

#### Member Management

Any of your existing characters can be added, removed, muted, or re-ordered within the group chat. By default, a new member is added to the top of the group members list and then can be re-ordered using the arrow icons.

#### Group Chat pop-out

The group chat menu pop-out can be activated by clicking on the icon next to the "Current Members" field. This creates a pop-out of the group chat menu. By enabling MovingUI from user settings, this menu can resized and dragged to any position within the interface and functions just like the regular group chat menu.
