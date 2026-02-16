---
route: /extensions/emulatorjs/
templating: false
---

# EmulatorJS

This extension allows you to play retro console games right from the SillyTavern chat.

## Installation

**Prerequisites:**

- Latest release version of SillyTavern.
- ROM files downloaded from the net. You can find them [anywhere](https://archive.org/details/ni-romsets).

**How to install:**

1. Install using SillyTavern's extensions downloader.
2. Or use this link: `https://github.com/SillyTavern/SillyTavern-EmulatorJS`

## Usage

- Open the "EmulatorJS" extension menu.
- Click "Add ROM file". ROMs are saved to your browser storage and not stored on a server.
- Select the game file to add. Input the name and core (if it wasn't auto-detected). If the core requires a BIOS file, add it too.
- Click the "Play" button in the list or launch via the wand menu.
- You can customize controls and other settings in the emulator frame after launching the game.
- Use save/load state functions if you need to take a break.

Check the EmulatorJS docs to see the list of available cores and their requirements: [Cores](https://emulatorjs.org/docs4devs/cores).

## Comments mode

With the power of multimodal models, your AI bots can see your gameplay and provide witty in-character comments.

### Requirements

1. A browser that supports [ImageCapture](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture#browser_compatibility). Tested on desktop Chrome. Firefox requires enabling it via config. Safari won't work.
2. Chat Completion API with image inlining mode is recommended. Check the API documentation to see if the chosen model supports multimodal prompts.
3. If image inlining is disabled, make sure that the [Image Captioning](./captioning.md#multimodal-source) extension is enabled, then select the "Multimodal" captioning source.

### How to enable comments

1. Make sure you set the interval of providing comments in the EmulatorJS extension settings. This setting defines how often the character is queried for comments using an image of your current gameplay. A value of 0 indicates that no comments are provided.
2. Select a character chat and launch the game. For the best performance, make sure that the ROM file is properly named so that AI can have more background context.
3. Start playing as you normally would. The vision model will be queried periodically to write a comment based on the latest screenshot it "sees".

### Settings

1. Caption template - a prompt used to describe the in-game screenshot.`{{game}}` and `{{core}}` additional macros are supported.
2. Comment template - a prompt used to write a comment based on the generated caption. `{{game}}`, `{{core}}`, `{{caption}}` additional macros are supported. For image inlining mode, `{{caption}}` is replaced with `see included image`.
3. Force captions - will force the use of multimodal captioning even if image inlining is supported and enabled.

### Why am I not seeing any comments?

Comments are temporarily paused (interval step skipped) if:

1. Emulator is paused (with a pause button, not in-game).
2. The browser window is out of focus.
3. The user input area is not empty. This is to let you type your reply in peace.
4. Another reply generation is currently in progress.
5. TTS voice is being read aloud. Comment is held off (30 seconds maximum) until it finishes, but not skipped.
6. A character card or group is currently open. Comment mode is disabled when starting the game from a welcome screen.

Other common issues:

1. Make sure you've set a commenting interval before launching the game.
2. Make sure you have set a multimodal API key and there are no errors in the ST server console.

Still doesn't work? Send us your browser debug console logs (press F12).

## Credits

- EmulatorJS engine (GPLv3): <https://github.com/EmulatorJS/EmulatorJS>
