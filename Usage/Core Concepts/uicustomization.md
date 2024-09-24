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

### Power User Options

#### Formulas Rendering

Enables math formulas rendering using the [showdown-katex](https://obedm503.github.io/showdown-katex/) package.

The following formatting rules are supported:

##### LaTeX syntax

```txt
$$ formula goes here $$
```

##### Asciimath syntax

```txt
$ formula goes here $
```

More information: [KaTeX](https://katex.org/)
