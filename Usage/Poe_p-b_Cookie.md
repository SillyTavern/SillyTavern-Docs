# How to get Poe API p-b cookie

First make an account on poe.com, then follow these instructions for your OS:

## PC/Mac/Linux

1. Using your browser, login to Poe.com
2. Open the dev tools (F12 for Chrome)
3. At the top of the DevTools view you will see a list of tabs, beginning with `Elements`.
4. Depending on the width of your screen, some other tabs might be visible after `Elements`, but to the right you will see a `>>` button.
5. Click the `>>` button and select 'Application'.
6. Look for 'Storage' in the left hand side of DevTools Application view
7. Look for 'Cookies', expand it if necessary.
8. Look for 'poe.com', click
9. On the right you will see 'p-b'
10. Copy the value of the cookie
11. Save it in a text file somewhere for future reference.
12. Paste this value into SillyTavern's Poe API box.

## iOS

### Download [Orion Browser](https://apps.apple.com/en-us/app/orion-browser-by-kagi/id1484498200)

#### (it has firefox and chrome extensions support)

### Enable extensions in the Orion browser

- Tap '...' button (bottom right)
- Tap Settings
- scroll down until you see the 'Extensions' section
- Enable Firefox

### Configure the browser User Agent

- stay inside the settings view
- scroll up until you see "User Agent", tap it.
- select Custom
- in the text field below, delete everything and just type "Desktop"
- close Settings panel

### Install Cookie-Editor Firefox extension

- Tap '...' button
- Tap 'Extensions'
- Tap "+" button (bottom left)
- Tap "Install Firefox extension"
- (a new browser tab will open showing the Firefox extensions website)
- Type "Cookie-Editor" into the search box at top of the page
- (Many options will show up in the auto-complete)
- tap one that says "Cookie-Editor" it should say 'by cgagnier' as the author
- Tap "Add to Orion" button

### Go to Poe.com, login

(you don't need to chat with the bot, just letting the page load is enough)

### Get your pb-cookie

- tap '...' button
- 'Cookie-Editor' will show up at the top of the menu, tap that
- an overlay panel will popup, showing 'p-b'
- tap the down arrow next to 'p-b' expand the entry
- Copy what's inside the Value box.

### Paste it into ST as usual in the Poe API panel box

## Android

1. Install [kiwi browser](https://play.google.com/store/apps/details?id=com.kiwibrowser.browser&hl=en&gl=US&pli=1)
2. Use the browser's devtools in the exact same way as mentioned above in the PC instructions.
