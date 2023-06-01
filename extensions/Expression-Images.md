# Expression Images

## What is it? 

Expression images are images (aka 'sprites') of your AI character which are shown next to (or behind) the chat window. 

Expression images do not require Extras to work.

However, having Extras running with the `classify` module enabled allows the Expressions to change automatically based on the sentiment expressed in the AI's most recent chat response. 

## Setup Instructions (Offline Mode without Extras)

1. Open the Extensions Panel and expand the 'Expression images' section. You will see a grid of image placeholders. 
![ST-ExpressionDrawer](https://github.com/SillyTavern/SillyTavern/assets/124905043/a6883961-d15d-4998-9a28-8d0333d65f29)

2. Click the 'import' button at the top left of each image in the grid, and select the image you want to apply to that emotion. This will save the image with the correct filename inside the `/public/characters/(character_name_here)/` folder.

3. To show the image in your SillyTavern window, click the image in the grid after it has been uploaded.

## Setup Instructions (with Extras)

1. Have Extras installed and running with the `classify` module enabled: `python server.py --enable-modules=classify`

2. Import the expression images the same way as mentioned above. 

3. The appropriate expression image will display automatically whenever the AI sends you a response. 

## How does the classify module work?

The classify module uses a small 'sentiment parsing' model that runs on the SillyTavern Extras host machine (eg. on your PC, or on the colab machine). This model takes the new output from the AI and detects what kind of sentiment, or emotion, the text is expressing. While multiple sentiments may be expressed in a single message, the model only picks the most likely one and returns that back to the Extras server. The Extras server then displays the image that is associated with that sentiment. 

## What image formats are supported for Expressions? 

Any image format is allowed, including webp and animated gifs. 

The most common format is a PNG file with a transparent background.

## Using the 'default expressions'
*(This feature will only activate if Extras is running. There is no way to manually display default expression images.)*

![opera_xTyIQ3xsI7](https://github.com/SillyTavern/SillyTavern/assets/124905043/07438380-7547-43e3-b10d-da2f2bac26a7)

If you don't have custom expression images for a character, you can use the built-in default expressions which are included with the base SillyTavern installation. These are simple emoji style images. Just click the checkbox at the bottom of the Expression images section inside the Extension panel. Default expressions will work alongside any available custom Expressions, and will display in cases when your custom image set is any missing images for a particular emotion.


## Importing an Expression images zip file

Using the 'import ZIP' button, you can import a zip file which contains a collection of expression images, and those images will automatically be added into the correct folder for your **currently selected character**. The zip file must have a flat internal structure (no subfolders) and the individual images should be named correctly. Importing a zip will not automatically rename any images to make them match the emotions.

## Limitations

### Display names (not character card filenames) dictate which image set is used.
If you have more than one character with the same display name, they will both use the same set of expression images. 

If you want a different image set to be used for each version of the same-named character, you must change the display name of one of them, and provide the alternate expression images in a folder with the new display name.

