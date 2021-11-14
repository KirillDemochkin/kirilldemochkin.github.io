---
layout: post
title: "7: StyleCLIP Explained"
meta_title: "StyleCLIP Explained"
description: "StyleCLIP: Text-Driven Manipulation of StyleGAN Imagery by Patashnik et al. explained in 5 minutes."
date: 2021-4-6 19:00:00 -0000
categories: clip-guided-text-based-semantic-image-editing
---

#### StyleCLIP: Text-Driven Manipulation of StyleGAN Imagery by Patashnik et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![StyleCLIP Samples](/assets/images/styleclip_teaser.gif "StyleCLIP Samples")

##### 🎯 At a glance:

The authors use the recent CLIP model in a loss function to train a mapping network that takes text descriptions of image edits (e.g. "a man with long hair", "Beyonce", "A woman without makeup") and an image encoded in the latent space of a pretrained StyleGAN generator and predicts an offset vector that transforms the input image according to the text description of the edit.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [CLIP]({% post_url 2021-9-14-CLIP-explained %})  
2) [StyleGAN2](https://github.com/NVlabs/stylegan2)

***

##### 🔍 Main Ideas:

**1) Text-guided latent optimization:**  
The most straightforward approach is to directly optimize a StyleGAN latent vector with a cosine distance loss between the CLIP еmbeddings of the input image and the text prompt. Additionally the L2 and arcface losses are used to control the similarity of the reconstructed and original images. All of the models are frozen during the optimization process. The optimization approach is versatile since it can be used on any image-text pair, but it takes several minutes per image edit, and has stability issues.

**2) Latent residual mapper:**  
To solve the issues of the aforementioned optimization approach the authors came up with a mapper network that learns to perform a single text prompt based edit on any arbitrary image that has been projected to the StyleGAN generator's W+ space. The model consists of three fully-connected mapping networks that independently predict the offsets for each of the coarse, medium, and fine input W+ vector groups. The final W+ style is obtained by summing the input vectors with the offsets. The mapping network is trained with an L2 regularization on the offsets, the CLIP loss (same as before) for a specific text prompt, and the identity loss (arcface cosine distance), except when the edit is expected to change the identity. The input W+ vectors for real images are obtained from a pretrained e4e encoder.

**3) Global text prompt mapping to a StyleGAN direction:**  
Apparently, the offsets are very similar across images, hence the authors strive for a global method of mapping a CLIP embedded text prompt to a single StyleGAN style space direction. The style space direction is determined by assessing the relevance of each style channel to the target attribute. Given the original and edited images the difference between their CLIP encodings is obtained, and it is expected to be highly collinear to the CLIP embedding of the text prompt describing the change. The style space editing direction can be determined by assessing the relevance of each channel in style space to the direction in CLIP space.

**4) Prompt engineering**  
The ImageNet zero-shot classification prompt bank is used to compute stable directions in the text manifold of CLIP. A "good" prompt is comprised of a text description of an attribute and a corresponding neutral class e.g. "A sports car", "A woman with makeup", etc. Prompt engineering is applied to produce the average embeddings for the target and neutral class, and the normalized difference between the two is used as the target direction in CLIP space.

**5) Channelwise relevance**  
A subset of channels in the style space that will be used in the edit is selected using 100 image pairs that are obtained by perturbing a single  channel in sampled direction vectors. The relevance of each channel is calculated by mean projection of the perturbed direction vector onto the sampled vector. Selectively zeroing out the channels under a certain threshold improves disentanglement of the edited attributes.

##### 📈 Experiment insights / Key takeaways:

Wow, there isn't a single comparison table that requires being highlighted here. There is A TON of mind-blowing qualitative examples though, make sure to check them out!

***

##### 🖼️ Paper Poster:

![StyleCLIP Paper Poster](/assets/images/styleclip.jpg "StyleCLIP Paper Poster")

***

##### ✏️My Notes:

- 7/10 for the model name, while, unfortunately not a meme, it is nevertheless a good name for SEO  
- There is a demo provided by the authors  
- I think the results are very solid  
- I wonder if it is possible to take this text based editing even further and use text prompts that describe a relationship between two images to make implicit edits (e.g. "The person from the first image with the hair of the person on the second image", "The object on the first picture with the background of the second image", "The first image with the filter of the second image", etc)

##### 🔗 Links:
[StyleCLIP arxiv](https://arxiv.org/pdf/2103.17249) / [StyleCLIP Github](https://github.com/orpatashnik/StyleCLIP)

***

##### 👋 Thanks for reading!

<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
