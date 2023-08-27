# Speech Recognition

This guide will walk you through setting up speech recognition to transcribe your voice into text within SillyTavern.

## Prerequisites

Before you begin, ensure you've met the following prerequisites:

- Make sure you're on the `staging` branch of `sillytavern`.

## Speech Recognition Setup (Browser)

1. **Prepare Model Files**:
   - In a file browser, navigate to `\SillyTavern-extras\data\models\rvc`.
   - Create a subfolder and place the `.pth` and `.index` files into the created folder.

2. **Configure SillyTavern**:
   - Launch SillyTavern and go to **Extensions** > **Speech Recognition**.
   - Select "Browser" from the dropdown options.
   - If your browser doesn't support voice recognition, an error popup will appear.

3. **Select Message Mode**:
   - Choose the "Message Mode" you want:
     - **Append**: Your message will be appended to the current user message text area.
     - **Replace**: Your message will replace the current user message in the text area.
     - **Auto send**: Your message will automatically be sent once the end of speech is detected.

4. **Enable Message Mapping** *(Optional)*:
   - Setup phrases mapping for vocal shortcuts.
   - For instance, by adding "command delete = /del2", the "/del2" command will replace your voice message when "command delete" is detected.
   - Useful when combined with auto send mode for full voice control. Enable this by checking "Enable messages mapping".

5. **Select Language**:
   - Choose the language you want to speak (Note: not every browser supports all languages).

6. **Recording**:
   - To start recording, click the microphone button to the right of the message area next to the send button. Click again to stop recording. Recording may stop automatically if no voice is detected.

## Speech Recognition Setup (Whisper/Vosk)

1. **Enable Provider**:
   - Switch to the `neo` branch in `sillytavern-extras`.
   - Enable the desired speech recognition provider on the extras server using the following command:
     ```shell
     python server.py --enable-modules=whisper-stt
     ```
     or
     ```shell
     python server.py --enable-modules=vosk-stt
     ```

2. **Configure SillyTavern**:
   - In SillyTavern, the settings are similar to the "Browser" provider setup (except for language).
   - You can also use a custom model by adding the option `--stt-vosk-model-path` or `--stt-whisper-model-path` with the path to the model.

## Speech Recognition Setup (Streaming)

1. **Enable Provider**:
   - Switch to the `neo` branch in `sillytavern-extras`.
   - Enable the streaming speech recognition module with the following command:
     ```shell
     python server.py --enable-modules=streaming-stt
     ```

2. **Configure SillyTavern**:
   - (Optional) Specify a custom Whisper model as in the Whisper setup above.
   - (Optional but recommended) Set up trigger words in SillyTavern. Only messages starting with these trigger words will be sent to SillyTavern as actual messages. This prevents random speech or noise from being transcribed. Enable this with the checkbox.
   - Other settings are similar to other providers.

You're now ready to transcribe your voice into text using speech recognition in SillyTavern.
