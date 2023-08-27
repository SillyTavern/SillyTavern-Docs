# Dynamic Audio

To use officially suported assets go into 

## Dynamic Audio Setup (Browser)

PREREQUISITES:

- `sillytavern`: Switch to the `staging` branch
1. In SillyTavern, go to **Extensions --> Assets** and connect to the official assets repository by clicking on the connect button.
2. Click to download the desired audio assets like BGM or ambients corresponding to the background you want to use.
3. In SillyTavern, go to **Extensions --> Dynamic Audio** enable the extension and unmute the BGM and ambient, set the volume as you see fit.
4. (optional) You can change the cooldown timer (in seconds) between bgm update.
    - Increase it if you feel that bgm change to much in group chat or when using character custom bgm with emotion detection.

## Import music for character

1. Go to the characters folder and select your character:
    - example: \SillyTavern\public\characters\Seraphina
2. Make a folder called bgm
3. Inside the bgm folder you can import your music for emotions (It does support more audio extensions like .ogg .wav)
    - anger_0.mp3
    - fear_0.mp3
    - joy_0.mp3
    - love_0.mp3
    - sad_0.mp3
    - surprise_0.mp3
    - neutral_0.mp3

4. You can import more music but count up the numbers like:
    - neutral_1.mp3
    - neutral_2.mp3
    - neutral_3.mp3

A random neutral will play as default when no emotion is detected. Emotion are detected the same way as for updating sprites see https://docs.sillytavern.app/extras/extensions/expression-images/

## Change default bgm music
When the character has no custom bgm in its folder you will hear a default music as fallback. You can change this:

Go to the following folder:

\SillyTavern\public\assets\bgm

You will see the official audio assets you downloaded using the assets extension.
You can replace or add any other music you want (Supports .mp3 .ogg .wav).
One of them will play randomly when there is no bgm found for the current character speaker (solo or group chat).

## Change ambient
Go to the following folder:

\SillyTavern\public\assets\ambient

The ambient audio will play when a background name correspond to the audio file name with spaces replaced by "-" for example:

- "bedroom-clean.mp3" is for "bedroom clean.jpg" background
- "cityscape-medieval-market.mp3" is for "cityscape medieval market.jpg" background

You can add your own ambient for any custom or existing background by following this patern.