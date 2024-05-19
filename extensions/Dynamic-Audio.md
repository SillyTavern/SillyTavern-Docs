# Dynamic Audio

This guide will walk you through setting up and customizing dynamic audio assets for your SillyTavern experience.

## Prerequisites

Before you begin, ensure you've met the following prerequisites:

- Make sure you're on the latest `staging` branch of SillyTavern.
- Install the "Dynamic Audio" extension from the "Download Extensions & Assets" menu in the Extensions panel (stacked blocks icon).

## Dynamic Audio Setup (Browser)

1. **Connect to the Assets Repository**:
   - Launch SillyTavern and navigate to **Extensions** > **Assets**.
   - Click on the "Connect" button to establish a connection to the official assets repository.
   - Download the desired audio assets, such as background music (BGM) or ambient sounds, that correspond to the backgrounds you intend to use.

2. **Enable Dynamic Audio Extension**:
   - In SillyTavern, go to **Extensions** > **Dynamic Audio**.
   - Enable the extension, unmute and adjust the volume of BGM and ambient sounds to your preference.
   - When bgm end another one will play randomly, click on loop button to keep current bgm playing
   - Click on roll button to pick another bgm randomly

3. **Expression based BGM**:
   - Enable expression BGM switch if you want bgm to follow character expression (require bgm in character folder see below).
   - Adjust the cooldown timer (in seconds) between BGM updates. Increase it if you find the BGM changes too frequently in group chats or when using character-specific BGM with emotion detection.

## Importing Music for Characters

To set up custom music for your characters' emotions, follow these steps:

1. **Navigate to Character Folder**:
   - Go to the characters folder, e.g., `\SillyTavern\data\<user-handle>\characters\Seraphina`.

2. **Create BGM Folder**:
   - Inside the character folder, create a subfolder named `bgm`.

3. **Import Emotion Music**:
   - Within the `bgm` folder, import the music files for each emotion. Supported audio extensions include `.mp3`, `.ogg`, and `.wav`.
   - Naming convention: `[emotion]_[number].mp3`, e.g., `anger_0.mp3`, `joy_0.mp3`.

4. **Multiple Tracks for Emotions**:
   - You can import multiple tracks for the same emotion by incrementing the number, e.g., `neutral_1.mp3`, `neutral_2.mp3`.

5. **Default Music Selection**:
   - When no emotion is detected, a random neutral track will play as the default. Emotions are detected similarly to updating sprites; refer to the [expression images documentation](https://docs.sillytavern.app/extras/extensions/expression-images/) for details.

## Changing Default BGM Music

If a character doesn't have custom BGM in their folder, a default track will play. Here's how you can change it:

1. **Navigate to BGM Folder**:
   - Go to the following folder: `\SillyTavern\data\<user-handle>\assets\bgm`.

2. **Replace/Add Music**:
   - Replace or add music files (`.mp3`, `.ogg`, `.wav`) to this folder.
   - These are the official audio assets downloaded using the assets extension.
   - One of these tracks will play randomly when no character-specific BGM is found (solo or group chat).

## Changing Ambient Sounds

Ambient sounds add depth to your scenes. Here's how you can customize them:

1. **Navigate to Ambient Folder**:
   - Go to the following folder: `\SillyTavern\data\<user-handle>\assets\ambient`.

2. **File Naming Convention**:
   - Ambient audio filenames correspond to background image filenames, replacing spaces with dashes.
   - Example: `"bedroom-clean.mp3"` corresponds to the "bedroom clean.jpg" background.
   - If the lock button is unlock the audio file corresponding to the background will play. Activating lock will keep current ambient playing.

3. **Custom Ambients**:
   - You can add your own ambient sounds for custom or existing backgrounds by following the same naming pattern.

Thank you for following this guide! Your SillyTavern experience is now enriched with dynamic audio.
