---
order: 103
---

# Self-hosted AI models

#### \[UPDATE 2023-12-18\] After this guide was written, feedback came that the newly-released Mixtral MoE 8x7B model is far superior to any self-hosted model that came before it, including any larger model. It can do higher-quality chat role-playing than even GPT4 can. Everything taught in this guide is still valid, but if you have the hardware to run a Mixtral model, then be sure to use that model!

## Intro

This guide aims to help you get set up using SillyTavern with a local AI running on your PC (we'll start using the proper terminology from now on and call it an LLM). Read it before bothering people with tech support questions.

## Hardware requirements and orientation

This is a complex subject, so I'll stick to the essentials and generalize.

* There are thousands of free LLMs you can download from the Internet, similar to how Stable Diffusion has tons of models you can get to generate images.
* Running an unmodified LLM requires a monster GPU with a ton of VRAM (GPU memory). More than you will ever have.
* It is possible to reduce VRAM requirements by compressing the model using quantization techniques, such as GPTQ or AWQ. This makes the model somewhat less capable, but greatly reduces the VRAM requirements to run it. Suddenly, this allowed people with gaming GPUs like a 3080 to run a 13B model. Even though it's not as good as the unquantized model, it's still good.
* There also exists a model format and quantization called GGUF (previously GGML). This allows you to use a LLM without a GPU at all. It will only use CPU and RAM. This is much slower (probably 15 times) than running the LLM on a GPU using GPTQ/AWQ, especially during the prompt processing, but the model's ability is just as good. The GGUF creator then optimized it further and made it so that you're able to offload parts of the model to your GPU. This means if your GPU doesn't have enough VRAM to run a GPTQ/AWQ model, you can still have your GPU assist the CPU with the limited VRAM it has, and their combined forces can run the LLM at a faster speed than if you'd used CPU alone (but still way slower than GPU).
* There are different sizes of models, named based on the number of parameters they were trained with. You will see names like 7B, 13B, 30B, 70B, etc. You can think of these as the brain size of the model. A 13B model will be more capable than the 7B from the same family of models: they were trained on the same data, but the bigger brain can retain the knowledge better and think more coherently. Bigger models also require more VRAM if using GPTQ/AWQ models, and more RAM if using GGUF models.
* There are several degrees of quantization (8-bit, 5-bit, 4-bit, etc). The lower you go, the more the model degrades, but the lower the hardware requirements. So even on bad hardware, you might be able to run a 4-bit version of your desired model. There's even 3-bit and 2-bit quantization but at this point, you're beating a dead horse.
* Sometime in 2023, NVIDIA changed their GPU driver so that if you need more VRAM than your GPU has, instead of the task crashing, it will begin using regular RAM as a fallback. This will ruin the writing speed of the LLM, but the model will still work and give the same quality of output. Thankfully, this behavior [can be disabled](https://nvidia.custhelp.com/app/answers/detail/a_id/5490).
* Misc note: the context size of Llama2-based models is 4k. Mistral is typically 8k, but some models are higher.

Given all of the above, the hardware requirements and performance vary completely depending on the family of model, the type of model, the size of the model, the quantization method, etc. 

TL;DR: Here are some general rules for the popular Llama-based models:

4-bit quantized GPU models (GPTQ):
* 7B models need 6GB VRAM
* 13B needs 10GB VRAM
* 30B needs 20GB VRAM

4-bit quantized CPU models (GGUF):
* 7B needs 8GB RAM
* 13B needs 11GB RAM
* 30B needs 23GB RAM

(but keep in mind your OS and tools need RAM too, so don't be trying a 13B model if you only have 12GB RAM)

Remember, you want to run the largest, least quantized model that can fit in your hardware specs. You WILL have to experiment using some trial-and-error.

\[UPDATE 2023-12-18: to run Mixtral MoE models, preliminary reports say you need 8GB VRAM and 26GB RAM for a 4-bit Q_M quantized GGUF model, run in split CPU/GPU mode with 7 layers offloaded to the GPU. This achieves 6 tokens/sec. If you have more concrete info, please update this page.\]

## Downloading a LLM

To get started, you will need to download a LLM. The most common place to find and download LLMs is on HuggingFace. There are thousands of models available. A good way to find models is to check TheBloke's account page: <https://huggingface.co/TheBloke>. He's a one-man army dedicated to converting every model to GGUF. If you don't want GGUF, he links the original model page where you might find other formats for that same model.

On a given model's page, you will find a whole bunch of files. 

* You might not need all of them! For GGUF, you just need the .gguf model file (usually 4-11GB). If you find multiple large files, it's usually all different quantizations of the same model, you only need to pick one. 
* For .safetensors files (which can be GPTQ or AWQ or HF quantized or unquantized), if you see a number sequence in the filename like model-00001-of-00003.safetensors, then you need all 3 of those .safetensors files + all the other files in the repository (tokenizer, configs, etc.) to get the full model.

### Walkthrough for downloading OpenHermes-2.5-Mistral-7B-GGUF

We will use the OpenHermes-2.5-Mistral-7B-GGUF model for the rest of this guide. Based on my experiments, this is the best model I could find for chat/roleplaying, it has an 8k context size, and there are even 16k context variants.

* Go to <https://huggingface.co/TheBloke/OpenHermes-2.5-Mistral-7B-GGUF>
* Click 'Files and versions'. You will see a listing of several files. These are all the same model but offered in different quantization options. Click the file 'openhermes-2.5-mistral-7b.Q4_K_S.gguf', which gives us a 4-bit quantization.
* Click the 'download' button. Your download should start.

### How to identify the type of model

Good model uploaders like TheBloke give descriptive names. But if they don't:

* Filename ends in .gguf: GGUF CPU model (duh)
* Filename ends in .safetensors: can be unquantized, or HF quantized, or GPTQ, or AWQ
* Filename is pytorch-***.bin: same as above, but this is an older model file format that allows the model to execute arbitrary Python script when the model is loaded, and is considered unsafe. You can still use it if you trust the model creator, or are desperate, but pick .safetensors if you have the option. 
* config.json exists? Look if it has a quant_method.
* q4 means 4-bit quantization, q5 is 5-bit quantization, etc
* There's a further quantization subtype called k_s or k_m. k_m is better than k_s but requires more resources.
* You see a number like -16k? That's an increased context size (i.e. how long your conversation can get before the model forgets the beginning of your chat)! Note that higher context sizes require more VRAM.

## Installing a LLM server: Oobagooba or KoboldAI

With the LLM now on your PC, we need to download a tool that will act as a middle-man between SillyTavern and the model: it will load the model, and expose its functionality as a local HTTP web API that SillyTavern can talk to, the same way that SillyTavern talks with paid webservices like OpenAI GPT or Claude. The tool you use should be either KoboldAI or Oobagooba (or other compatible tools). 

The rest of this guide will continue using Oobagooba, but these tools should be considered equivalent.

### Installing Oobagooba

Here's a more correct/dummy proof installation procedure:

1. git clone <https://github.com/oobabooga/text-generation-webui> (or download their repo as a .zip in your browser, then extract it)
2. Run start_windows.bat or whatever your OS is
3. When asked, select your GPU type. Even if you intend to use GGUF/CPU, if your GPU is in the list, select it now, because it will give you the option to use a speed optimization later called GPU sharding (without having to reinstall from scratch). If you have no gaming-grade dGPU (NVIDIA, AMD), select None.
4. Wait for the installation to finish
5. Place openhermes-2.5-mistral-7b.Q4_K_S.gguf in text-generation-webui/models
6. Open text-generation-webui/CMD_FLAGS.txt, delete everything inside and write: --api
7. Restart oobagooba
8. Visit <http://127.0.0.1:5000/docs>. Does it load a FastAPI page? If not, you messed up somewhere.

### Loading our model in Oobagooba

1. Open <http://127.0.0.1:7860/> in your browser
2. Click the Model tab
3. In the dropdown, select our OpenHermes model. It should have automatically selected the llama.cpp loader.
4. Click Load

Notice on the right side it said "It seems to be an instruction-following model with template ChatML". Don't take this as gospel! Ooba is incapable of guessing how the fine-tuners of a model trained it. Knowing this will come into play later when trying to optimize the writing quality in SillyTavern.

### Configuring SillyTavern to talk to Oobagooba

1. Click API Connections (2nd button in the top bar)
2. Set API to Text Completion
3. Set API Type to Default (Oobagooba)
4. Set server URL to <http://127.0.0.1:5000/>
5. Click Connect. It should connect successfully and detect TheBloke_OpenHermes-2.5-Mistral-7B-GGUF as the model.
6. Chat with a character to test that it works

## Conclusion

Congrats, you should now have a working local LLM.

## Part 2: Configuring SillyTavern to get better outputs from the LLM

So you chatted with the bot and it kind of sucks. Maybe it's a bad model. Maybe you're incapable of running better models and this is as good as it gets. But maybe you can fix her.

Read on in the next guide, [How To Improve Output Quality](https://docs.sillytavern.app/usage/local-llm-guide/how-to-improve-output-quality/)
