---
layout: post
title:  "29: GANs N’ Roses Explained"
name:  "29: GANs N’ Roses Explained"
description:  "GANs N’ Roses: Stable, Controllable, Diverse Image to Image Translation by Min Jin Chong et al. explained in 5 minutes."
date:   2021-6-19 19:00:00 -0000
categories: anime multimodal image-to-image video-stylization
---

#### GANs N’ Roses: Stable, Controllable, Diverse Image to Image Translation (works for videos too!) by Min Jin Chong et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![GANs N’ Roses by Xintao Wang et al. samples](/assets/images/gnr_teaser.gif "GANs N’ Roses explained")

##### 🎯 At a glance:

Did you ever want to see what you look like as an anime waifu? Thanks to the authors of GANs N' Roses you can animefy (pretty sure this isn't a real word) selfies and even videos with a multitude of unique styles. The authors from the University of Illinois propose a new generator architecture that combines a content code computed from a face image and a randomly chosen style to produce consistent, diverse and controllable anime faces with attributes matching the content image.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*
1. [Encoders for Editing - e4e](https://t.me/casual_gan/25)

***

##### 🔍 Main Ideas:

**1) Framework overview:**
GANs N' Roses is made up of an encoder and a decoder in two directions (face <-> anime). The encoder disentangles an image into a content code and a style code, and the decoder produces an image from the target domain from the two codes. At test time only the content code is used while the style code is obtained elsewhere. The authors define "style" as everything invariant under data augmentations of content (scale, translation, rotation, etc).

**2) Ensuring style diversity:**
It is nontrivial to ensure that the generated anime faces are diverse. There are several ways a generator can cheat, and ignore the style code or hide it in just a few pixels of the output image. (Note: if you like formulas, there is a whole probability theory section in the paper that I will mostly skip, and summarize the idea in the next sentence) To solve this issue, the authors propose a simple fix: during training instead of taking batches of different images along with a random sample of style codes from some distribution, make batches of augmented versions of the same image and use a discriminator to ensure that the batch of generated anime faces is indistinguishable from a batch of actual random anime faces.

**3) Losses:**
Style consistency loss is the variance of the styles predicted by an encoder for a batch of augmented images (it should be 0 as the style is the same regardless of augmentations).
Cycle consistency loss is the average L2 norm between the input images and recycled images produced by the anime-> face encoder and decoder, except the style passed to the decoder here is the style predicted by the face-> anime encoder, which should be the same for all images in the batch (oh, so that is why we needed to predict the style code 🧐)
Adversarial loss is the standard non-saturating loss with R1 regularization.

##### 📈 Experiment insights / Key takeaways:
- Works on 256x256 images
- Just check out the images, they are way more expressive than numbers in this case
- The authors introduce a new metric called diversity FID that aims to measure the style diversity of generated images
- The method works out of the box on videos
- All latent space editing methods apply to GNR

***

##### 🖼️ Paper Poster:

![GANs N’ Roses: Stable, Controllable, Diverse Image to Image Translation paper poster](/assets/images/gnr.png "GANs N Roses Paper Poster")

***

##### ✏️My Notes:
- (5/5) for the name, FINALLY a meme name! No idea, why this model is called GANs N' Roses, but kudos to the authors going for the laughs👌
- It is so strange that there are only female anime faces in the dataset. I thought I was going crazy when I tried their demo before reading the paper and kept getting my selfie transformed into a female anime character 😅😅 (kinda started thinking it wad due to my long-ish hair that it kept thinking I had to be an anime girl)
- the paper is written in a strangely convoluted way that makes it harder to follow the ideas than should be, hence the three moon rating

##### 🔗 Links:
[GANs N Roses arxiv](https://arxiv.org/pdf/2106.06561v1.pdf) / [GANs N Roses github](https://github.com/mchong6/GANsNRoses)

***

##### 👋 Thanks for reading!
*If you found this paper digest useful, subscribe and share the post with your friends and colleagues to support Casual GAN Papers!*

*Join [the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
