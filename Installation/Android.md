---
label: Android (Termux)
route: /installation/android-(termux)/
---

# Android (Termux) Installation

SillyTavern can be run natively on Android phones using Termux.

Consider installing this keyboard
https://github.com/Julow/Unexpected-Keyboard
It makes several commands easy

## Get termux:
https://github.com/termux/termux-app/releases

Install the termux apk you just downloaded.

Open Termux. run your first command"

`termux-change-repo`

Select "Mirror group" then your closest servers. You can touch the screen or use swipe with Unexpected-Keyboard.

update Termux:

`pkg update && pkg upgrade`

## install things:

`pkg install git nodejs nano`

Note: Are you running 32bit android? See ## Common errors below

## Install SillyTavern.

Clone the SillyTavern repo
for Release Branch: `git clone https://github.com/SillyTavern/SillyTavern -b release`
for Staging Branch: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

Just clone the Staging and thank me later.

Create some aliases.

`nano ~/.bashrc`

Copy and paste this
`# Aliases
# alias alias_name="command_to_run"

#Update Termux
alias pkgup="pkg update && pkg upgrade"
#Update SillyTavern
alias stup="git -C "SillyTavern" pull"
#Update termux. Update and run SillyTavern
alias st="pkgup && stup && bash ~/SillyTavern/start.sh"`

Hit these keys to save and leave nano:
To Exit: ctrl+X
Do you want to save?: Y
Save as .bashrc?: Hit enter
Apply the .bashrc
`source ~/.bashrc`

Update termux. You will need to hit 'y' when it asks. Then Update and run SillyTavern: 
`st`

Just Update termux. 
`pkgup`

To start Sillytavern
`stup`

## Common errors

### Unsupported platform: android arm LEtime-web
32-bit Android requires an external dependency that can't be installed with npm.

Use the following command to install it:

`pkg install esbuild`

Then run the steps above.
