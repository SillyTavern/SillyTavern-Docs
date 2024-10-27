---
order: user-settings-10
---

# UI Customization

### UI Customization

#### Message Sound

To play your own custom sound on receiving a new message from bot, replace the following MP3 file in your SillyTavern folder:

`public/sounds/message.mp3`

Plays at 80% volume.

If "Background Sound Only" option is enabled, the sound plays only if SillyTavern window is **unfocused**.

### UI Colors

You can use the color pickers and sliders to customize the UI in many ways.

You can also save presets and share them with other users (saved into `/data/<user-handle>/themes/`).

### Formulas Rendering

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
