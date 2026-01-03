---
order: 140
icon: codescan
route: /usage/core-concepts/macros/
templating: false
---

# Macros

!!!tip Experimental Macro Engine
To enable advanced macro processing that supports nesting, stable substitution order, and other improvements, go to **User Settings** > **Chat/Message Handling** and enable the **Experimental Macro Engine** option.
!!!

Macros are dynamic placeholders that get replaced with actual values when text is processed. They are used throughout SillyTavern in prompts, character cards, lorebooks, Quick Replies, and more.

## Finding Available Macros

SillyTavern provides built-in documentation for all available macros:

- **Slash command**: Type `/? macros` in the chat input to display a list of all registered macros with their descriptions.
- **Autocomplete**: Type `{{` during slash commands autocomplete in the input field to get suggestions for available macros.

## Basic Syntax

Macros are enclosed in double curly braces:

```txt
{{macroName}}
```

Macro names are **case-insensitive**. `{{User}}`, `{{USER}}`, and `{{user}}` all resolve to the same macro.

Examples:

```txt
{{user}}        // Returns the current user/persona name
{{char}}        // Returns the current character name
{{time}}        // Returns the current time
{{date}}        // Returns the current date
```

## Arguments

Many macros accept arguments to customize their behavior.

### Space Separator

For macros with a single argument, a space can separate the macro name from its argument:

```txt
{{macroName argument}}
```

Examples:

```txt
{{getvar myVariable}}
{{roll 1d20}}
{{reverse Hello World}}
```

### Double Colon Separator

Use `::` to separate multiple arguments:

```txt
{{macroName::arg1::arg2::arg3}}
```

Examples:

```txt
{{setvar::myVariable::Hello World}}
{{random::red::green::blue}}
{{roll::2d6+3}}
```

Both space and `::` are the recommended syntax for macro arguments.

### Single Colon Separator (Legacy)

A single `:` can also introduce arguments, but this syntax is considered legacy and not recommended for new content:

```txt
{{macroName:argument}}
```

Examples:

```txt
{{roll:1d20}}
```

## Whitespace in Macro Definitions

Whitespace between the macro name, separators, and arguments is ignored. This allows for more readable formatting:

```txt
{{ macroName :: arg1 :: arg2 }}
{{ setvar :: myVariable :: some value }}
{{ if :: condition }}
```

All of the above are equivalent to their compact forms without extra spaces.

## Nested Macros

Macros can be nested inside other macros. Inner macros are resolved first:

```txt
{{getvar::{{char}}_mood}}
```

This first resolves `{{char}}` (e.g., to "Alice"), then resolves `{{getvar::Alice_mood}}`.

More examples:

```txt
{{setvar::greeting::Hello, {{user}}!}}
```

Sets a variable with content that includes the user's name.

```txt
{{if {{getvar::showDetails}}}}Details here{{/if}}
```

The condition itself is a macro that retrieves a variable value.

## Scoped Macros

!!!warning Staging Feature
This is currently only available on the `staging` branch of SillyTavern, and not part of the latest release.
!!!

Any macro that accepts at least one argument supports scoped syntax. The content between opening and closing tags becomes the **last argument** of the macro.

### Scoped Syntax

Instead of writing the last argument inline, it can be placed between opening and closing tags:

```txt
{{macroName argument}}
  scoped content here
{{/macroName}}
```

The closing tag uses the `/` flag before the macro name: `{{/macroName}}`.

This is equivalent to writing:

```txt
{{macroName::argument::scoped content here}}
```

### Examples

Setting a variable with multiline content:

```txt
{{ setvar backstory }}
  This character was born in a small village
  and grew up to become a renowned scholar.
{{ /setvar }}
```

Using `reverse` with scoped content:

```txt
{{ reverse }}
  Hello World
{{ /reverse }}
```

This returns "dlroW olleH".

### Content Trimming

By default, scoped content is automatically trimmed:

- Leading and trailing whitespace is removed
- Consistent indentation is de-dented (the indentation of the first non-empty line is removed from all lines)

This allows clean formatting:

```txt
{{ if condition }}
    # Heading
    Some content here
{{ /if }}
```

Produces `# Heading\nSome content here` (without the leading spaces).

To preserve all whitespace including leading/trailing newlines, use the `#` flag. See [Macro Flags](#macro-flags) for details.

## Conditional Macros

!!!warning Staging Feature
This is currently only available on the `staging` branch of SillyTavern, and not part of the latest release.
!!!

The `{{if}}` macro renders content conditionally based on whether a value is truthy or falsy.

### Simple Condition

```txt
{{ if description }}
  # Character Description
  {{ description }}
{{ /if }}
```

This displays the heading and description only if `description` returns a non-empty value.

The condition can be:

- A macro name (resolved automatically if no arguments are required)
- Any value from a nested macro like `{{getvar::flag}}`
- A variable shorthand like `.myFlag` or `$globalFlag` (see [Variable Shorthand Syntax](#variable-shorthand-syntax))
- Any text you want (that will implicitly resolve to truthy or falsy based on its content)

Falsy values: empty string, `false`, `0`, `off`, `no`.

### Using Variable Shorthands in Conditions

Variable shorthands provide a concise way to check variable values in conditions:

```txt
{{ if .isEnabled }}
  Feature is enabled.
{{ /if }}

{{ if !$globalDisabled }}
  Not globally disabled.
{{ /if }}
```

See [Variable Shorthand Syntax](#variable-shorthand-syntax) for more details on shorthand notation.

### Inverted Condition

Prefix the condition with `!` to invert it:

```txt
{{ if !personality }}
  No personality defined for this character.
{{ /if }}
```

### If/Else Branches

Use `{{else}}` inside an `{{if}}` block to define an alternative branch:

```txt
{{ if personality }}
  {{ personality }}
{{ else }}
  No personality defined.
{{ /if }}
```

Another example:

```txt
{{ if {{getvar::details-block}} }}
  # Details Block
  {{ getvar::details-block }}
{{ else }}
  No details currently exist.
{{ /if }}
```

## Macro Flags

!!!warning Staging Feature
This is currently only available on the `staging` branch of SillyTavern, and not part of the latest release.
!!!

Flags are special symbol characters placed between the opening braces and the macro name that modify macro behavior.

### Syntax

```txt
{{!macroName}}
{{#macroName}}
```

Flags can be combined:

```txt
{{!?macroName}}
```

Whitespace is allowed between flags and the macro name:

```txt
{{ / macroName }}
{{ # macroName }}
```

### Implemented Flags

| Flag | Name | Description |
|------|------|-------------|
| `/` | Closing Block | Marks a closing tag for scoped macros. Example: `{{/if}}` |
| `#` | Preserve Whitespace | Prevents automatic trimming of scoped content. |

### Planned Flags (Not Yet Implemented)

| Flag | Name | Description |
|------|------|-------------|
| `!` | Immediate | Resolve this macro before other macros in the same text. |
| `?` | Delayed | Resolve this macro after other macros in the same text. |
| `~` | Re-evaluate | Mark this macro for re-evaluation. |
| `>` | Filter | Enable pipe-based output filters for this macro. |

### Flags-like prefix operators

Variable shorthand syntax uses prefix operators (`.` and `$`) which behave similarly to flags but are not flags themselves.  
See the [Variable Shorthand Syntax](#variable-shorthand-syntax) section for details.

### Preserve Whitespace Flag

Use the `#` flag when you need to preserve all whitespace in scoped content, including leading/trailing newlines and indentation:

```txt
{{ # setvar code }}
    function hello() {
        return "world";
    }
{{ /setvar }}
```

Without `#`, the content would be trimmed and dedented. With `#`, all whitespace is preserved exactly as written—including the newline after the opening tag and before the closing tag.

## Comments

Use the comment macro to add notes that won't appear in the output:

```txt
{{// This is a comment and will be removed}}
```

For multi-line comments, use scoped syntax:

```txt
{{ // }}
  This entire block is a comment.
  It can span multiple lines.
{{ /// }}
```

## Escaping Macros

To display literal curly braces without macro resolution, escape them with backslashes:

```txt
\{\{notAMacro\}\}
```

This outputs `{{notAMacro}}` as plain text.

## Variable Shorthand Syntax

!!!warning Staging Feature
This is currently only available on the `staging` branch of SillyTavern, and not part of the latest release.
!!!

Variable shorthands provide a concise syntax for common variable operations. Use `.` for local variables and `$` for global variables.

### Variable Shorthands Prefix Operators

| Prefix | Name            | Description                                                     |
| ------ | --------------- | --------------------------------------------------------------- |
| `.`    | Local Variable  | Shorthand for local variable operations. Example: `{{.myvar}}`  |
| `$`    | Global Variable | Shorthand for global variable operations. Example: `{{$myvar}}` |

These prefix operators have to be placed **immediately before** the variable name, after any optionally appearing [Macro Flags](#macro-flags). They aren't considered macro flags, but more indicators that a variable shorthand is being inserted, instead of a macro by name. The prefix operators are not part of the variable name itself, but rather modifiers that change how the variable is accessed.

### Getting Variables

Retrieve variable values with a simple prefix:

```txt
{{.myvar}}       // Get local variable "myvar"
{{$myvar}}       // Get global variable "myvar"
```

Equivalent to `{{getvar::myvar}}` and `{{getglobalvar::myvar}}`.

### Setting Variables

Use the `=` operator to set a variable value:

```txt
{{ .myvar = Hello World }}     // Set local variable
{{ $myvar = Some value }}      // Set global variable
```

Equivalent to `{{setvar::myvar::Hello World}}` and `{{setglobalvar::myvar::Hello World}}`. Returns an empty string.

### Increment and Decrement

Use `++` and `--` to increment or decrement numeric variables:

```txt
{{.counter++}}    // Increment local variable, returns new value
{{$counter--}}    // Decrement global variable, returns new value
```

Equivalent to `{{incvar counter}}` and `{{decglobalvar counter}}`.

### Add to Variable

Use `+=` to add a numeric value to a variable:

```txt
{{.score += 10}}     // Add 10 to local variable
{{$total += 5}}      // Add 5 to global variable
```

Equivalent to `{{addvar::score::10}}` and `{{addglobalvar::total::5}}`. Returns an empty string.

The add operator also supports appending string to an existing string macro, if neither of them are numbers.

```txt
{{.myvar += {{noop}} | Second block}}   // Resolves to "Content | Second block" when the variable before was "Content".
                                        // Use `{{noop}}` to be able to add whitespaces, that otherwise would be trimmed automatically.
```

### Nested Macros in Values

Variable values can contain nested macros:

```txt
{{.greeting = Hello, {{user}}!}}
```

Resolves to a variable that that saves `Hello, User!` inside. (If `{{user}}` is named "User")

### Whitespace Handling

Whitespace around operators is allowed:

```txt
{{ .myvar = spaced value }}
{{ .counter ++ }}
```

### Variable Names

Variable names follow the same rules as macro identifiers: start with a letter, followed by letters, digits, underscores, or hyphens. The last character is not allowed to be an underscore or hyphen.

```txt
{{.my-var}}       // Valid
{{.my_var}}       // Valid
{{.myVar123}}     // Valid
```

If your variable has an identifier that does not match the standard rules, you have to use the full variable macro syntax with angle brackets (e.g. `{{getvar::my§var----}}`), or rename/move your variable value.

## Legacy Syntax

For backwards compatibility, angle bracket markers are still supported:

| Legacy | Equivalent Macro |
|--------|------------------|
| `<USER>` | `{{user}}` |
| `<BOT>` | `{{char}}` |
| `<CHAR>` | `{{char}}` |
| `<GROUP>` | `{{group}}` |
| `<CHARIFNOTGROUP>` | `{{charIfNotGroup}}` |

These are automatically converted to their macro equivalents during processing.

> **Note:** Legacy syntax is not recommended. Use the equivalent `{{macro}}` syntax instead for new content.

## Common Macros by Category

!!!tip
Use `/? macros` for the complete list of available macros and their detailed descriptions.
!!!

### Names & Participants

| Macro | Description |
|-------|-------------|
| `{{user}}` | Current user/persona name |
| `{{char}}` | Current character name |
| `{{group}}` | Comma-separated list of group member names (including muted) or character name in solo chats |
| `{{groupNotMuted}}` | Comma-separated list of group member names excluding muted members |
| `{{charIfNotGroup}}` | Character name (empty in groups) |
| `{{notChar}}` | Comma-separated list of all participants except the current speaker |

### Character Card & Persona Fields

| Macro | Description |
|-------|-------------|
| `{{description}}` | Character description |
| `{{personality}}` | Character personality |
| `{{scenario}}` | Character scenario |
| `{{persona}}` | User persona description |
| `{{charPrompt}}` | Character's Main Prompt override |
| `{{charInstruction}}` | Character's Post-History Instructions override |
| `{{charDepthPrompt}}` | Character's @ Depth Note |
| `{{charCreatorNotes}}` | Creator notes from the character card |
| `{{charVersion}}` | Character's version number |
| `{{mesExamples}}` | Character's dialogue examples, formatted for instruct mode |
| `{{mesExamplesRaw}}` | Unformatted dialogue examples from the character card |
| `{{original}}` | Original message content for substitution in character prompt overrides |

### Chat History & Messages

| Macro | Description |
|-------|-------------|
| `{{lastMessage}}` | Last message in the chat |
| `{{lastMessageId}}` | Index of the last message in the chat |
| `{{lastUserMessage}}` | Last user message in the chat |
| `{{lastCharMessage}}` | Last character/bot message in the chat |
| `{{firstIncludedMessageId}}` | Index of first message included in current context |
| `{{firstDisplayedMessageId}}` | Index of the first displayed message in the chat |
| `{{lastSwipeId}}` | 1-based index of the last swipe for the last message |
| `{{currentSwipeId}}` | 1-based index of the current swipe |
| `{{summary}}` | Latest chat summary from the "Summarize" extension (when available) |

### Time & Date

| Macro | Description |
|-------|-------------|
| `{{time}}` | Current local time |
| `{{time::UTC±(offset)}}` | Time with UTC offset |
| `{{date}}` | Current local date in short format |
| `{{weekday}}` | Current day of the week |
| `{{isotime}}` | Current time in HH:mm format |
| `{{isodate}}` | Current date in YYYY-MM-DD format |
| `{{datetimeformat::format}}` | Custom formatted date/time (e.g., `YYYY-MM-DD HH:mm:ss`) |
| `{{idleDuration}}` | Human-readable duration since the last user message |
| `{{timeDiff::left::right}}` | Human-readable difference between two times |

### Variables

| Macro | Description |
|-------|-------------|
| `{{getvar::name}}` | Get local variable value |
| `{{setvar::name::value}}` | Set local variable |
| `{{addvar::name::value}}` | Add value to local variable (numeric or string append) |
| `{{incvar::name}}` | Increment local variable by 1 and return new value |
| `{{decvar::name}}` | Decrement local variable by 1 and return new value |
| `{{getglobalvar::name}}` | Get global variable value |
| `{{setglobalvar::name::value}}` | Set global variable |
| `{{addglobalvar::name::value}}` | Add value to global variable (numeric or string append) |
| `{{incglobalvar::name}}` | Increment global variable by 1 and return new value |
| `{{decglobalvar::name}}` | Decrement global variable by 1 and return new value |

### Randomization

| Macro | Description |
|-------|-------------|
| `{{random::a::b::c}}` | Random selection (re-rolls each time) |
| `{{pick::a::b::c}}` | Stable random selection (consistent per chat and position) |
| `{{roll::1d20}}` | Dice roll using droll syntax |

### Runtime State

| Macro | Description |
|-------|-------------|
| `{{maxPrompt}}` | Maximum prompt context size |
| `{{model}}` | Model name for the currently selected API |
| `{{isMobile}}` | "true" if running in mobile environment, "false" otherwise |
| `{{lastGenerationType}}` | Type of last queued generation request (e.g., "normal", "impersonate", "regenerate", "quiet", "swipe", "continue") |

### Prompt Templates

| Macro | Description |
|-------|-------------|
| `{{systemPrompt}}` | Active system prompt text (optionally overridden by character) |
| `{{defaultSystemPrompt}}` | Default system prompt |
| `{{authorsNote}}` | Contents of the Author's Note |
| `{{charAuthorsNote}}` | Contents of the Character Author's Note |
| `{{defaultAuthorsNote}}` | Contents of the Default Author's Note |
| `{{instructStoryStringPrefix}}` | Instruct story string prefix |
| `{{instructStoryStringSuffix}}` | Instruct story string suffix |
| `{{instructUserPrefix}}` | Instruct input/user prefix sequence |
| `{{instructUserSuffix}}` | Instruct input/user suffix sequence |
| `{{instructAssistantPrefix}}` | Instruct output/assistant prefix sequence |
| `{{instructAssistantSuffix}}` | Instruct output/assistant suffix sequence |
| `{{instructSeparator}}` | Instruct separator sequence |
| `{{instructSystemPrefix}}` | Instruct system prefix sequence |
| `{{instructSystemSuffix}}` | Instruct system suffix sequence |
| `{{instructFirstAssistantPrefix}}` | Instruct first assistant/output prefix sequence |
| `{{instructLastAssistantPrefix}}` | Instruct last assistant/output prefix sequence |
| `{{instructFirstUserPrefix}}` | Instruct first user/input prefix sequence |
| `{{instructLastUserPrefix}}` | Instruct last user/input prefix sequence |
| `{{instructStop}}` | Instruct stop sequence |
| `{{instructUserFiller}}` | Instruct user alignment filler |
| `{{instructSystemInstructionPrefix}}` | Instruct system instruction prefix sequence |
| `{{chatSeparator}}` | Separator between example chat blocks in text completion prompts |
| `{{chatStart}}` | Chat start marker in text completion prompts |
| `{{reasoningPrefix}}` | Prefix string used before reasoning blocks |
| `{{reasoningSuffix}}` | Suffix string used after reasoning blocks |
| `{{reasoningSeparator}}` | Separator between content and response |
| `{{charPrefix}}` | Character's positive Image Generation prompt prefix |
| `{{charNegativePrefix}}` | Character's negative Image Generation prompt prefix |

### Utility

| Macro | Description |
|-------|-------------|
| `{{newline}}` | Insert newline character |
| `{{newline::count}}` | Insert multiple newlines |
| `{{space}}` | Insert space character |
| `{{space::count}}` | Insert multiple spaces |
| `{{noop}}` | Does nothing, produces empty string |
| `{{trim}}` | Remove surrounding newlines |
| `{{reverse::text}}` | Reverse a string |
| `{{input}}` | Current chat input field content |
| `{{banned::word}}` | Ban a word for Text Completion backend |
| `{{outlet::key}}` | Return world info outlet prompt for a given outlet key |
