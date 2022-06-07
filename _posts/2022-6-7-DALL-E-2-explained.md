---
layout: post
title: "87: DALL-E 2 (unCLIP)"
meta_title: "DALL-E 2 (unCLIP)"
description: "Hierarchical Text-Conditional Image Generation with CLIP Latents by Aditya Ramesh, Prafulla Dhariwal, Alex Nichol, Casey Chu et al. explained in 5 minutes"
date: 2022-6-7 16:00:00 -0000
categories: sota-text-to-image-openai-clip-dalle
---

#### Hierarchical Text-Conditional Image Generation with CLIP Latents by Aditya Ramesh, Prafulla Dhariwal, Alex Nichol, Casey Chu et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![DALL-E 2 Model](/assets/images/dalle2_preview.jpeg "DALL-E 2 Teaser")

##### 🎯 At a glance:

Does this paper even need an introduction? This is DALL-E 2 and if you have somehow not heard of it yet, sit down. Your mind is about to be blown.  

Enough said. Let’s dive in, shall we?

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Tip Casual GAN Papers', '#2b2f3c', 'V7V7BXBHV');kofiwidget2.draw();</script> 

***

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1. [CLIP]({% post_url 2021-9-14-CLIP-explained %})  
2. [Diffusion]({% post_url 2021-12-26-Guided-Diffusion-explained %})  
3. [GLIDE]({% post_url 2022-4-25-GLIDE-explained %})  
4. [LDMs]({% post_url 2022-5-3-Latent-Diffusion-Models-explained %})  

##### 🚀 Motivation:

DALL-E 2 feels like the pinnacle of evolution of text-to-image models, the final boss and if you have been reading Casual GAN Papers, then you are in luck, because we have covered all of the prerequisites in previous posts. No, seriously, if you missed any, go back and read them. Now! Done? Good. The pitch is simple, let’s make the ultimate text-to-image model using all of the recent breakthroughs from diffusion in combination with the monstrous dataset of combined 650 million text-image pairs from CLIP and DALL-E.  

***

##### 🔍 Main Ideas:

**1. Overview:**  
DALL-E 2 or unCLIP, as it referred to here, consists of a prior that maps the CLIP text embedding to a CLIP image embedding and a diffusion decoder that outputs the final image, conditioned on the predicted CLIP image embedding.  

**2. Decoder:**  
The decoder is based on GLIDE with classifier-free guidance. It additionally receives projected CLIP embeddings added to the tilmestep embedding and four more tokens of separately projected CLIP embeddings concatenated as context to the outputs of the GLIDE text encoder. The original text conditioning pathway from GLIDE is not used.  

In addition to the base decoder, two upsampling diffusion models are trained to take images from 64 to 256 and from 256 to 1024 resolution respectively. These models are unconditional and do not use attention layers, only convolutions. This helps speed up training and reduce the compute requirements.  

**3. Prior:**  
Two types of prior models were considered: autoregressive and diffusion. The first converts a CLIP embedding into a sequence of discrete codes predicted autoregressively conditioned on the caption. The second models the CLIP image embedding vector directly with a Gaussian diffusion model conditioned on the caption.  

To train the autoregressive prior the authors use PCA on the CLIP image embeddings to select the 319 most important dimensions and quantize them into 1024 discrete tokens. The transformer prior then learns to model the sequence of these tokens. The conditions are prepended to the sequence along with a quantized token that represents how good the input caption matches the image. This enables explicit control of how much leeway the model has when generating an image for a caption at inference.  

The diffusion prior is a decoder-only transformer that is trained on a sequence consisting of the encoded text, the CLIP text embedding, the diffusion tilmestep embedding, the noised CLIP image embedding and the final token that holds the unnoised CLIP image embedding after diffusion. The diffusion prior is not conditioned on the text-image matching score. Instead, two samples are produced at inference, and the one with the better score is selected.  

**4. Image Manipulations:**  
DALL-E two is not a boring model and it can create variations of input images, interpolate between images and perform “text diffs” among other things. Such a feat is achieved by encoding images into the latent space of the decoder represented as two vectors, one for describing what the CLIP models sees, the other for all of the residual information needed to reconstruct the image. The first is obtained from the CLIP image encoder, the other by inverting the decoder (obtaining the latent vector that best reconstructs the image).  

Circling back to the beautiful use cases for these embeddings we have:  

Semantically similar variations of the input image with remixed content are obtained by freezing the CLIP image embedding and resampling the later steps of the diffusion decoder for the other latent.  

Interpolations between two images by applying SLERP to both types of embeddings.  

Text diffs for text-guided image editing by subtracting the original CLIP text embedding from the CLIP text embedding of the new caption and SLERPing this normalized difference to the CLIP image embedding of the input image to make it more like the edit prompt.  

**5. Probing the CLIP space:**  
Another set of insightful, albeit less fun (in my opinion) experiments consist of applying PCA to the CLIP embedding to see how much and what type of information each component contains and generating variations of input images containing typographic attacks (the word IPOD is written over an Apple). Interestingly, the neighboring images still contain an apple despite the class label “IPOD” has a much higher probability. I believe this is also the source of the insane [“secret language” of DALL-E 2](https://twitter.com/giannis_daras/status/1531693093040230402?s=20&t=HSm5WI_POT7zrLOf4yroOA)  

##### 📈 Experiment insights / Key takeaways:

- *Baselines:* GLIDE, AttnGAN, DM-GAN, DF-GAN, XMC-GAN, LAFITTE, Make-A-Scene  
- *Datasets:* MS-COCO  
- *Metrics:* FID, User Study  
- *Qualitative:* Yes, the images are as awesome as the authors claim they are  
- *Quantitative:* The diffusion prior gets several points over the autoregressive prior, while GLIDE wins on photorealism and caption similarity and unCLIP takes home the diversity trophy from the user study. Playing with the clasisfier guidance scale let’s unclip achieve GLIDE’s levels of photorealism at the cost of diversity and caption similarity.  
- *Additional:* The model can be trained without the CLIP text to CLIP image prior, although with a significant drop in quality.    

***

##### 🖼️ Paper Poster:

![DALL-E 2 poster](/assets/images/dalle2.jpg "DALL-E 2Poster")

***

##### 🛠 Possible Improvements:

- unCLIP struggles with binding different attributes to different objects in the prompt. Supposedly, using an attribute-focused CLIP might help.  
- unCLIP struggles to produce coherent text, which is worsened by the BPE encoding that obscures spelling  
- unCLIp is not very good at producing details in complex scenes due to several upsamples  


##### ✏️My Notes:

- *(Naming: 5/5)* DALL-E 2 or unCLIP, both are great and very memeable!  
- *(Reader Experience - RX: 3/5)* The figures are great, the captions tell a story, the architecture overview is clear, the samples are diverse, the contributions are obvious, the text is pithy and well written, the math is … tbh I don’t know, I am reading that crap again. Intuitively understanding diffusion is enough for my satisfaction.  

- Can’t wait to compare this to Imagen, obviously  
- I did not know that GPT could be used for diffusion, had to do a double take there  
- I think the most interesting takeaway here is that CLIP is far from perfect when it comes to aligning the image and text latent spaces. I wonder if a better/reworked CLIP model is the next step to improving this pipeline.  
- Not really sure what to take away from the fact that GLIDE achieves more or less the same results as unCLIP. Is it just better marketing this time around?  
- I think that DALL-E 2 is best suited for illustrations and crazy prompts, not for photorealism. I can’t wait for something like AI-dungeon to incorporate DALL-E 2 into it.  

##### 🔗 Links:

[DALL-E 2 arxiv](https://arxiv.org/pdf/2204.06125.pdf) / [DALL-E 2 GitHub (unofficial)](https://github.com/lucidrains/DALLE2-pytorch) / [DALL-E 2 Demo](https://labs.openai.com/waitlist)

##### 💸 If you enjoy Casual GAN Papers, consider tipping on KoFi:  

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Tip Casual GAN Papers', '#e02863', 'V7V7BXBHV');kofiwidget2.draw();</script> 

***

##### 🔥 Read More Popular AI Paper Summaries:
- [Mask-conditioned text-to-image - Make-A-Scene Explained]({% post_url 2022-4-12-Make-A-Scene-explained %})
- [Democratizing Diffusion Models - Latent Diffusion Models Explained]({% post_url 2022-5-3-Latent-Diffusion-Models-explained %})
- [Improved Text-to-Image Diffusion Models - GLIDE Explained ]({% post_url 2022-4-25-GLIDE-explained %})

##### 👋 Thanks for reading!
*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
