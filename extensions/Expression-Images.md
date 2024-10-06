# Expression Images

### What is it?

Expression images are images (aka 'sprites') of your AI character which are shown next to (or behind) the chat window.

Expression images can use a classification model running alongside SillyTavern's main application. This allows the Expressions to change automatically based on the sentiment expressed in the AI's most recent chat response.

### Setup Instructions (Offline Mode without Extras)

1. Open the Extensions Panel and expand the 'Expression images' section. If you have the character chat open, you will see a grid of image placeholders.
![Expression Drawer](/static/extensions/expression-drawer.png)

2. Click the 'import' button at the top left of each image in the grid, and select the image you want to apply to that emotion. This will save the image with the correct filename inside the `/data/<user-handle>/characters/(character_name_here)/` folder.

3. To show the image in your SillyTavern window, click the image in the grid after it has been uploaded.

### How does the classify module work?

The `classify` module uses a small 'sentiment parsing' model that runs on the SillyTavern host machine (eg. on your PC, or the colab machine). This model takes the new output from the AI and detects what kind of sentiment, or emotion, the text is expressing. While multiple sentiments may be expressed in a single message, the model only picks the most likely one and returns that to the SillyTavern. The frontend plugin then displays the image that is associated with that sentiment.

### Change Expressions Manually

1. Click on any of the uploaded expression images (sprites) to display them near the chat interface (with default UI mode) or at the center of the screen (in Visual Novel mode).
2. Use the `/emote (name)` slash command or matching Quick Reply to set the sprite without opening the extensions menu.

### Setup Instructions (local classification)

1. Make sure you're on the latest version of SillyTavern.
2. Open the extensions panel and expand the "Character Expressions" plugin menu.
3. Select "Local" in the classification source dropdown.
4. This will start a one-time download of the classification model from HuggingFace Hub (about ~100 Mb).
5. Generate any message to verify that the classification works and the sprite appears. You may also check the server console for debug logs.

### Setup Instructions (with Extras)

1. Have Extras installed and running with the `classify` module enabled: `python server.py --enable-modules=classify`
2. Import the expression images the same way as mentioned above.
3. Select "Extras" in the classification source dropdown.
4. The appropriate expression image will display automatically whenever the AI sends you a response.

### Setup Instructions (with LLM)

1. Connect to any of the supported and properly configured Text Generation APIs.
2. Import the expression images the same way as mentioned above.
3. Select "LLM" in the classification source dropdown.
4. Optionally, configure the classification instruction prompt.
5. Generate any message to verify that the classification works and the sprite appears. You may also check the server console for debug logs.

### How do I get more expression options?

#### Local

Local classification defaults to 28 possible image labels: [Cohee/distilbert-base-uncased-go-emotions-onnx](https://huggingface.co/Cohee/distilbert-base-uncased-go-emotions-onnx)

To use the 6-option classification model, open the `config.yaml` file with a text editor and change the value of the `classificationModel` variable to [Cohee/bert-base-uncased-emotion-onnx](https://huggingface.co/Cohee/bert-base-uncased-emotion-onnx)

After that, restart the ST server to redownload the model. To revert it, change the value in `config.yaml` again.

#### Extras API

Extras API uses a classification model with 6 options by default: [nateraw/bert-base-uncased-emotion](https://huggingface.co/nateraw/bert-base-uncased-emotion)

There is also a model with 28 options: [joeddav/distilbert-base-uncased-go-emotions-student](https://huggingface.co/joeddav/distilbert-base-uncased-go-emotions-student)

To use this model you need to change your Extras command line to include the following argument (with a space before and after):

`--classification-model=joeddav/distilbert-base-uncased-go-emotions-student`

### What image formats are supported for Expressions?

Any image format is allowed, including webp and animated gifs.

The most common format is a PNG file with a transparent background.

### Using the 'default expressions'

!!! warning
This feature will only activate if local classification is enabled or with Extras API connected. There is no way to manually display default expression images.
!!!

![Use Default Expressions](/static/extensions/expression-default.png)

If you don't have custom expression images for a character, you can use the built-in default expressions which are included with the base SillyTavern installation. These are simple emoji-style images. Just click the checkbox at the top of the Expression images section inside the Extension panel. Default expressions will work alongside any available custom Expressions, and will display in cases when your custom image set has any missing images for a particular emotion.

### Importing an Expression images zip file

Using the 'import ZIP' button, you can import a zip file that contains a collection of expression images, and those images will automatically be added to the correct folder for your **currently selected character**. The zip file must have a flat internal structure (no subfolders) and the individual images should be named correctly. Importing a zip will not automatically rename any images to make them match the emotions.

### Limitations

#### Display names (not character card filenames) dictate which image set is used

If you have more than one character with the same display name, they will both use the same set of expression images.

If you want a different image set to be used for each version of the same-named character, you can use the sprites folder override.

#### How to set an override

1. Create a folder in the `/data/<user-handle>/characters` with any name and put images there, e.g. `/data/<user-handle>/characters/Boris`.
2. Open the chat with the character whose sprites you'd like to override.
3. Enter the name of the override folder into the "Sprite Folder Override" input and click "Submit".
4. The Sprites list will reload and the "Sprite set" indicator should show the override folder.
5. Alternatively, you can use the `/costume` slash command to achieve the same result: `/costume Boris`.
6. By prepending a backslash to the override folder name, it will resolve to a subfolder in the current character sprites folder, e.g. `/costume \tracksuit` for the character named Boris will resolve to the `/data/<user-handle>/characters/Boris/tracksuit` folder.

#### Custom expressions

1. You can add your custom expression labels to the list and upload sprites for them.
2. **However**, they won't be labeled automatically by the classification model. You *need* to set them manually (either by clicking or the `/emote` command).
