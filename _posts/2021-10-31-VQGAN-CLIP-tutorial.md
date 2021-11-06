---
layout: post
title: "1B: VQGAN + CLIP Tutorial"
meta_title: "VQGAN + CLIP Tutorial"
description: "I GAN Explain: VQGAN + CLIP Tutorial and Colab Walkthrough by Casual GAN Papers"
date: 2021-10-31 19:00:00 -0000
categories: howto-CLIP-VQGAN-text-guided-image-generation-explained
---

#### I GAN Explain: VQGAN + CLIP Tutorial and Colab Walkthrough

##### ⭐Tutorial difficulty: 🌕🌕🌑🌑🌑

***

![VQGAN + CLIP teaser](/assets/images/clipvqgan_teaser.png "VQGAN + CLIP Teaser")

Hi everyone!

Welcome to "I GAN Explain", a new series by Casual GAN Papers that will cover everything you need to know to get started in the world of generative art, ai-based image editing, 3D generation, and more!  
Today we will cover VQGAN + CLIP - a very popular twitter-sourced text guided image generation method that somehow still has not made an appearance in any conference published paper.  
There are many variations of this notebook, but the most intuitive one I found is by **Justin John** modified from **Katherine Crowson**.  
Get the colab [here](https://colab.research.google.com/github/justinjohn0306/VQGAN-CLIP/blob/main/VQGAN%2BCLIP_%28z%2Bquantize_method_with_augmentations%2C_user_friendly_interface%29.ipynb?authuser=1#scrollTo=c2505191-1756-4e5d-b9a8-33af798ad879), and let's get started!  

1) This colab is released under the MIT license, which allows it to be used for commercial projects, which is a great thing for anyone wanting to use this method in a startup.  

2) This notebook is quite heavy, hence it requires a powerful GPU to run, make sure that you have enough juice, otherwise reset and reconnect.  

3) It takes about 10 minutes to generate a good looking image for a single prompt, and colab likes to disconnect you from the GPU instance if you are not actively editing code (even if you are already running some code on the GPU). Luckily, the authors added an "anti-disconnect" block that implicitly clicks "reconnect" if colab attempts to kick you off the GPU. This way you can input a prompt and forget about it until a good looking image is generated.  

4) This cell just downloads and installs the necessary models from the official repositories: CLIP, VQGAN, along with several utility libraries.  

5) Next, you got to select, which VQGAN models to download. The type of model determines the domain of the images that it best generates.
Here are your options:  
- *imagenet_1024(16384)*: the default model that sort of does everything, comes in two sizes  
- *coco*: alternative option to imagenet  
- *faceshq*: FFHQ - faces  
- *wikiart_1024(16384)*: classical art  
- *sflckr*: also for paintings  
- *openimages_8192*: alternative option to imagenet  

6) Mostly utility methods and model loaders here  

7) Settings are mostly self-explanatory. I found that 10k iterations was enough to generate good looking images most of the time. Providing an initial image speeds things up a bit if you are looking to have a specific structure in an image. Think of this setting as a switch to turn this task into an img2img problem.

8) This is the juiciest part of the notebook:  
- The input vector Z is created either from random noise or by embedding the input image with the VQGAN encoder. In either case it is [vector-quantized](https://t.me/casual_gan/46)  
- Likewise, the prompts are encoded with the CLIP encoder. Additionally, the target images are also encoded with CLIP if you provided them in the previous step.  
- Finally, an image is synthesized from the current quantized Z vectors with the VQGAN decoder, normalized and clipped to \[0, 1\]  
- To calculate the loss, the synthesized image is embedded with CLIP, and the CLIP distances to each of the prompts is added to the total loss along with a slowly decaying MSE regularizer on the current Z vectors so that they do not deviate too much from the initial image in case it was provided.  
- At each iteration Z is updated with gradient descent on the total loss and clamped to the minimum and maximum values in the pretrained VQGAN codebook. The previous two steps are repeated until interrupted or until the number of iterations reaches the maximum.  

At this point feel free to experiment with prompts, and send me the cool images that you get as a result!

I want to finish this quick tutorial with a couple of lifehacks on prompt-building that the community has figured out while experimenting with VQGAN + CLIP:  
- Broad abstract prompts usually yield the most visually appealing results  
- If you want your image to follow a specific structure, use a starting image  
- Prompts are usually formulated as: "a \[decriptive adjective - beautiful, abstract, disgusting, magical, anime, etc\] \[subject - whatever you want to generate\] (optionally if you are using one of the art/painting models: by \[Painter name\]) (optionally, altough highly recommended: \[combination of modifiers, 1-2 at a time, refer to [this extensive list](https://imgur.com/a/SnSIQRu) for inspiration\]"  
- Example prompts to get you started (taken from twitter): "Cyberpunk City artstationHQ", "a clockwork angel, trending on ArtStation", "Rise of consciousness trending on artstation", "A treehouse is seen in the middle of a valley artstation"  

That is all for today, thank you for reading!

Take care,  
-Kirill  

##### 🔗 Links:
[VQGAN + CLIP huggingface space](https://huggingface.co/spaces/akhaliq/VQGAN_CLIP) - slower with pretty UI  
[VQGAN + CLIP colab](https://colab.research.google.com/drive/1L8oL-vLJXVcRzCFbPwOoMkPKJ8-aYdPN#scrollTo=g7EDme5RYCrt) - faster text-based UI

***

##### 🔥 Dive deeper:  
- [CLIP explained]({% post_url 2021-9-14-CLIP-explained %})  
- [VQGAN explained]({% post_url 2021-6-1-VQGAN%})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
