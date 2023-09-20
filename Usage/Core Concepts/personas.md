# Persona Management

## What is a persona?

A persona in SillyTavern represents a combination of a user name and a picture that
create a unique identity for the character you play in your chats or identify yourself as.

**Creating user personas is optional, but it is a good way to automate routine actions.**

## How to create a persona?

To create a persona:

1. Open the Persona Management panel (smiley face icon) in the top bar.
2. Upload a user avatar, select an existing avatar, or create a blank persona with a dummy image ("Blank" button).
3. Hover (tap on mobile) over the image and pick the top-left option ("Bind a name to that avatar").
4. Input a desired name into the popup window. Confirm by clicking an "OK" button.
5. The avatar now represents a Persona. Click on the avatar to select it, and this will also automatically update the user name.

## Persona Description

The contents of this field represent any specific information that you want to bind to the selected persona.
It could be any kind of description of your user character: mental and physical traits, age, occupation, etc.
Here you can also use replacement macros like \{\{user\}\}, \{\{char\}\}, and other supported parameters.

If defined, the Persona Description will be inserted into the AI prompt in one of the chosen places:

1. In Prompt Manager (Chat Completions API) or Story String (other APIs). This is the default position.
2. Top of an Author's Note (if Author's Note is not disabled).
3. Bottom of an Author's Note (if Author's Note is not disabled).

**However, keep in mind that picking a persona when you have set user description on an unbound avatar will not keep your changes.**

To permanently save the user description, bind the currently selected avatar to a user name.

## Persona locking

One chosen persona could be locked to the currently open individual or group chats.
If the currently selected persona is different from the locked one, it will be automatically selected when you open that chat.

### To lock a persona

1. Open a chat (either group or individual).
2. Select a desired persona by clicking on its avatar.
3. Click the lock icon in the buttons row next to a name input.
4. To revert this action, click the unlock icon. Then persona won't be changed automatically when the chat is reopened.

Alternatively, you could use /lock and /unlock slash commands to achieve the same result.

## Default persona

You can select one persona to be your preferred default identity, selecting it automatically for all new chats and chats where a persona is not locked.
The default persona is represented by a yellow outline on the Persona Management panel. 

* To set persona as a default, hover (tap on mobile) over the persona avatar and click the top-right "crown" button, then confirm your action.
* To undo this action and unset the default persona, click the "crown" button again.

## Pro Tips

1. Switching user personas in chat does not automatically change the attribution of previously sent user messages. That way it is possible to role-play as multiple user characters at once.
2. You can attribute all user-sent messages in the currently open chat to a currently selected persona by clicking
the "sync" button on the Persona Management panel or by using the /sync slash command.
3. To change the persona image without deleting it, hover over the avatar on the Persona Management panel and click the bottom-left button.
Then choose a new image and it will be replaced, preserving your set description and chat lock states.
4. To quickly change a persona while in chat without opening the Persona Management panel, use the /persona slash command. For example, `/persona Blaze`.
