---
label: Extras via Colab
order: 100
icon: cloud
---
# Running Extras via Colab

Instructions to run the SillyTavern Extras Colab.

* Open the [Official Extras Colab](https://colab.research.google.com/github/Cohee1207/SillyTavern/blob/main/colab/GPU.ipynb)
* Select the desired "Extra" options
* select `use_cpu` to run Extras without requiring GPU credit
  * this will make Stable Diffusion slower, but everything else will run normally
* Click the Start button on the left (looks like a triangle 'play' button)
* Wait for it to finish loading everything
* Look for `trycloudflare.com` link at the bottom of the output. Ignore the localhost link, it won't work (we tried!).
* It will start with the text `Running on`
* Copy the API URL link that is listed under that line. (**DO NOT copy the 'localhost' URL, use the other one**)
* Start SillyTavern with extensions support: (set `enableExtensions` to `true` in your `config.conf` if necessary)
* Navigate to SillyTavern's Extensions menu (click the 'stacked blocks' icon at the top of the page).
* Paste the API URL into the box at the top. (**NOT the API Key box**)
* Make sure the API Key box is completely empty when using the official colab.
* Click "Connect"
