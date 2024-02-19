---
order: 103
---

# Self-hosted AI models

## Intro

This guide aims to help you get set up using SillyTavern with a local AI running on your PC (we'll start using the proper terminology from now on and call it an LLM). Read it before bothering people with tech support questions.

## Hardware requirements and orientation

This is a complex subject, so I'll stick to the essentials and generalize.

* There are thousands of free LLMs you can download from the Internet, similar to how Stable Diffusion has tons of models you can get to generate images.
* Running an unmodified LLM requires a monster GPU with a ton of VRAM (GPU memory). More than you will ever have.
* It is possible to reduce VRAM requirements by compressing the model using quantization techniques, such as GPTQ or AWQ. This makes the model somewhat less capable, but greatly reduces the VRAM requirements to run it. Suddenly, this allowed people with gaming GPUs like a 3080 to run a 13B model. Even though it's not as good as the unquantized model, it's still good.
* It gets better: there also exists a model format and quantization called GGUF (previously GGML) which has become the format of choice for normal people without monster GPUs. This allows you to use an LLM without a GPU at all. It will only use CPU and RAM. This is much slower (probably 15 times) than running the LLM on a GPU using GPTQ/AWQ, especially during the prompt processing, but the model's ability is just as good. The GGUF creator then optimized GGUF further by adding a configuration option that allows people with a gaming-grade GPU to offload parts of the model to the GPU, allowing them to run part of the model at GPU speed (note that this doesn't reduce RAM requirements, it only improves your generation speed).
* There are different sizes of models, named based on the number of parameters they were trained with. You will see names like 7B, 13B, 30B, 70B, etc. You can think of these as the brain size of the model. A 13B model will be more capable than the 7B from the same family of models: they were trained on the same data, but the bigger brain can retain the knowledge better and think more coherently. Bigger models also require more VRAM/RAM.
* There are several degrees of quantization (8-bit, 5-bit, 4-bit, etc). The lower you go, the more the model degrades, but the lower the hardware requirements. So even on bad hardware, you might be able to run a 4-bit version of your desired model. There's even 3-bit and 2-bit quantization but at this point, you're beating a dead horse. There's also a further quantization subtypes named k_s, k_m, k_l, etc. k_m is better than k_s but requires more resources.
* The context size (how long your conversation can become without the model dropping parts of it) also affects VRAM/RAM requirements. Thankfully, this is a configurable setting, allowing you to use a smaller context to reduce VRAM/RAM requirements. (Note: the context size of Llama2-based models is 4k. Mistral is advertised as 8k, but it's 4k in practice.)
* Sometime in 2023, NVIDIA changed their GPU driver so that if you need more VRAM than your GPU has, instead of the task crashing, it will begin using regular RAM as a fallback. This will ruin the writing speed of the LLM, but the model will still work and give the same quality of output. Thankfully, this behavior [can be disabled](https://nvidia.custhelp.com/app/answers/detail/a_id/5490).

Given all of the above, the hardware requirements and performance vary completely depending on the family of model, the type of model, the size of the model, the quantization method, etc.

#### Model size calculator
You can use [Nyx's Model Size Calculator](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator) to determine how much RAM/VRAM you need.

Remember, you want to run the largest, least quantized model that can fit in your memory, i.e. without causing [disk swapping](https://serverfault.com/a/48487).

## Downloading an LLM

To get started, you will need to download an LLM. The most common place to find and download LLMs is on HuggingFace. There are thousands of models available. A good way to find models is to check TheBloke's account page: <https://huggingface.co/TheBloke>. He's a one-man army dedicated to converting every model to GGUF. If you don't want GGUF, he links the original model page where you might find other formats for that same model.

On a given model's page, you will find a whole bunch of files. 

* You might not need all of them! For GGUF, you just need the .gguf model file (usually 4-11GB). If you find multiple large files, it's usually all different quantizations of the same model, you only need to pick one. 
* For .safetensors files (which can be GPTQ or AWQ or HF quantized or unquantized), if you see a number sequence in the filename like model-00001-of-00003.safetensors, then you need all 3 of those .safetensors files + all the other files in the repository (tokenizer, configs, etc.) to get the full model.
* As of January 2024, Mixtral MOE 8x7B is widely considered the state of the art for local LLMs. If you have the 32GB of RAM to run it, definitely try it. If you have less than 32GB of RAM, then use Kunoichi-DPO-v2-7B, which despite its size is stellar out of the gate.

### Walkthrough for downloading Kunoichi-DPO-v2-7B

We will use the Kunoichi-DPO-v2-7B model for the rest of this guide. It's an excellent model based on Mistral 7B, that only requires 7GB RAM, and punches far above its weight. Note: Kunoichi uses Alpaca prompting.

* Go to <https://huggingface.co/brittlewis12/Kunoichi-DPO-v2-7B-GGUF>
* Click 'Files and versions'. You will see a listing of several files. These are all the same model but offered in different quantization options. Click the file 'kunoichi-dpo-v2-7b.Q6_K.gguf', which gives us a 6-bit quantization.
* Click the 'download' button. Your download should start.

### How to identify the type of model

Good model uploaders like TheBloke give descriptive names. But if they don't:

* Filename ends in .gguf: GGUF CPU model (duh)
* Filename ends in .safetensors: can be unquantized, or HF quantized, or GPTQ, or AWQ
* Filename is pytorch-***.bin: same as above, but this is an older model file format that allows the model to execute arbitrary Python script when the model is loaded, and is considered unsafe. You can still use it if you trust the model creator, or are desperate, but pick .safetensors if you have the option. 
* config.json exists? Look if it has a quant_method.
* q4 means 4-bit quantization, q5 is 5-bit quantization, etc
* You see a number like -16k? That's an increased context size (i.e. how long your conversation can get before the model forgets the beginning of your chat)! Note that higher context sizes require more VRAM.

## Installing an LLM server: Oobabooga or KoboldAI

With the LLM now on your PC, we need to download a tool that will act as a middle-man between SillyTavern and the model: it will load the model, and expose its functionality as a local HTTP web API that SillyTavern can talk to, the same way that SillyTavern talks with paid webservices like OpenAI GPT or Claude. The tool you use should be either KoboldAI or Oobabooga (or other compatible tools). 

The rest of this guide will continue using Oobabooga, but these tools should be considered equivalent.

### Installing Oobabooga

Here's a more correct/dummy proof installation procedure:

1. git clone <https://github.com/oobabooga/text-generation-webui> (or download their repo as a .zip in your browser, then extract it)
2. Run start_windows.bat or whatever your OS is
3. When asked, select your GPU type. Even if you intend to use GGUF/CPU, if your GPU is in the list, select it now, because it will give you the option to use a speed optimization later called GPU sharding (without having to reinstall from scratch). If you have no gaming-grade dGPU (NVIDIA, AMD), select None.
4. Wait for the installation to finish
5. Place kunoichi-dpo-v2-7b.Q6_K.gguf in text-generation-webui/models
6. Open text-generation-webui/CMD_FLAGS.txt, delete everything inside and write: --api
7. Restart Oobabooga
8. Visit <http://127.0.0.1:5000/docs>. Does it load a FastAPI page? If not, you messed up somewhere.

### Loading our model in Oobabooga

1. Open <http://127.0.0.1:7860/> in your browser
2. Click the Model tab
3. In the dropdown, select our Kunoichi DPO v2  model. It should have automatically selected the llama.cpp loader.
4. (Optional) We mentioned 'GPU offload' several times earlier: that's the n-gpu-layers setting on this page. If you want to use it, set a value before loading the model. As a basic reference, setting it to 30 uses just under 6GB VRAM for 13B and lower models. (it varies with model architecture and size)
5. Click Load


### Configuring SillyTavern to talk to Oobabooga

1. Click API Connections (2nd button in the top bar)
2. Set API to Text Completion
3. Set API Type to Default (Oobabooga)
4. Set server URL to <http://127.0.0.1:5000/>
5. Click Connect. It should connect successfully and detect kunoichi-dpo-v2-7b.Q6_K.gguf as the model.
6. Chat with a character to test that it works

## Conclusion

Congrats, you should now have a working local LLM.

## Part 2: Configuring SillyTavern to get better outputs from the LLM

So you chatted with the bot and it kind of sucks. Maybe it's a bad model. Maybe you're incapable of running better models and this is as good as it gets. But maybe you can fix her.

Read on in the next guide, [How To Improve Output Quality](https://docs.sillytavern.app/usage/local-llm-guide/how-to-improve-output-quality/)
