---
icon: file-symlink-file
---

# STscript Language Reference

## What is STscript?

It's a simple yet powerful scripting language that could be used to expand the functionality of SillyTavern without serious coding, allowing you to:

- Create mini-games or speed run challenges
- Build AI-powered chat insights
- Unleash your creativity and share with others

STscript is built using the slash commands engine, utilizing command batching, data piping, macros, and variables.
These concepts are going to be described in the following document.

> **STscript is still in active development.** It's recommended to use the *staging* branch if you plan to use any of its features.

### Security precaution

With great power comes great responsibility. Be careful and always inspect the scripts before executing them.

## Hello, World!

To run your first script, open any SillyTavern chat and type the following into the chat input bar:

```
/pass Hello, World! | /echo
```

| <img alt="image" src="https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/9845caa2-5b83-4ba3-bb34-af02d4d05de5"> |
| -- |

You should the message in the toast on top of the screen. Now let's break it down bit by bit.

A script is a batch of commands, each one starting with the slash, with or without named and unnamed arguments, and terminated with the command separator character: `|`.

Commands are executed sequentially, one after another, and transfer data between each other.

1. The `/pass` command accepts a constant value of "Hello, World!" as an unnamed argument and writes it to the pipe.
2. The `/echo` command receives the value through the pipe from the previous command and displays it as a toast notification.

> **Hint:** To see a list of all available commands, type `/help slash` into the chat.

As constant unnamed arguments and pipes are interchangeable, we could rewrite this script simply as:

```
/echo Hello, World!
```

## User input

Now let's add a little bit of interactivity to the script. We will accept the input value from the user and display it in the notification.

```
/input Enter your name |
/echo Hello, my name is {{pipe}}
```

1. The `/input` command is used to display an input box with the prompt specified in the unnamed argument and then writes the output to the pipe.
2. Because `/echo` already has an unnamed argument that sets the template for the output, we use the `{{pipe}}` macro to specify a place where the pipe value will be rendered.

| <img alt="image" src="https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/04fcc602-d8e0-42a9-ae06-f16613799047"> | <img alt="image" src="https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/323b4034-f6fa-4a06-b259-6ecd36733cb1"> |
| -- | -- |

### Other input/output commands

- `/popup (text)` — shows a blocking popup, supports lite HTML formatting, e.g: `/popup <font color=red>I'm red!</font>`.
- `/setinput (text)` — replaces the contents of the user input bar with the provided text.
- `/speak voice="name" (text)` — narrates the text using the selected TTS engine and the character name from the voice map, e.g. `/speak name="Donald Duck" Quack!`.
- `/buttons labels=["a","b"] (text)` — shows a blocking popup with the specified text and button labels. `labels` must be a JSON-serialized array of strings or a variable name containing such an array. Returns the clicked button label into the pipe or empty string if canceled. The text supports lite HTML formatting.

## Variables

Variables are used to store and manipulate data in scripts, using either commands or macros. The variables could be one of the following types:

- Local variables — saved to the metadata of the current chat, and unique to it.
- Global variables — saved to the settings.json and exist everywhere across the app.

1. `/getvar name` or `{{getvar::name}}` — gets the value of the local variable.
2. `/setvar key=name value` or `{{setvar::name::value}}` — sets the value of the local variable.
3. `/addvar key=name increment` or `{{addvar::name::increment}}` — adds the `increment` to the value of the local variable.
4. `/getglobalvar name` or `{{getglobalvar::name}}` — gets the value of the global variable.
5. `/setglobalvar key=name` or `{{setglobalvar::name::value}}` — sets the value of the global variable.
6. `/addglobalvar key=name` or `{{addglobalvar::name:increment}}` — adds the `increment` to the value of the global variable.
7. `/flushvar name` — deletes the value of the local variable.
8. `/flushglobalvar name` — deletes the value of the global variable.

The default value of previously undefined variables is an empty string, or a zero of it is first used in the `/addvar` command.

Increment in the `/addvar` command performs an addition or subtraction of the value if it can be converted to a number, or otherwise does the string concatenation.

All *slash commands* for variable manipulation write the resulting value into the pipe for the next command to use.
For *macros*, only the "get" type macro returns the value, "add" and "set" are replaced with an empty string instead.

Now, let's consider the following example:

```
/input What do you want to generate? |
/setvar key=SDinput |
/echo Requesting an image of {{getvar::SDinput}} |
/getvar SDinput |
/imagine
```

1. The value of the user input is saved in the local variable named `SDinput`.
2. The `getvar` macro is used to display the value in the `/echo` command.
3. The `getvar` command is used to retrieve the value of the variable and pass it through the pipe.
4. The value is passed to the `/imagine` command (provided by the Image Generation plugin) to be used as its input prompt.

Since the variables are saved and not flushed between the script executions, you can reference the variable in other scripts and via macros, and it will resolve to the same value as during the execution of the example script. To guarantee that the value will be discarded, add the `/flushvar` command to the script.

## Flow control - conditionals

You can use the `/if` command to create conditional expressions that branch the execution based on the defined rules.

`/if left=valueA right=valueB rule=comparison else="(command on false)" "(command on true)"`

Let's review the following example:

```
/input What's your favorite drink? |
/if left={{pipe}} right="black tea" rule=eq else="/echo You shall not pass \| /abort" "/echo Welcome to the club, \{\{user\}\}"
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
8. `in` (includes substring) => A includes B, case insensitive
9. `nin` (not includes substring) => A not includes B, case insensitive

### Subcommands

A subcommand is a string containing a list of slash commands to execute.

1. To use command batching in subcommands, the command separator character should be escaped like this: `\|`.
2. Since macro values are executed when the conditional is entered, not when the subcommand is executed, a macro could be additionally escaped to delay their evaluation to the subcommand execution time: `\{\{\macro\}\}`.
3. The result of the subcommands execution is piped to the command after `/if`.
4. The `/abort` command interrupts the script execution when encountered.

`/if` commands can be used as a ternary operator.
The following example will pass a "true" string to the next command the variable `a` equals 5, and a "false" string otherwise.

```
/if left=a right=5 rule=eq else="/pass false" "/pass true" |
/echo
```

## Flow control - loops

If you need to run some command in a loop until a certain condition is met, use the `/while` command.

`/while left=valueA right=valueB rule=operation guard=on "commands"`

On each step of the loop it compares the value of variable A with the value of variable B, and if the condition yields true, then executes any valid slash command enclosed in quotes, otherwise exists the loop. This command doesn't write anything to the output pipe.

### Arguments for `/while`

**The set of available boolean comparisons, handing of variables, literal values, and subcommands is the same as for the `/if` command.**

The optional `guard` named argument (`on` by default) is used to protect against endless loops, limiting the number of iterations to 100.
To disable and allow endless loops, set `guard=off`.

This example adds 1 to the value of `i` until it reaches 10, then outputs the resulting value (10 in this case).

```
/setvar key=i 0 |
/while left=i right=10 rule=lt "/addvar key=i 1" |
/echo {{getvar::i}} |
/flushvar i
```

## Math operations

- All of the following operations accept a series of numbers or variable names and output the result to the pipe.
- Invalid operations (such as division by zero), and operations that result in a NaN value or infinity return zero.
- Multiplication and addition accept an unlimited number of arguments separated by spaces.
- Subtraction, division, exponentiation, and modulo accept two arguments separated by spaces.
- Sine, cosine, natural logarithm, absolute value, and rounding accept one argument.

**List of operations:**

1. `/add (a b c d)` – performs an addition of the set of values, e.g. `/add 10 i 30 j`
2. `/mul (a b c d)` – performs a multiplication of the set of values, e.g. `/mul 10 i 30 j`
3. `/sub (a b)` – performs a subtraction of two values, e.g. `/sub i 5`
4. `/div (a b)` – performs a division of two values, e.g. `/div 10 i`
5. `/mod (a b)` – performs a modulo operation of two values, e.g. `/mod i 2`
6. `/pow (a b)` – performs a power operation of two values, e.g. `/pow i 2`
7. `/sin (a)` – performs a sine operation of a value, e.g. `/sin i`
8. `/cos (a)` – performs a cosine operation of a value, e.g. `/cos i`
9. `/log (a)` – performs a natural logarithm operation of a value, e.g. `/log i`
10. `/abs (a)` – performs an absolute value operation of a value, e.g. `/abs -10`
11. `/round (a)` – performs a rounding to the nearest integer operation of a value, e.g. `/round 3.14`

Example: get an area of a circle with a radius of 50.

```
/setglobalvar key=PI 3.1415 |
/setvar key=r 50 |
/mul r r PI |
/round |
/echo Circle area: {{pipe}}
```

Example: calculate a factorial of 5.

```
/setvar key=input 5 |
/setvar key=i 1 |
/setvar key=product 1 |
/while left=i right=input rule=lte "/mul product i \| /setvar key=product \| /addvar key=i 1" |
/getvar product |
/echo Factorial of {{getvar::input}}: {{pipe}} |
/flushvar input |
/flushvar i |
/flushvar product
```

## Using the LLM

Scripts can make requests to your currently connected LLM API using the following commands:

- `/gen (prompt)` — generates text using the provided prompt for the selected character and including chat messages.
- `/genraw (prompt)` — generates text using just the provided prompt, ignoring the current character and chat.
- `/trigger` — triggers a normal generation (equivalent to clicking a "Send" button). If in group chat, you can optionally provide a 1-based group member index or a character name to have them reply, otherwise triggers a group round according to the group settings. 

`/genraw lock=on/off stop=[] instruct=on/off (prompt)`

### Arguments for `/gen` and `/genraw`

- `lock` — can be `on` or `off`. Specifies whether a user input should be blocked while the generation is in progress. Default: `off`.
- `stop` — JSON-serialized array of strings. Adds a custom stop string (if the API supports it) just for this generation. Default: none.
- `instruct` (only `/genraw`) — can be `on` or `off`. Allows to use instruct formatting on the input prompt (if instruct mode is enabled and the API supports it). Set to `off` to force pure prompts. Default: `on`.

The generated text is then passed through the pipe to the next command and can be saved to a variable or displaced using the I/O capabilities:

```
/genraw Write a funny message from Cthulhu about taking over the world. Use emojis. |
/popup <h3>Cthulhu says:</h3><div>{{pipe}}</div>
```

| <img alt="image" src="https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/f771f364-8c90-42fa-8822-ee3862e275ce"> |
| -- |

## Access chat messages

### Read messages

You can access messages in the currently selected chat using the `/messages` command.

`/messages names=on/off start-finish`

- The `names` argument is used to specify whether you want to include character names or not, default: `on`.
- In an unnamed argument, it accepts a message index or range in the `start-finish` format. Ranges are inclusive!
- If the range is unsatisfiable, i.e. an invalid index or more messages than exist are requested, then an empty string is returned.
- Messages that are hidden from the prompt (denoted by the ghost icon) are excluded from the output.
- If you want to know the index of the latest message, use the `{{lastMessageId}}` macro, and `{{lastMessage}}` will get you the message itself.

To calculate the start index for a range, for example, when you need to get the last N messages, use variable subtraction.
This example will get you 3 last messages in the chat:

```
/setvar key=start {{lastMessageId}} |
/addvar key=start -2 |
/messages names=off {{getvar::start}}-{{lastMessageId}} |
/setinput
```

### Send messages

A script can send messages as either a user, character, persona, neutral narrator, or add comments.

1. `/send (text)` — adds a message as the currently selected persona.
2. `/sendas name=charname (text)` — adds a message as any character, matching by their name. `name` argument is required. Use the `{{char}}` macro to send as the current character.
3. `/sys (text)` — adds a message from the neutral narrator that doesn't belong to the user or character. The displayed name is purely cosmetic and can be customized with the `/sysname` command.
4. `/comment (text)` — adds a hidden comment that is displayed in the chat but is not visible to the prompt.
5. `/addswipe (text)` — adds a swipe to the last character message. Can't add a swipe to the user or hidden messages.
6. `/hide (message id or range)` — hides one or several messages from the prompt based on the provided message index or inclusive range in the `start-finish` format.
7. `/unhide (message id or range)` — returns one or several messages to the prompt based on the provided message index or inclusive range in the `start-finish` format.

`/send`, `/sendas`, `/sys`, and `/comment` commands optionally accept a named argument `at` with a zero-based numeric value (or a variable name that contains such a value) that specifies an exact position of message insertion. By default new messages are inserted at the end of the chat log.

This will insert a user message at the beginning of the conversation history:

```
/send at=0 Hi, I use Linux.
```

### Delete messages

**These commands are potentially destructive and have no "undo" function. Check the /backups/ folder if you accidentally deleted something important.**

1. `/cut (message id or range)` — cuts one or several messages from the chat based on the provided message index or inclusive range in the `start-finish` format.
2. `/del (number)` — deletes last N messages from the chat.
3. `/delswipe (1-based swipe id)` — deletes a swipe from the last character message based on the provided 1-based swipe ID.
4. `/delname (character name)` — deletes all messages in the current chat that belong to a character with the specified name.
5. `/delchat` — deletes the current chat.

## Text manipulation

There's a variety of useful text manipulation utility commands to be used in various script scenarios.

1. `/trimtokens` — trims the input to the specified number of text tokens from the start or from the end and outputs the result to the pipe.
2. `/trimstart` — trims the input to the start of the first complete sentence and outputs the result to the pipe.
3. `/trimend` — trims the input to the end of the last complete sentence and outputs the result to the pipe.
4. `/fuzzy` — performs fuzzy matching of the input text to the list of strings, outputting the best string match to the pipe.

### Arguments for `/trimtokens`

`/trimtokens limit=number direction=start/end (input)`

1. `direction` sets the direction for trimming, which can be either `start` or `end`. Default: `end`.
2. `limit` sets the amount of tokens to left in the output. Can also specify a variable name containing the number. **Required argument.**
3. Unnamed argument is the input text to be trimmed.

### Arguments for `/fuzzy`

`/fuzzy list=["candidate1","candidate2"] (input)`

1. `list` is a JSON-serialized array of strings containing the candidates. Can also specify a variable name containing the list. **Required argument.**
2. Unnamed argument is the input text to be matched. Output is one of the candidates matching the input most closely.

## Quick Replies: script library and auto-execution

Quick Replies is a built-in SillyTavern extension that provides an easy way to store and execute your scripts.

### Configuring Quick Replies

In order to get started, enable open the extensions panel (stacked blocks icon), and expand the Quick Replies menu.

| <img alt="image" height="300px" src="https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/7f02e1c3-138e-4347-8c16-ebc07c818c28"> |
| -- |

**Quick Replies are disabled by default, you need to enable them first.** Then you will see a bar appearing above your chat input bar.

You can set the displayed button text label (we recommend using emojis for brevity) and the script that will be executed when you click the button.

The number of buttons is controlled by the **Number of slots** settings (max = 100), adjust it according to your needs and click "Apply" when done.

**Inject user input automatically** recommended to be disabled when using STscript, otherwise it may interfere with your inputs, use the `{{input}}` macro to get the current value of the input bar in scripts instead.

**Quick Reply presets** allow to have multiple sets of predefined Quick Replies and switch between manually or by using the `/qrset (name of set)` command.
Don't forget to click "Update" before switching to a different set to write your changes to the currently used preset!

### Manual execution

Now you can add your first script to the library. Pick any free slot (or create one), type "Click me" into the left box to set the label, then paste this into the right box:

```
/addvar key=clicks 1 |
/if left=clicks right=5 rule=eq else="/echo Keep going..." "/echo You did it!  \| /flushvar clicks"
```

Then click 5 times on the button that appeared above the chat bar.
Every click increments the variable `clicks` by one and displays a different message when the value equals 5 and resets the variable.

### Automatic execution

Open the modal menu by clicking the `⋮` button for the created command.

| <img alt="image" height="300px" src="https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/ba8c9ef7-8c8e-445b-94c1-ea71767fa01d"> |
| -- |

In this menu you can do the following:

- Hide the button from the chat bar, making it accessible only for auto-execution.
- Enable automatic execution on one or more of the following conditions:
  * App startup
  * Sending a user message to the chat
  * Receiving an AI message in the chat
  * Opening a character or group chat

Commands are executed automatically only if the Quick Replies extension is enabled.

For example, you can display a message after sending five user messages by adding the following script and setting it to auto-execute on the user message.

```
/addvar key=usercounter 1 |
/echo You've sent {{pipe}} messages. |
/if left=usercounter right=5 rule=gte "/echo Game over! \| /flushvar usercounter"
```

## Calling procedures

A `/run` command can call scripts defined in the Quick Replies by their label, basically providing the ability to define procedures and return results from them. This allows to have reusable script blocks that other scripts could reference. The last result from the procedure's pipe is passed to the next command after it.

`/run ScriptLabel`

Let's create two Quick Replies:

***
**Label:**

`GetRandom`

**Command:**

```
/pass {{roll:d100}}
```
***
**Label:**

`GetMessage`

**Command:**
```
/run GetRandom | /echo Your lucky number is: {{pipe}}
```
***

Clicking on the `GetMessage` button will call the `GetRandom` procedure which will resolve the `{{roll}}` macro and pass the number to the caller, displaying it to the user.

- Procedures do not accept named or unnamed arguments, but can reference the same variables as the caller.
- Avoid recursion when calling procedures as it may produce the "call stack exceeded" error if handled unadvisedly.

## More examples

### Generate chat summary (by @IkariDevGIT)

```
/setvar key=tmp |
/messages 0-{{lastMessageId}} |
/setvar key=s1 |
/echo Generating, please wait... |
/genraw lock=on instruct=off ### Instruct:{{newline}}Summarize the most important facts and events that have happened in the chat given to you in the Input header. Limit the summary to 100 words or less. Your response should include nothing but the summary.{{newline}}{{newline}}### Input:{{newline}}{{getvar::s1}}{{newline}}{{newline}}### Response:{{newline}}The chat summary:{{newline}} |
/setvar key=tmp |
/echo Done! |
/setinput {{getvar::tmp}} |
/flushvar tmp |
/flushvar s1
```

### Buttons popup usage

```
/setglobalvar key=genders ["boy", "girl", "other"] |
/buttons labels=genders Who are you? |
/echo You picked: {{pipe}}
```
