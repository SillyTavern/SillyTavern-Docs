# Extension-live2d

This guide will walk you through setting up and customizing Live2D extension for your SillyTavern experience. This extension allow to use a Live2D animated model for your character.You can customize several interactions depending on the model capacities.

## Prerequisites

Before you begin, ensure you've met the following prerequisites:

- Make sure you're on the latest `staging` branch of SillyTavern.
- Install the "Live2d" extension from the "Download Extensions & Assets" menu in the Extensions panel (stacked blocks icon).
- Put your live2d model folder into /public/assets/live2d folder. Should look like this:

![Asset folder example](https://raw.githubusercontent.com/SillyTavern/Extension-Live2d/main/readme_img/example_asset_folder.png)

- The model folder must include everything needed by the live2d model: expressions/motions/texture/sounds and settings file. This is the content of "shizuku" folder in this example:

![Live2d model folder example](https://raw.githubusercontent.com/SillyTavern/Extension-Live2d/main/readme_img/example_live2d_model_folder.png)

### Remarks
- The models can also be put into the character folder for example in /public/characters/Shizuku/live2d/. But those models will only be accessible for this character.

# Extension settings

![UI global settings](https://raw.githubusercontent.com/SillyTavern/Extension-Live2d/main/readme_img/ui_global.png)

## Global settings

1. **Enabled**:
   - Enable checkbox to activate the extension.
   - Disable the extension if you wanna move normal sprites in group chat and enable again.

2. **Follow cursor**:
   - Enable checkbox to make live2d model follow your cursor (if the model handle it).

3. **Auto-send interaction**:
   - Enable checkbox to automatically request character when clicking on area with mapped message (see hit areas section).

## Debug settings

1. **Reset model before animation**
   - Enable checkbox to reload the model before any animation. Will force the animation to start, allow spamming click. May be needed by some model where animations end in a state not compatible with the start of other animations.

2. **Show model frames**
   - Enable checkbox to show the model frame, make it easier to know where to click to drag the model around. Also show the hit area if there is any, overing mouse shows hit area name.

## Character selection

1. **Refresh button**
    - Click the refresh button to reload the list of character in current chat.

2. **Select character**
    - Use the drop down list to select a character you want to assign a live2d model to.

3. **Remove button**
    - Click this button and confirm if you want to delete all assigned model of a character.

## Model selection

![UI model list](https://raw.githubusercontent.com/SillyTavern/Extension-Live2d/main/readme_img/ui_model_list.png)

1. **Refresh button**
    - Click the refresh button if your live2d model does not show in the list.

2. **Select model**
    - Select a model from the list to assign it at the selected character
    - It can be a model stored in the asset folder or in the current character folder.
    - The list show the model folder name, if its asset or char origin and the name of the detected model setting file
    - It's possible that some model have different version in the same model folder, just try the different model file.

## Model settings

![UI model settings](https://raw.githubusercontent.com/SillyTavern/Extension-Live2d/main/readme_img/ui_model_settings.png)

1. **Model scale**
    - Use the slider to change the model smaller or bigger.

2. **Model center X offset**
    - Use the slider to change the model horizontal position relative to window center.

3. **Model center Y offset**
    - Use the slider to change the model vertical position relative to window center.

### Remarks
    - The settings are saved and carry over different chats.
    - You can also drag the model with your mouse and those settings will be updated and saved.
    - Use those ui settings to bring you model back on screen if you somehow made it out of view. Also check the show frame checkbox to see clearly where you can click to drag the model.

## Model Talk

![UI model talk](https://raw.githubusercontent.com/SillyTavern/Extension-Live2d/main/readme_img/ui_model_talk.png)

1. **Param mouth open Y id**
    - Select from the list the id of the paramater corresponding to the model mouth Y value. Not all model have one and name vary from model to model. Usually something like "PARAM_MOUTH_OPEN_Y" or "ParamMouthOpenY". Check the model when selecting an element of the list it will try to run the speak animation, if the mouth move you got it!

2. **Mouth movement speed**
    - Adjust the slider to chagne the movement speed of the mouth animation.

3. **Time per character**
    - Set the time duration of each character, the duration of the animation talk will be this time multiplied by the size of the message.

### Remarks
    - This mouth animation does not work on every model and every animations. Even if your model has animation where the mouth move does not mean the mouth animation can be controled by this extension. If nothing show in the parameter list your model is probably made with a too old version of live2d to access the parameters properly.

## Model Animations

![UI model animations](https://raw.githubusercontent.com/SillyTavern/Extension-Live2d/main/readme_img/ui_model_animations.png)

1. **Starter animation**
    - Select an expression and motion from the lists that will play when starting a chat with the character. You can also add a delay during wich the model will be invisible if you need to hide the character during some time to achieve perfect effect.

2. **Default animation**
    - Select and expression and motion from the list that will play when the character send a message. Used a fallback animation when using classify expression extension.

### Remarks
- Animation will play when you select one in the lists.
- use the replay button to replay the selected animation.
- Some model have expression defines as motions.
- If nothing show in the lists, it's probable your model setting file have no expression/motion defined.

## Hit areas mapping 

![UI model hit frames](https://raw.githubusercontent.com/SillyTavern/Extension-Live2d/main/readme_img/ui_model_hit_frames.png)

1. **Default click animation**
    - select an expression and motion from the list that will plays when you click on the model. You can also set a message that will be send a user message.

2. **Hit areas**
    - If the model have hit areas they will be listed and you can assign an animation/message to each of them.

### Remarks
    - Some model have no hit area but default click is detected for all.
    - Default click will trigger if you click on a hit area with nothing mapped or if clicking outside of any hit area
    - Hit are have priority defined in the model, for example "mouth" is inside "head", if it does not behave properly it's the model file fault.
    - For some model animation need to finish before starting another one, use the debug checkbox if you wanna force the refresh and spam animations.

## Classified expressions mapping

![UI model classify](https://raw.githubusercontent.com/SillyTavern/Extension-Live2d/main/readme_img/ui_model_classify.png)

1. **Requirements**
    - Need to use the classify expression extension.
    - Otherwise will fallback to default animation

2. **Mapping**
    - For each detected emotion by the classify extension you can assign an expression/motion animation.

### Remarks
    - If the previous animation did not finished when new message is received it's possible the new animation will not play. It's dependant of the live2d model. Use the debug checkbox to force the animation to play.

Thank you for following this guide! Your SillyTavern experience is now enriched with animated and interactive live2d models.
