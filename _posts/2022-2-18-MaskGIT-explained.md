---
layout: post
title: "81: MaskGIT"
meta_title: "MaskGIT"
description: "MaskGIT: Masked Generative Image Transformer by Huiwen Chang et al. explained in 5 minutes"
date: 2022-2-18 16:00:00 -0000
categories: improved-vqgan-inpainting-outpainting-conditional-editing
---

#### MaskGIT: Masked Generative Image Transformer by Huiwen Chang et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![MaskGIT Model](/assets/images/maskgit_preview.png "MaskGIT Teaser")

##### 🎯 At a glance:

This is one of those papers with an idea that is so simple yet powerful that it really makes you wonder, how nobody has tried it yet! What I am talking about is of course changing the strange and completely unintuitive way that image transformers handle the token sequence to one that logically makes much more sense. First introduced in ViT, the left-to-right, line-by-line token processing and later generation in VQGAN (the second part of the training pipeline, the transformer prior that generates the latent code sequence from the codebook for the decoder to synthesize an image from) just worked and sort of became the norm.

The authors of MaskGIT say that generating two–dimentional images in this way makes little to no sense, and I could not agree more with them. What they propose instead is to start with a sequence of MASK tokens and process the entire sequence with a bidirectional transfer by iteratively predicting, which MASK tokens should be replaced with which latent vector from the pretrained codebook. The proposed approach greatly speeds-up inference and improves performance on various image editing tasks.

As for the details, let’s dive in, shall we?

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [VQGAN]({% post_url 2021-6-1-VQGAN %})  

***

##### 🚀 Motivation:

second part of the VQGAN, i.e. the token sampling prior. If you recall, VQGAN first learns an encoder-decoder model along with a codebook of latent vectors by reconstructing input images, and after the codebook is frozen a transformer is trained to predict sequences of tokens one-by-one left-to-right, line-by-line that are passed to the decoder to form the final image. Crudely, this is equivalent to predicting the next patch of the image from all of the previous patches.

Does this approach make much sense? The authors certainly do not think so (and I concur). Why should the patches be generated sequentially in this manner, when it makes much more sense to start with a rough sketch and fill in the details iteratively (StyleGAN says “Hi!”). Apparently, there are no prominent papers that explore the upgrade to a Bidirectional Transformer despite the apparent advantages such as the ability to regenerate parts of an image, do inpainting and outpainting and perform class-conditioned editing.

***

##### 🔍 Main Ideas:

**1) MVTM in Training:**  
During each training iteration the input images are passed through a pretrained VQVAE to obtain a set of tokens that are treated as the ground truth. Some of the tokens are left intact, while a portion of them is replaced with a MASK token. The probability that a token is replaced changes overtime as discussed in Maksing Design. The entire sequence of tokens is processed by a bidirectional transformer that learns to predicts the probabilities for each masked token with a simple negative log likelihood (multiclass cross-entropy) loss.  

**2) Iterative Decoding:**  
Unlike the regular transformer from VQGAN that uses information from previously generated tokens and selects the next tokens one-by-one, the bidirectional transformer used in MaskGIT is able to generate the entire image in a single forward pass. However, this does not really work, since the model was trained to fill in a portion of the tokens, not generate all of them at once. Hence, a progressive sampling scheme is employed: the model starts with all MASK tokens, generates the probability distribution and samples a value for each of them. Then, only the ones above a certain confidence threshold are kept. The rest are replaced with the MASK token, and the process repeats until all of the tokens are kept. The authors find that just 7-8 iterations is enough to produce high–quality results. After the first iteration only a single token is always kept, and for the rest of the iterations the required confidence level is decreased until all of the tokens are filled.  

**3) Masking Design:**  
Unlike BERT’s constant 15% masking, the authors propose to vary this number during training according to a schedule. Multiple approaches to mask scheduling design are considered: linear function, concave function, convex function, but the important thing is that the authors used the cosine function, which worked best in all their experiments.  

##### 📈 Experiment insights / Key takeaways:

- Datasets: ImageNet 256, 512; Places2  
- Baselines: BigGAN, VQGAN, DCTransformer, VQVAE-2, improved DDPM, ADM  

- MaskGIT outperforms significantly both VQGAN and BigGAN on ImageNet in 256 and 512 resolutions. New SOTA FID for 512x512 - 7.32  
- MaskGIT accelerates VQGAN by 30-64x  
- MaskGIT establishes a new SOTA ClassificationAccuracy Score for ImageNet generation  

- MaskGIT achieves impressive results for class-conditioned editing (resampling parts of an image with a different class), inpainting and outpainting. With all tasks being trivial to set up with the masking token approach.  
- From ablations: Doing too many iterations at inference leads to less diversity in the generated images, because the model is discouraged from keeping less confident tokens  

***

##### 🖼️ Paper Poster:

![MaskGIT poster](/assets/images/maskgit.jpg "MaskGIT Poster")

***

##### 🛠 Possible Improvements:

- When outpainting, the model has trouble coordinating distant parts of the image, which causes strange semantic and color shifts between different edges of the image.  
- There are cases of oversmoothing and various artifacts that can be fixed  
- Further design on mask design  

##### ✏️My Notes:

- (4.5/5) A solid shorthand way to refer to the model and a “mask it” pun  
- (5/5) for the Paper UX, the figures deserve a separate shoutout! (Yay a new rubric! 🎉)  

- Love these simple-yet-elegant papers that don’t require me to connect to the cosmos to parse through a mountain of complex formulas  
- After reading this paper I felt as if I finally scratched an itch that was bothering me for a while  
- No CLIP experiments?! I feel slightly offended by that XD  
- I like how this idea echoes diffusion models with the progressive sampling scheme that gradually removes the noise (MASK tokens) from the image  

- Well, we started with transformers in CV last year, now we got a sort of a vision-BERT, what NLP paper will migrate to CV next? GPTImage, anyone? Not sure how it would work, but it does sound cool  
- What do you think about MaskGIT? Let's discuss in the comments!
##### 🔗 Links:

[MaskGIT arxiv](https://papers.nips.cc/paper/2019/file/31c0b36aef265d9221af80872ceb62f9-Paper.pdf) / [MaskGIT Github](https://aliaksandrsiarohin.github.io/first-order-model-website/)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Train a NeRF in Seconds - Instant Neural Graphics explained]({% post_url 2022-1-18-Instant-Neural-Graphics-Primitives-explained %})
- [Is StyleGAN Good? - Third Time is the Chart explained]({% post_url 2022-2-2-Third-Time-Is-The-Charm-explained %})
- [Best Facial Animation Tool - First Order Motion Model]({% post_url 2022-2-10-First-Order-Motion-Model-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
