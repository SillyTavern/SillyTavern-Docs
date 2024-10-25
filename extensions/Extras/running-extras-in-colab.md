---
label: Extras via Colab
order: 100
icon: cloud
---
# Running Extras in Colab

!!! Discontinued
The Extras project was discontinued in April 2024 and won't receive any new updates or modules. The vast majority of modules are available natively in the main SillyTavern application. You may still install and use it but don't expect to get immediate support if you face any issues.
!!!

## Instructions

* Open the [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)
* Select the desired "Extra" options
* select `use_cpu` to run Extras without requiring GPU credit
  * this will make Stable Diffusion slower, but everything else will run normally
* Not required, but recommended: select the `secure` option to generate the API key to protect your shared instance.
* Click the Start button on the left (looks like a triangle 'play' button)
* Wait for it to finish loading everything
* Look for the `trycloudflare.com` link at the bottom of the output. Ignore the localhost link, it won't work (we tried!).
* It will start with the text `Running on`
* Copy the API URL link that is listed under that line. (**DO NOT copy the 'localhost' URL, use the other one**)
* Start SillyTavern with extensions support: (set `enableExtensions` to `true` in your `config.yaml` if necessary)
* Navigate to SillyTavern's Extensions menu (click the 'stacked blocks' icon at the top of the page).
* Paste the API URL into the box at the top. (**NOT the API Key box**)
* If you have NOT enabled the `secure` option, make sure the API Key box is completely empty when using the official colab.
* If you have enabled the `secure` option, paste the generated API key into the API Key box.
* API key will appear in the colab's console output, for example: `Your API key is fee2f3f559`
* Click "Connect"
