---
order: 20
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

These display options affect how characters and messages are presented in the chat interface.

#### Avatar Style

Choose between Circle, Square, Rectangle, or Rounded Square. This setting applies to both user and AI avatars.

#### Chat Style

| Style        | Description                                                                                                                                                    | [Slash command](/For_Contributors/st-script.md#ui-styling) |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| **Flat**     | Clean and continuous "chat log" style, a flat canvas for your AI interactions to come to life.                                                                 | `/flat`<br>`/default`                                      |
| **Bubbles**  | "Instant messenger" style with distinct bubbles for each message, delightful rounded corners, and a subtle 3D effect.                                          | `/bubble`<br>`/bubbles`                                    |
| **Document** | Compact, document-like appearance with a text-focused layout. Hides avatars, timestamps, and message control buttons for past messages. | `/single`<br>`/story`                                      |

### Notifications

Set a position where the notification popups (toast messages) will appear on the screen.

* Top Left
* Top Center (default)
* Top Right
* Bottom Left
* Bottom Center
* Bottom Right

### Media Style

Default display style for media attachments (images, audio, video) in chat messages. Extensions that append media to chat messages may override this setting. It can also be changed manually per message using "Toggle media display style" action in the message context menu.

* **List**: Display all media attachments at once in a grid-like layout.
* **Gallery**: Display media attachments in a carousel-style gallery.

!!!
This setting also affects how the inline media attachments are sent to supported Chat Completion sources: list sends all attachments at once, while gallery sends the selected attachment.
!!!

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
* **Swipe # for All Messages**: Show swipe numbers on all messages
* **Characters Hotswap**: Quick-select buttons for favorite characters
* **Avatar Hover Magnification**: Zoom effect on avatar hover
* **Tags as Folders**: Organize characters using tags as folders
* **Click to Edit**: Click on messages to quickly open a message editor

### Custom CSS
/* ১. ক্যারেক্টার ব্যাকগ্রাউন্ড এবং গ্লাস ইফেক্ট */
body {
    background-size: cover !important;
    background-attachment: fixed !important;
}

#chat {
    background: rgba(0, 0, 0, 0.4) !important;
    backdrop-filter: blur(10px) !important;
}

/* ২. মেসেজ বাবল ডিজাইন (২ নম্বর ছবির মতো) */
.mes_text {
    background: rgba(255, 255, 255, 0.05) !important;
    border: 1px solid rgba(0, 212, 255, 0.3) !important;
    border-radius: 20px !important;
    padding: 15px !important;
    margin-bottom: 20px !important;
    color: #ffffff !important;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.5) !important;
}

/* ইউজারের মেসেজ ডানে এবং AI-এর মেসেজ বামে রাখা */
.mes[is_user="true"] .mes_text {
    margin-left: 20% !important; /* ডানে সরানোর জন্য */
    border-color: rgba(255, 0, 127, 0.5) !important; /* পিঙ্ক নিওন */
}

.mes[is_user="false"] .mes_text {
    margin-right: 20% !important; /* বামে রাখার জন্য */
    border-color: rgba(0, 212, 255, 0.5) !important; /* ব্লু নিওন */
}

/* ৩. ইনপুট বার এবং মাইক্রোফোন (Gemini স্টাইল) */
#send_form {
    background: rgba(15, 15, 15, 0.8) !important;
    border: 1.5px solid #00d4ff !important;
    border-radius: 35px !important;
    padding: 5px 15px !important;
    display: flex !important;
    align-items: center !important;
}

/* মাইক্রোফোন বাটন ডান পাশে (Send বাটনের পাশে) */
#mic_button {
    order: 3 !important;
    background: none !important;
    color: #ff007f !important;
    font-size: 22px !important;
    margin-left: 10px !important;
    filter: drop-shadow(0 0 5px #ff007f) !important;
}

#send_button {
    order: 2 !important;
}

/* ৪. ওপরের স্ক্রলযোগ্য ক্যারেক্টার বার */
#character_list_top {
    display: flex !important;
    overflow-x: auto !important;
    padding: 15px !important;
    background: rgba(0, 0, 0, 0.5) !important;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1) !important;
}

.character_thumb {
    width: 65px !important;
    height: 65px !important;
    border-radius: 50% !important;
    border: 2px solid #ff007f !important;
    box-shadow: 0 0 10px #ff007f !important;
    margin-right: 12px !important;
    flex-shrink: 0 !important;
}

/* ৫. থ্রি-ডট মেনু এবং অপশন সরণি */
.fa-ellipsis-v, .fa-bars {
    color: #00d4ff !important;
    font-size: 20px !important;
}


Allows you to apply custom CSS styles to further customize the appearance of the chat interface.

Use <i class="fa-fw fa-solid fa-maximize" title="Expand icon"></i> **Expand** to expand the editor window for better visibility and editing.

If you switch themes, your custom CSS will be replaced by the custom CSS of the new theme. Ensure you save your custom CSS to a theme if you want to keep it when switching themes.

If you use a lot of custom CSS, or want to use the same custom CSS with several themes, the unofficial [CSS Snippets extension](https://github.com/LenAnderson/SillyTavern-CssSnippets) can help you manage and organize your custom CSS.

---

## Message Sound

To play your own custom sound on receiving a new message from bot, replace the following MP3 file in your SillyTavern folder:

`public/sounds/message.mp3`

Plays at 80% volume.

If the "[Background Sound Only](index.md#miscellaneous)" option is enabled, the sound plays only if SillyTavern window is **unfocused**.

## Formulas Rendering

To enable math formulas rendering, use the [LaTeX extension](https://github.com/SillyTavern/Extension-LaTeX). To get the extension, you need to install it via the "Download Extensions & Assets" menu in SillyTavern.

Type your formulas in code blocks with `latex` or `asciimath` language identifiers for LaTeX and AsciiMath respectively. The extension uses [KaTeX](https://katex.org/) for rendering.

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!!info Deprecation notice
The legacy `$` and `$$` wrapper syntax is no longer supported. Please use the following regex scripts to polyfill the old syntax:

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
!!!
