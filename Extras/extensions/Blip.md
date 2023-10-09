# Blip

This guide will walk you through setting up and customizing blip extension for your SillyTavern experience. This extension animate the text of messages with variable speed and play sound along the animation. You can use audio file or generate the sound.

## Prerequisites

Before you begin, ensure you've met the following prerequisites:

- Make sure you're on the `staging` branch of `sillytavern`.
- Go into Assets extension menu and install blip extension

## Blip global settings

1. **Blip user message**:
   - Need to be enable to play animation on user message.
   - A profile need to be set for the user or a the default profile will be used if it exist.

2. **Blip only for certain text**:
   - Only for text inside quotes.
   - Ignore everything inside asterisks.

3. **Automatic scroll down**:
   - If enable the chat will go down to follow the text animation, disable it if you wanna scroll freely during animation.

4. **Audio volume**
   - Can mute the audio if just the animation of the text is desired.
   - Can adjust the global volume of blip audio.

## Character animation/voice profile

You can save a profile for each character:
   - including user.
   - and an optional default profile that will be used when character have no profile. 

1. **Select the character to assign/update profile**:
   - If the character have a profile it will be loaded.
   - Selecting a character without profile assign the current parameters to it.
   - Any profile can be deleted using the remove button.

2. **Text animation settings**:
   - Set the text speed: the delay in milliseconds between each letter printed.
   - Min/max speed multiplier can be set for randomness.
   - Comma/phrase delay can be set to add a pause when special character are printed, can add more liveliness to animation. Audio is paused too in this case.

3. **Audio parameters**:
   - You can set a volume multiplier that will only affect this voice profile.
   - Audio speed is the delay between each blip sound, independant of text speed.

4. **Blip origin: Generated sound**:
   - Using the min/max frequency slider you can customize the blip sound played.
   - If min/max are different a random sound in this range is played each time.

5. **Blip origin: file**:
   - You can use audio file by downloading official ST blip assets.
   - Or put file directly into: `\SillyTavern\public\assets\blip`.

Thank you for following this guide! Your SillyTavern experience is now enriched with text animation and blip voices.
