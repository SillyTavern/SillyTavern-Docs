---
tags:
    [
        visual novel,
        vn,
    ]
---

# Visual Novel (VN) Mode

Visual Novel Mode is a special screen layout in SillyTavern that allows you to chat to characters with sprites (or their character card image) that resembles that of a visual novel like Doki Doki Literature Club, The Fruits of Grisaia, Fate: Stay/night and other famous VN games.

## Toggling Visual Novel Mode

### Enabling Visual Novel Mode

Visual Novel Mode comes built in with SillyTavern and can be toggle by going to *User Settings* (User Settings Icon) and checking **Visual Novel Mode** below *No Text Shadows*.

![User Settings](/static/vn/vn-mode-toggle.png)

### Disabling Visual Novel Mode

Disabling Visual Novel Mode is the same steps as enabling it.

!!! warning VN Mode with VN Extensions
Some extensions (like the Prome VN Extension) will toggle this option if you use their own respective VN modes. Enabling/Disabling VN Mode from the *User Settings* menu will also affect these extensions as well and vice versa.
!!!

## The Visual Novel UI

![VN Display](/static/vn/vn-display.png)

In Visual Novel Mode, the UI is altered slightly in order to accommodate character sprites (or the character card image) which is shown in the center. In a group chat with multiple characters however, the character sprites will spread themselves out, accommodating for each other as shown below.

![Group VN Display](/static/vn/group-vn-display.png)

### VN Mode with MovingUI

!!! info
To toggle MovingUI, go to *User Settings* and check on **MovingUI**. Do note that this feature **only** works on Desktops.
!!!

If **MovingUI** is enabled in *User Settings*, the sprites (or character card image) can be moved around if you wish to move them around or place them in a more specific area on the screen.

!!! warning Regarding Sprite Sizes
If the size of your character sprites are relatively big, that MovingUI's clickable box icon will be unable to be clicked unless you adjust the placement of other sprites to facilitate them.
!!!

![Group VN Display (MovingUI)](/static/vn/vn-group-display-movingui.png)

## Obtaining Character Sprites

Obtaining character sprites can be done by browsing the internet for existing sprites, for say, a existing character from a Visual Novel or a game that uses a Visual Novel feature such as DDLC or CounterSide. If the character you desire sprites form does not come with sprites already, you have several options remaining.

1. Search the character post for any sprite ZIP package or link to a sprite pack.
    !!! info
    Some bot creators may release their bots with a sprite pack (either within the same post or in a sprites channel). Search those posts if someone hasn't made sprites of the character you want already.
    !!!
2. Create your own using LoRAs and Stable Diffusion.
    !!! warning
    Generating sprites from scratch is time-consuming (especially if no LoRAs exist for your character and/or for the Stable Diffusion model you want to use) and will require decent hardware in order to generate them, more so if you plan on making 28 sprite expression than 6 and if you are using SDXL and/or upscaling sprites to a more higher resolution.
    !!!
3. Use the character card image. It might not be like a sprite, but at least you have something to look at on-screen.

## VN Extensions

!!! warning
The following VN extensions are not maintained by the SillyTavern developers. Be mindful of your SillyTavern's security when installing such extensions.
!!!

### Prome Visual Novel Extension

The Prome Visual Novel Extension is a third-party extension from Bronya Rand and Prometheus that enhances the visual novel experience further with small features like Letterbox Mode which makes the visual novel UI more "cinematic" and adds the ability to hide the message box (sheld) temporarily for screenshots, etc.

| Letterbox Mode | Hide Sheld |
| :------------: | :--------: |
| ![Horizontal Letterbox Mode](/static/vn/extensions/prome/horizontal.png) | ![Hide Sheld](/static/vn/extensions/prome/sheld_hide.png)

To install the Prome Visual Novel Extension, follow the installation instructions on the [Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension?tab=readme-ov-file#installation-and-usage) Github page. Enabling the Prome Visual Novel Extension's settings can be found in *Extensions* -> **Prome (Visual Novel Extension)**.
