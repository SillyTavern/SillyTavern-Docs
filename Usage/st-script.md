---
icon: file-symlink-file
---

# STscript Language Reference

## What is STscript?

It's a simple yet powerful scripting language that allows you to expand the functionality of SillyTavern without serious coding, allowing you to:

- Create mini-games or speed run challenges
- Build AI-powered chat insights
- Unleash your creativity and share with others

STscript is built using the slash commands engine, utilizing command batching, data piping, macros, and variables.
These concepts are going to be described in the following document.

## Hello, World!

To run your first script, open any SillyTavern chat and type the following into the chat input bar:

```
/pass Hello, World! | /echo
```

| <img width="300" alt="image" src="https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/9845caa2-5b83-4ba3-bb34-af02d4d05de5"> |
| -- |

You should the message in the toast on top of the screen. Now let's break it down bit by bit.

A script is a batch of commands, each one starting with the slash, with or without named and unnamed arguments, and terminated with the command separator character: `|`.

Commands are executed sequentially, one after another, and transfer data between each other.

1. The `/pass` command accepts a constant value of "Hello, World!" as an unnamed argument and writes it to the pipe.
2. The `/echo` command receives the value through the pipe from the previous command and displays it as a toast notification.

As constant unnamed arguments and pipes are interchangeable, we could rewrite this script simply as:

```
/echo Hello, World!
```

## User input

Now let's add a little bit of interactivity to the script. We will accept the input value from the user and display it in the notification.

```
/input Enter your name | /echo Hello, my name is {{pipe}}
```

1. The `/input` command is used to display an input box with the prompt specified in the unnamed argument and then writes the output to the pipe.
2. Because `/echo` already has an unnamed argument that sets the template for the output, we use the `{{pipe}}` macro to specify a place where the pipe value will be rendered.

| <img width="300" alt="image" src="https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/04fcc602-d8e0-42a9-ae06-f16613799047"> | <img width="300" alt="image" src="https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/323b4034-f6fa-4a06-b259-6ecd36733cb1"> |
| -- | -- |

## Flow control - conditionals

You can use the `/if` command to create conditional expressions that branch the execution based on the defined rules.

```
/input What's your favorite drink? | /if left={{pipe}} right="black tea" rule=eq else="/echo You shall not pass \| /abort" "/echo Welcome to the club, \{\{user\}\}"
```

This script evaluates the user input against a required value and displays different messages, depending on the input value.

### Arguments for `/if`

1. `left` is the first operand. Let's call it A.
2. `right` is the second operand. Let's call it B.
3. `rule` is the operation to be applied to the operands.
4. `else` is the optional string of subcommands to be executed if the result of boolean comparison is false.
5. Unnamed argument is the subcommand to be executed if the result of boolean comparison is true.

The operand values are evaluated in the following order:

1. Numeric literals
2. Local variable names
3. Global variable names
4. String literals

String values of named arguments could be escaped with quotes to allow multi-word strings. Quotes are then discarded.

### Boolean operations

Supported rules for boolean comparison are the following. An operation applied to the operands results in either a true or false value.

1. `eq` (equals) => A = B
2. `neq` (not equals) => A != B
3. `lt` (less than) => A < B
4. `gt` (greater than) => A > B
5. `lte` (less than or equals) => A <= B
6. `gte` (greater than or equals) => A >= B
7. `not` (unary negation) => !A
8. `in` (includes substring) => A includes B
9. `nin` (not includes substring) => A not includes B

### Subcommands

A subcommand is a string containing a list of slash commands to execute.

1. To use command batching in subcommands, the command separator character should be escaped like this: `\|`.
2. Since macro values are executed when the conditional is entered, not when the subcommand is executed, a macro could be additionally escaped to delay their evaluation to the subcommand execution time: `\{\{\macro\}\}`.
3. The result of the subcommands execution is piped to the command after `/if`.
4. The `/abort` command interrupts the script execution when encountered.

## Variables

Variables are used to store and manipulate data in scripts, using either commands or macros. The variables could be one of the following types:

- Local variables - saved to the metadata of the current chat, and unique to it.
- Global variables - saved to the settings.json and exist everywhere across the app.

1. `/getvar name` or `{{getvar::name}}` - gets the value of the local variable.
2. `/setvar key=name value` or `{{setvar::name::value}}` - sets the value of the local variable.
3. `/addvar key=name increment` or `{{addvar::name::increment}}` - adds the `increment` to the value of the local variable.
4. `/getglobalvar name` or `{{getglobalvar::name}}` - gets the value of the global variable.
5. `/setglobalvar key=name` or `{{setglobalvar::name::value}}` - sets the value of the global variable.
6. `/addglobalvar key=name` or `{{addglobalvar::name:increment}}` - adds the `increment` to the value of the global variable.
7. `/flushvar name` - deletes the value of the local variable.
8. `/flushglobalvar name` - deletes the value of the global variable.

The default value of previously undefined variables is an empty string, or a zero of it is first used in the `/addvar` command.

Increment in the `/addvar` command performs an addition or subtraction of the value if it can be converted to a number, or otherwise does the string concatenation.

All slash commands for variable manipulation write the resulting value into the pipe for the next command to use.

Now, let's consider the following example:

```
/input What do you want to generate? | /setvar key=SDinput | /echo Requesting an image of {{getvar::SDinput}} | /getvar SDinput | /imagine
```

1. The value of the user input is saved in the local variable named `SDinput`.
2. The `getvar` macro is used to display the value in the `/echo` command.
3. The `getvar` command is used to retrieve the value of the variable and pass it through the pipe.
4. The value is passed to the `/imagine` command (provided by the Image Generation plugin) to be used as its input prompt.

Since the variables are saved and not flushed between the script executions, you can reference the variable in other scripts and via macros, and it will resolve to the same value as during the execution of the example script. To guarantee that the value will be discarded, add the `/flushvar` command to the script.

## Using the LLM

Under construction.

## Flow control - loops

Under construction.

## Quick Replies: script library and auto-execution

Under construction.

## Calling procedures

Under construction.
