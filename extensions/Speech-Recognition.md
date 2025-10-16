---
route: /extensions/speech-recognition/
---

# Speech Recognition

This guide will walk you through setting up speech recognition to transcribe your voice into text within SillyTavern.

## Prerequisites

Before you begin, ensure you've met the following prerequisites:

- Make sure you're on the latest version of SillyTavern.
- Install the "Speech Recognition" extension from the "Download Extensions & Assets" menu in the Extensions panel (stacked blocks icon).
- Have ffmpeg binary installed. See [RVC setup](RVC.md#rvc-setup) for more details.

## Speech Recognition Setup (Browser)

1. **Configure SillyTavern**:
   - Launch SillyTavern and go to **Extensions** > **Speech Recognition**.
   - Select "Browser" from the dropdown options.
   - If your browser doesn't support voice recognition, an error popup will appear.

2. **Select Message Mode**:
   - Choose the "Message Mode" you want:
     - **Append**: Your message will be appended to the current user message text area.
     - **Replace**: Your message will replace the current user message in the text area.
     - **Auto send**: Your message will automatically be sent once the end of speech is detected.

3. **Enable Message Mapping** *(Optional)*:
   - Setup phrases mapping for vocal shortcuts.
   - For instance, by adding "command delete = /del2", the "/del2" command will replace your voice message when "command delete" is detected.
   - Useful when combined with auto send mode for full voice control. Enable this by checking "Enable messages mapping".

4. **Select Language**:
   - Choose the language you want to speak (Note: not every browser supports all languages).

5. **Recording**:
   - To start recording, click the microphone button to the right of the message area next to the send button. Click again to stop recording. Recording may stop automatically if no voice is detected.

## Speech Recognition Setup (Whisper/Vosk)

1. **Enable Provider**:
   - Enable the desired speech recognition provider on the extras server using the following command:
     ```shell
     python server.py --enable-modules=whisper-stt
     ```
     or
     ```shell
     python server.py --enable-modules=vosk-stt
     ```
   - You can also use a custom model by adding the option `--stt-vosk-model-path` or `--stt-whisper-model-path` with the path to the model.

2. **Configure SillyTavern**:
   - Launch SillyTavern and go to **Extensions** > **Speech Recognition**.
   - Select "Vosk" or "Whisper" from the dropdown options (whisper is more accurate).
   - The settings are similar to the "Browser" provider setup (except for language) see above.

## Speech Recognition Setup (Streaming)

1. **Enable Provider**:
   - Enable the streaming speech recognition module on Sillytavern-extras with the following command:
     ```shell
     python server.py --enable-modules=streaming-stt
     ```

2. **Configure SillyTavern**:
   - (Optional) Specify a custom Whisper model as in the Whisper setup above.
   - (Optional but recommended) Set up trigger words in SillyTavern. Only messages starting with these trigger words will be sent to SillyTavern as actual messages. This prevents random speech or noise from being transcribed. Enable this with the checkbox. The trigger words can be included/excluded from the actual message using a checkbox.
   - Other settings are similar to other providers.

You're now ready to transcribe your voice into text using speech recognition in SillyTavern.
