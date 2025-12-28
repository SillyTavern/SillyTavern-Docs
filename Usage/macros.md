---
order: 140
icon: icon-codescan
route: /usage/core-concepts/macros/
templating: false
---

# Macros

Macros are dynamic placeholders that get replaced with actual values when text is processed. They are used throughout SillyTavern in prompts, character cards, lorebooks, Quick Replies, and more.

## Finding Available Macros

SillyTavern provides built-in documentation for all available macros:

- **Slash command**: Type `/? macros` in the chat input to display a list of all registered macros with their descriptions.
- **Autocomplete**: Type `{{` during slash commands autocomplete in the input field to get suggestions for available macros.

## Basic Syntax

Macros are enclosed in double curly braces:

```
{{macroName}}
```

Examples:
```
{{user}}        // Returns the current user/persona name
{{char}}        // Returns the current character name
{{time}}        // Returns the current time
{{date}}        // Returns the current date
```

## Arguments

Many macros accept arguments to customize their behavior.

### Space Separator

For macros with a single argument, a space can separate the macro name from its argument:

```
{{macroName argument}}
```

Examples:
```
{{getvar myVariable}}
{{roll 1d20}}
{{reverse Hello World}}
```

### Double Colon Separator

Use `::` to separate multiple arguments:

```
{{macroName::arg1::arg2::arg3}}
```

Examples:
```
{{setvar::myVariable::Hello World}}
{{random::red::green::blue}}
{{roll::2d6+3}}
```

Both space and `::` are the recommended syntax for macro arguments.

### Single Colon Separator (Legacy)

A single `:` can also introduce arguments, but this syntax is considered legacy and not recommended for new content:

```
{{macroName:argument}}
```

Examples:
```
{{roll:1d20}}
```

## Whitespace in Macro Definitions

Whitespace between the macro name, separators, and arguments is ignored. This allows for more readable formatting:

```
{{ macroName :: arg1 :: arg2 }}
{{ setvar :: myVariable :: some value }}
{{ if :: condition }}
```

All of the above are equivalent to their compact forms without extra spaces.

## Nested Macros

Macros can be nested inside other macros. Inner macros are resolved first:

```
{{getvar::{{char}}_mood}}
```

This first resolves `{{char}}` (e.g., to "Alice"), then resolves `{{getvar::Alice_mood}}`.

More examples:

```
{{setvar::greeting::Hello, {{user}}!}}
```

Sets a variable with content that includes the user's name.

```
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

```
{{macroName argument}}
  scoped content here
{{/macroName}}
```

The closing tag uses the `/` flag before the macro name: `{{/macroName}}`.

This is equivalent to writing:
```
{{macroName::argument::scoped content here}}
```

### Examples

Setting a variable with multiline content:
```
{{ setvar backstory }}
  This character was born in a small village
  and grew up to become a renowned scholar.
{{ /setvar }}
```

Using `reverse` with scoped content:
```
{{ reverse }}
  Hello World
{{ /reverse }}
```

This returns "dlroW olleH".

### Content Trimming

By default, scoped content is automatically trimmed:
- Leading and trailing whitespace is removed
- Consistent indentation is dedented (the indentation of the first non-empty line is removed from all lines)

This allows clean formatting:

```
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

```
{{ if description }}
  # Character Description
  {{ description }}
{{ /if }}
```

This displays the heading and description only if `description` returns a non-empty value.

The condition can be:
- A macro name (resolved automatically if no arguments are required)
- Any value from a nested macro like `{{getvar::flag}}`
- Any text you want (that will implicitly resolve to truthy or falsy based on its content)

Falsy values: empty string, `false`, `0`, `off`, `no`.

### Inverted Condition

Prefix the condition with `!` to invert it:

```
{{ if !personality }}
  No personality defined for this character.
{{ /if }}
```

### If/Else Branches

Use `{{else}}` inside an `{{if}}` block to define an alternative branch:

```
{{ if personality }}
  {{ personality }}
{{ else }}
  No personality defined.
{{ /if }}
```

Another example:
```
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

```
{{!macroName}}
{{#macroName}}
```

Flags can be combined:

```
{{!?macroName}}
```

Whitespace is allowed between flags and the macro name:

```
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
| `.` | Variable (dot) | Shorthand for variable access using dot notation. |
| `$` | Variable (dollar) | Shorthand for variable access using dollar notation. |

### Preserve Whitespace Flag

Use the `#` flag when you need to preserve all whitespace in scoped content, including leading/trailing newlines and indentation:

```
{{ # setvar code }}
    function hello() {
        return "world";
    }
{{ /setvar }}
```

Without `#`, the content would be trimmed and dedented. With `#`, all whitespace is preserved exactly as written—including the newline after the opening tag and before the closing tag.

## Comments

Use the comment macro to add notes that won't appear in the output:

```
{{// This is a comment and will be removed}}
```

For multi-line comments, use scoped syntax:

```
{{ // }}
  This entire block is a comment.
  It can span multiple lines.
{{ /// }}
```

## Escaping Macros

To display literal curly braces without macro resolution, escape them with backslashes:

```
\{\{notAMacro\}\}
```

This outputs `{{notAMacro}}` as plain text.

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

### Names
- `{{user}}` - Current user/persona name
- `{{char}}` - Current character name
- `{{group}}` - Group chat members list
- `{{charIfNotGroup}}` - Character name (empty in groups)

### Character Card Fields
- `{{description}}` - Character description
- `{{personality}}` - Character personality
- `{{scenario}}` - Character scenario
- `{{persona}}` - User persona description

### Chat & Messages
- `{{lastMessage}}` - The last message in the chat
- `{{lastMessageId}}` - ID of the last message
- `{{firstIncludedMessageId}}` - ID of first message in context

### Time & Date
- `{{time}}` - Current time
- `{{date}}` - Current date
- `{{weekday}}` - Current day of the week
- `{{isotime}}` - ISO format timestamp

### Variables
- `{{getvar::name}}` - Get local variable value
- `{{setvar::name::value}}` - Set local variable
- `{{getglobalvar::name}}` - Get global variable value
- `{{setglobalvar::name::value}}` - Set global variable

### Randomization
- `{{random::a::b::c}}` - Random selection (re-rolls each time)
- `{{pick::a::b::c}}` - Stable random selection (consistent per chat)
- `{{roll::1d20}}` - Dice roll

### Utility
- `{{newline}}` - Insert newline character
- `{{space}}` - Insert space character
- `{{trim}}` - Remove surrounding newlines
- `{{reverse::text}}` - Reverse a string
- `{{input}}` - Current chat input field content

Use `/? macros` for the complete list of available macros and their detailed descriptions.
