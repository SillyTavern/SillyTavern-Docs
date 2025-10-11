---
icon: paperclip
route: /usage/core-concepts/connection-profiles/
order: 100
---

# Connection Profiles

Save Connection Profiles to quickly switch between different APIs, models and formatting templates. This is useful when you actively use multiple API connections or need to switch between different configurations without surfing through the menus.

## Accessing Connection Profiles

This feature is enabled by default starting from SillyTavern 1.12.6 or later as a built-in extension, and available in the API Connections menu. If you wish to *disable* it, open the Extensions panel, click on "Manager extensions", locate Connection Profiles in the list, uncheck the "Enabled" checkbox, and then click "Close".

## What is Saved

Connection Profiles store the following selections.

### Common

* [API type, model and the server URL](/Usage/API_Connections/index.md)
* [Secret Key](/Usage/faq.md#where-are-my-api-keys-stored-why-cant-i-see-them)
* [Settings preset](/Usage/Common-Settings.md)
* [Start Reply With](/Usage/Prompts/advancedformatting.md#start-reply-with) (can be explicitly empty)
* [Custom Stopping Strings](/Usage/Prompts/advancedformatting.md#custom-stopping-strings) (can be explicitly empty)
* [Reasoning Formatting](/Usage/Prompts/reasoning.md#configuration)

### Text Completion APIs

* [System Prompt and its state](/Usage/Prompts/advancedformatting.md#system-prompt)
* [Instruct Mode state and template](/Usage/Prompts/instructmode.md)
* [Context Template](/Usage/Prompts/advancedformatting.md#context-template)
* [Tokenizer](/Usage/Prompts/advancedformatting.md#tokenizer)

### Chat Completion APIs

* [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing)
* Proxy preset

## Managing Connection Profiles

!!!info Note
Profiles only save the selection in dropdown fields, without knowing anything about the underlying settings. This means that you will lose unsaved changes by switching to a different profile. To prevent this, make sure to update all presets and templates if you don't want to lose ephemeral changes.
!!!

* To save a profile, set all the required settings and click the "Create" button. Then review the settings and provide a name for the profile. **A name should be unique.**
* To view the detailed information about a chosen profile, click on the "Information" button. Click again to hide the details.
* Connection Profile settings are saved into `settings.json` without altering the associated profile save file until you press the "Update" button. This means that if you setup a profile, but then switch to a different one without updating, you will lose all of your previous changes.
* To restore the changed selections from a saved profile, click the "Reload" button.
* To delete a profile, click the "Delete" button and confirm the deletion. **This action is irreversible.**

## Slash Commands

Connection profiles can be managed using the following slash commands.

1. `/profile [name]` - switch to a profile if the argument is provided, or get the name of the current profile if not.
2. `/profile-create [name]` - saves the current settings as a new profile with the provided name.
3. `/profile-list` - returns a JSON-serialized array of available profile names.
4. `/profile-get [name]` - gets the details of the profile with the provided name as a JSON-serialized object.
5. `/profile-update` - updates the selected profile with the current settings.
