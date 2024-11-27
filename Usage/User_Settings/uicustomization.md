---
order: user-settings-10
route: /usage/core-concepts/uicustomization/
---

# UI Customization

## UI Theme

### Theme Management
Theme files allow you to save, share, and reuse your UI customizations. You can maintain multiple themes for different moods or purposes, and switch between them instantly.

* Import/Export theme files
* Delete existing themes
* Save changes to current theme
* Save as new theme

All the settings in this section are saved to the current theme. If you switch themes, the settings will be replaced by the settings of the new theme.

### Display Settings
These fundamental display options affect how characters and messages are presented in the chat interface. Choose the style that best fits your preferences and provides the most comfortable reading experience.
* **Avatar Style**: Choose between Circle, Square, or Rectangle
* **Chat Style**: Select from Flat, Bubbles, or Document layouts

### Theme Colors
Customize the color scheme of every UI element to create your perfect theme. Colors can be selected using a color picker, and include transparency options where applicable.

* Main Text
* Italics Text
* Underlined Text
* Quote Text
* Text Shadow
* Chat Background
* UI Background
* UI Border
* User Message
* AI Message

### Layout & Visual Settings
Fine-tune the visual presentation of the interface with these sliders.

* **Chat Width**: Adjust chat window width (25-100% of screen)
* **Font Scale**: Customize text size (0.5-1.5x)
* **Blur Strength**: Control UI panel blur (0-30)
* **Shadow Width**: Adjust text shadow intensity (0-5)

### Theme Toggles
These switches control various UI features and behaviors. Some options can improve performance on lower-end devices, while others add useful information or functionality to the chat interface.
* **Reduced Motion**: Disable animations and transitions
* **No Blur Effect**: Remove background blur for better performance
* **No Text Shadows**: Disable text shadow effects
* **[Visual Novel mode](Visual-Novel.md)**: Compact chat with background sprite
* **Expand Message Actions**: Always show full message context menu
* **Zen Sliders**: Simplified parameter controls
* **Mad Lab Mode**: Unrestricted parameter ranges
* **Message Timer**: Show AI response generation time
* **Chat Timestamps**: Display message timestamps
* **Model Icons**: Show AI model icons for messages
* **Message IDs**: Display sequential message numbers
* **Hide Chat Avatars**: Remove avatars from chat
* **Message Token Count**: Show token counts per message
* **Compact Input Area**: Single-row input (Mobile only)
* **Swipe # for All Messages**: Show swipe numbers on all messages (Mobile)
* **Characters Hotswap**: Quick-select buttons for favorite characters
* **Avatar Hover Magnification**: Zoom effect on avatar hover
* **Tags as Folders**: Organize characters using tags as folders


### Custom CSS

Allows you to apply custom CSS styles to further customize the appearance of the chat interface.

Use <i class="fa-fw fa-solid fa-maximize" title="Expand icon"></i> **Expand** to expand the editor window for better visibility and editing.

If you switch themes, your custom CSS will be replaced by the custom CSS of the new theme. Ensure you save your custom CSS to a theme if you want to keep it when switching themes.

If you use a lot of custom CSS, or want to use the same custom CSS with several themes, the unofficial [CSS Snippets extension](https://github.com/LenAnderson/SillyTavern-CssSnippets) can help you manage and organize your custom CSS.

---

## Message Sound

To play your own custom sound on receiving a new message from bot, replace the following MP3 file in your SillyTavern folder:

`public/sounds/message.mp3`

Plays at 80% volume.

If the "[Background Sound Only](User_Settings.md#miscellaneous)" option is enabled, the sound plays only if SillyTavern window is **unfocused**.


## Formulas Rendering

To enable math formulas rendering, use the [LaTeX extension](https://github.com/SillyTavern/Extension-LaTeX). To get the extension, you need to install it via the "Download Extensions & Assets" menu in SillyTavern.

Type your formulas in code blocks with `latex` or `asciimath` language identifiers for LaTeX and AsciiMath respectively. The extension uses [KaTeX](https://katex.org/) for rendering.

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!! info Deprecation notice
The legacy `$` and `$$` wrapper syntax is no longer supported. Please use the following regex scripts to polyfill the old syntax:

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
!!!
