---
order: 70
route: /usage/core-concepts/groupchats/
---

# Group Chats

## Reply order strategies

Decides how characters in group chats are drafted for their replies.

### Manual

You can select the character to reply manually from the menu or with the `/trigger` command. The selected group member will be the only one to reply. User messages won't trigger any replies automatically. Triggering a generation with an empty user input will trigger a random unmuted group member to reply.

### Natural Order

Tries to simulate the flow of a real human conversation. The algorithm is as follows:

1. Mentions of the group member names are extracted from the last message in chat.

    Only whole words are recognized as mentions! If your character's name is "Misaka Mikoto", they will reply only activate on "Misaka" or "Mikoto", but never to "Misa", "Railgun", etc.
    
    Unless the "Allow Self Responses" setting is enabled, characters won't reply to mentions of their name in their own message!

2. Characters are activated by the "Talkativeness" factor.

    Talkativeness defines how often the character speaks if they were not mentioned. Adjust this value on the "Advanced Definitions" screen in the character editor. Slider values are on a linear scale from **0% / Shy** (character never talks unless mentioned) to **100% / Chatty** (character always replies). The default value for new characters is 50% chance.

3. A random character is selected.

    If no characters were activated at previous steps, one speaker is selected randomly, ignoring all other conditions.

### List Order

Characters are drafted based on the order they are presented in the group members list. No other rules apply.

### Pooled Order

Activates one random character who hasn't spoken yet since the last user message. If all characters have spoken, selects one randomly until the next user message.

## Group generation handling mode

This setting decides how to handle the character information of the group chat members. No matter the choice, the group chat history is always shared between all the members.

### Swap character cards

Default mode. Every time the message is generated, only the character card information of the active speaker is included in the context.

### Join character cards

The information of all of the group members is combined into one joint prompt in their list order. This can help in cases when altering large chunks of the context is undesirable, e.g. with llama.cpp prompt caching.

This mode has two sub-modes (you must choose one):

* Include muted - muted characters will always be included into the joint prompt.
* Exclude muted - muted characters won't be included if they aren't the current speaker.

The following fields are being combined:

1. Description
2. Scenario, if not overridden for the chat
3. Personality
4. Message examples
5. Character notes / Depth prompts

**Important!** Please be aware that due to how the typical character card is structured, the use of this mode can lead to unexpected behavior, including but not limited to: characters being confused about themselves, having merged personalities, uncertain traits, etc.

### Join Prefix and Suffix

When 'Join character cards' is selected, all respective fields of the characters are being joined together. This means that in the resulting prompt all character descriptions will be joined to one big blob of text. If you want those fields to be separated, you can define a prefix and/or suffix.

These options support normal macros and will also replace \{\{char\}\} with the relevant character's name and \<FIELDNAME\> with the name of the part (e.g.: description, personality, scenario, etc.)

## Other Group Chat menu options

### Mute Character

The struck-out speech bubble icon next to the character avatar in the group chat menu can disable or enable replies from a particular character in the chat.

### Force Talk

The speech bubble icon next to the character avatar in the group chat menu will trigger a reply only from a particular character, bypassing the reply order strategy. It will work even if the group member is muted.

### Auto-mode

While auto-mode is enabled, the group chat will follow the reply order and trigger the message generation without user interaction. The next auto-mode turn is triggered after a 5-second delay when the last drafted character sends its message. When the user starts typing into the send message text area, the auto-mode will be disabled, but already queued generations are not stopped automatically.

### Allow Self Responses

Will allow consecutive replies from the character who sent the latest message of each turn if they happen to be triggered due to being self-mentioned when the Natural Order is selected. Has no effect on List order.

### Group Chat Scenario Override

All group members will use the entered scenario text instead of what is specified in their character cards. Branched chats inherit the scenario override from their parent and can be changed individually after that.

### Peek Character Definitions

Clicking on the character card icon next to the avatar in the group chat menu will quickly navigate to the usual character definitions screen. Any changes made here will be saved to the card itself.

To return back to the group chat, click the Group Name title link.

### Member Management

Any of your existing characters can be added, removed, muted, or re-ordered within the group chat. By default, a new member is added to the top of the group members list and then can be re-ordered using the arrow icons.

### Group Chat pop-out

The group chat menu pop-out can be activated by clicking on the icon next to the "Current Members" field. This creates a pop-out of the group chat menu. By enabling MovingUI from user settings, this menu can be resized and dragged to any position within the interface and functions just like the regular group chat menu.
