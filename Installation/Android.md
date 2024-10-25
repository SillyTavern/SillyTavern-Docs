---
label: Android (Termux)
---
# Android (Termux) Installation

SillyTavern can be run natively on Android phones using Termux.

Please refer to this guide by ArroganceComplex#2659:

<https://rentry.org/STAI-Termux>

## Common errors

### Unsupported platform: android arm LEtime-web
32-bit Android requires an external dependency that can't be installed with npm.

Use the following command to install it:

`pkg install esbuild`

Then run the steps from the guide.
