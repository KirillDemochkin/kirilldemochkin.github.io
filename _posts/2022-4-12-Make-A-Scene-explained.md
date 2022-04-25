---
layout: post
title: "84: Make-A-Scene"
meta_title: "Make-A-Scene"
description: "Make-A-Scene: Scene-Based Text-to-Image Generation with Human Priors by Oran Gafni et al. explained in 5 minutes"
date: 2022-4-12 16:00:00 -0000
categories: text-to-image-VQVAE-scene-generation
---

#### Make-A-Scene: Scene-Based Text-to-Image Generation with Human Priors by Oran Gafni et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![Make-A-Scene Model](/assets/images/Make-A-Scene-Preview.png "Make-A-Scene Teaser")

##### 🎯 At a glance:

The authors of Make-A-Scene propose a novel text-to-image method that leverages the information from an additional input condition called a “scene” in the form of segmentation tokens to improve the quality of generated images and enable scene editing, out-of-distribution prompts, and text-editing of anchor scenes.

As for the details, let’s dive in, shall we?

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Tip Casual GAN Papers', '#2b2f3c', 'V7V7BXBHV');kofiwidget2.draw();</script> 

***

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1. [VQGAN]({% post_url 2021-6-1-VQGAN %})  
2. [GLIDE]({% post_url 2022-4-25-GLIDE-explained%})

##### 🚀 Motivation:

Existing methods for text-to-image synthesis are good, yet they lack in several key ares. First, controllability. Only so much information about the desired image can be passed to the model through a text prompt alone. For example, structural composition is notoriously difficult to control with text. Second, existing models are not very good at generating faces and coherent humans in general. Finally, the proposed approach works in 512 resolution as opposed to previous SOTA’s 256 by 256 pixels.

***

##### 🔍 Main Ideas:

**1. Overview:**  
The model is essentially 3 encoders with discrete tokens, a transformer that learns to sample sequences of tokens conditioned on the provided context and a decoder that generates images from a sequence of tokens sampled from the transformer. The entire pipeline works in a feed-forward manner with classifier-free guidance to avoid expensive reranking.

**2. Scene Representation and Tokenization:**  
The input semantic scene map is encoded by a VQ-VAE-like model called VQ-SEG that works on segmentation maps. The scene map is made up of 3 groups of channels: panoptic (objects), humans, and faces, as well as 1 extra channel for the edges between classes and instances.

**3. Face-aware VQ:**  
The authors duly note that text-to-image models struggle to synthesize coherent human faces, hence additional attention is required to smooth things out a bit. On the one hand, authors use a feature matching loss in the regions of the image containing faces comparing the activations of a pertained face-embedding network. On the other hand, a weighted binary cross-entropy loss is added to the segmentation predictions of the face regions along with the edges to promote the importance of small facial details.

**4. Scene Transformer:**  
The regular objects in the panoptic regions also get the feature-matching treatment similar to the face regions. The tokens for the text, the panoptic and human region embeddings along with the face embeddings are passed to an autoregressive transformer (GPT) that outputs a sequence of tokens for the decoder.

**5. Classifier-free Guidance:**  
This neat little trick is used to improve fidelity and diversity of generated samples. Essentially, instead of using a classifier to “guide” the sample towards a cluster of samples from a class, which tends to reduce diversity of generated samples, a technique similar to truncation is used. During training some of the text inputs are replaced with blanks, representing the unconditional samples . During inference, we generate two token sequences in parallel. One with the blank text, and the other with the text prompt as a condition. The unconditioned token is then pushed towards the conditioned one with a predefined weight, then both streams receive the new token and the process is repeated until all tokens have been generated. More on this in the GLIDE post.s

##### 📈 Experiment insights / Key takeaways:

- Baselines: DALL-E, GLIDE, CogView, LAFITE, XMC0GAN, DF-GAN, DM-GAN, AttnGAN
- Datasets: CC12M, CC, YFCC100m, Redcaps, MS-COCO
- Metrics: FID, human evaluation
- Quantitative: Lowest FID, second best is GLIDE with 0.4 difference
- Qualitative: Generating images out of distribution, Scene editing and resampling with different prompts
- Ablations: biggest contribution to the score is from classifier-free guidance and object-aware vector quantization

***

##### 🖼️ Paper Poster:

![Make-A-Scene poster](/assets/images/Make-A-Scene.jpg "Make-A-Scene Poster")

***

##### 🛠 Possible Improvements:

- It would be nice to provide an image as a style reference in addition to the text and a scene segmentation

##### ✏️My Notes:

- (Naming: 4.5/5) - Funny name, easy to pronounce  
- (Reader Expirience - RX: 3.5/5) My main issue with this paper as a reader is the absence of a detailed model architecture diagram and the choice of colors throughout the paper. The choses color scheme is all over the place and just doesn’t look clean and professional. The figure captions are good. The math is fairly easy to follow. Overall the paper has a nice flow.  
- This approach would probably mix well with Duplex Attention from GANsfromer  
- I still find it befuddling that for image-based tasks authors insist on processing tokens in scan order without any regard to their spatial positions. In other words, check out MaskGIT.  

##### 🔗 Links:

[Make-A-Scene arxiv](https://arxiv.org/abs/2203.13131v1) / [Make-A-Scene GitHub](https://github.com/CasualGANPapers/Make-A-Scene) - by Casual GAN Papers Community

##### 💸 If you enjoy Casual GAN Papers, consider tipping on KoFi:  

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Tip Casual GAN Papers', '#e02863', 'V7V7BXBHV');kofiwidget2.draw();</script> 

***

##### 🔥 Read More Popular AI Paper Summaries:
- [Improved VQGAN - MaskGIT explained]({% post_url 2022-2-18-MaskGIT-explained %})
- [How to generate videos from static images - FILM explained]({% post_url 2022-2-26-FILM-explained %})
- [How to do text-to-image without training on texts - CLIP-GEN explained ]({% post_url 2022-3-9-CLIP-GEN-explained %})

##### 👋 Thanks for reading!
*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
