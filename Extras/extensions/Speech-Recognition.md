# Speech Recognition

Speech recognition transcribe your voice into text.

## Speech Recognition Setup (Browser)

PREREQUISITES:

- `sillytavern`: Switch to the `staging` branch

1. In a file browser navigate to **\SillyTavern-extras\data\models\rvc**, create a subfolder and put **.pth** and **.index** into the created folder.
2. In SillyTavern, go to **Extensions --> Speech Recognition** and select "Browser" from the drop down options.
- If your browser do not support voice recognition you will see an error popup, see other method below in this case.
3. Select the "Message Mode" you want from:
- Append: your message will be appended to the current user message text area.
- Replace: your message will replace the current user message in the text area.
- Auto send: your message will automatically be sent once end of talk is detected.
4. (optional) Setup phrases mapping, can be used as vocal shortcut.
- by adding "command delete = /del2" for example you can have the "/del2" command replace your voice message whenever "command delete" is detected in your voice message.
- Can be usefull when combine with auto send mode for full voice control.
- Need to be enabled by click the check box "enable messages mapping".
5. Choose the language you want to speak (not every browser handle every language, try and see).
6. To start recording click on the mic button on the right of the message area next to send button, click again to stop recording (it may stop on itself if it detect no more voice).

## Speech Recognition Setup (Whisper/Vosk)

PREREQUISITES:

- `sillytavern-extras`: Switch to the `neo` branch
- `sillytavern`: Switch to the `staging` branch

1. Enable the desired speech recognition provider on extras server.
```yaml
python server.py --enable-modules=whisper-stt
```
```yaml
python server.py --enable-modules=vosk-stt
```
2. In ST the settings are the same as for the "Browser" provider see above (beside the language option).
- Power user tips: you can use a custom model to be used by adding the option "--stt-vosk-model-path" or "--stt-whisper-model-path" with the path to the model.
Example:
```yaml
python server.py --enable-modules=whisper-stt --stt-whisper-model-path=path/to/whispermodel
```

## Speech Recognition Setup (Streaming)

PREREQUISITES:

- `sillytavern-extras`: Switch to the `neo` branch
- `sillytavern`: Switch to the `staging` branch
- This setup perform voice recording from the machine `sillytavern-extras` is running on.

1. Enable the module using the following option.
```yaml
python server.py --enable-modules=streaming-stt
```

2. (optional) you can specify a custom whisper model to be used as in Whisper setup above.
3. (optional) But strongly advised, setup some trigger words in ST. Only the messages starting with those trigger words will be sent to ST as actual message. Allow to avoid anything you say or noise to be transcripted as message. Need to be enable with the checkbox.
4. Other settings are the same as other providers.