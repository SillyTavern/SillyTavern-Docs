# Blip

This guide will walk you through setting up and customizing blip extension for your SillyTavern experience. This extension animate the text of messages with variable speed and play sound along the animation. You can use audio file or generate the sound.

## Prerequisites

Before you begin, ensure you've met the following prerequisites:

- Make sure you're on the latest `staging` branch of SillyTavern.
- Install the "Blip" extension from the "Download Extensions & Assets" menu in the Extensions panel (stacked blocks icon).

## Blip global settings

1. **Blip user message**:
   - Enable checkbox to play animation on user message.
   - Set a profile for the user or a default profile if you want blip animation for user.

2. **Blip only for certain text**:
   - Enable checkbox to only blip for text inside quotes.
   - Enable checkbox to ignore everything inside asterisks.

3. **Automatic scroll down**:
   - Enable checkbox to make the chat go down to follow the text animation, disable it if you wanna scroll freely during animation.

4. **Audio volume**
   - Mute the audio if just the animation of the text is desired.
   - You can adjust the global volume of blip audio.

## Character animation/voice profile

You can save a profile for each character:
   - including the user and an optional default profile that will be used when character have no profile. 
   - If only the current chat characters are shown in the list, click the checkbox to show all your characters.

1. **Select the character to assign/update profile**:
   - Select a character, if he have a profile it will be loaded.
   - If it does not have a profile yet the current parameters will become his profile settings.
   - Any profile can be deleted using the remove button.
   - Use refresh button if your character does not appear in the list.

2. **Text animation settings**:
   - Set the text speed: the delay in milliseconds between each letter printed.
   - Set Min/max speed multiplier different to 1.0 for randomness of speed animation.
   - Set comma/phrase delay superior to 0 to add a pause when special character are printed, can add more liveliness to animation. Audio is paused too in this case.

3. **Audio parameters**:
   - Set a volume multiplier that will only affect this voice profile if needed.
   - Set audio speed: the delay between each blip sound, independant of text speed.

4. **Blip origin: Generated sound**:
   - Use the min/max frequency slider to customize the blip sound played.
   - If min/max are different a random sound in this range is played each time.

5. **Blip origin: file**:
   - Choose a file in the list.
   - You can get official ST blip assets from the assets extension menu.
   - Or put file directly into: `\SillyTavern\data\<user-handle>\assets\blip`.
   - Enable the checkbox to force to wait entire file is played before playing again if needed.

# Remarks
- Blip animation can only start when full message is received, don't use streaming mode.

Thank you for following this guide! Your SillyTavern experience is now enriched with text animation and blip voices.
