---
layout: post
title: "72: HyperStyle"
meta_title: "HyperStyle"
description: "HyperStyle: StyleGAN Inversion with HyperNetworks for Real Image Editing by Yuval Alaluf, Omer Tov, et al. explained in 5 minutes"
date: 2021-12-3 16:00:00 -0000
categories: image-editing-stylegan2-encoder-generator-tuning-inversion
---

#### HyperStyle: StyleGAN Inversion with HyperNetworks for Real Image Editing by Yuval Alaluf, Omer Tov, et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![HyperStyle](/assets/images/hyperstyle_teaser.jpeg "HyperStyle Teaser")

##### 🎯 At a glance:

It proved to be a surprisingly difficult task to balance the reconstruction quality of images inverted into the latent space of the StyleGAN2 generator and the ability to edit these images afterward. Now Yuval Alaluf, Omer Tov, and the team that originally reported the infamous reconstruction-editability tradeoff in their “Designing Encoders for Editing” paper are back at it again with a new encoder design inspired by the recent PTI paper that sidesteps the tradeoff by finetuning the generator’s weights in a way that places the inverted image into a well-behaved region of the latent space and leaves the editing capability unchanged. HyperStyle is a hyper network that speeds things up by training a single encoder to predict the weight offsets for any input image, replacing the compute-intensive per-image optimization with a single forward pass of the model that takes a second instead of a minute.  

How are the authors able to predict the weight offsets for the entire StyleGAN2 generator in such an efficient manner? Let’s find out!

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [PTI - Pivotal Tuning Inversion]({% post_url 2021-6-29-PTI %})  
2) [e4e - Encoders for Editing]({% post_url 2021-4-13-e4e-explained %})  
3) [ReStyle]({% post_url 2021-4-9-ReStyle-explained %})  

***

##### 🚀 Motivation:

Let’s take a quick look back at how the image inversion and editing approaches evolved this year to understand how the authors of HyperStyle are able to craft their elaborate method that both incorporates the ideas from several previous approaches and elevates them to the next level. It was established early on that there exists a tradeoff between the reconstruction quality of an image and its editability.  

The intuition for this discovery was that the style vectors most suitable for editing all lie in the most well-defined areas of the generator’s latent space and reconstructing images with a higher level of details inevitably pushes the style vectors out of this nice latent vector distribution that the generator learns while training. Therefore, several encoders were developed that sacrificed some fine details on the reconstructed images to retain the ability to edit them, alas they were far from perfect.  

Recently, a new idea emerged: what if instead of trying to push the style vectors for perfectly detailed reconstructed images into the correct areas of the generator’s latent space we slightly shifted the latent space in a way that placed the desired style code right inside it? This method exists and it is called Pivotal Tuning Inversion, and while it works pretty well, it requires up to a minute of optimization to finetune the generator’s weights for every input image.  

HyperStyle brings all of the previous research to a nice conclusion by introducing an encoder that directly predicts the weight offsets for the finetuned generator in a single forward pass for any image and in doing so mitigates the need to balance reconstruction quality and editability of the inverted images.  

***

##### 🔍 Main Ideas:

**1) Overview:**  
The method starts with a pretrained StyleGAN2 generator, input images, and the initial projected style codes for those images obtained with an off-the-shelf e4e StyleGAN2 encoder. The goal then is to utilize a hypernetwork that uses the initial reconstruction and the target image to predict offsets for the weights of the generator that minimize the reconstruction loss computed using the initial estimates of the inverted vectors.

**2) Designing the Hypernetwork:**  
Straight up predicting 30m offsets for every single parameter is almost as unrealistic as training a GPT-3 at home. Hence, several clever ideas are employed to make the task more feasible. First, only a single offset per channel is predicted, which right away reduces the number of hypernetworks by 88% without a noticeable drop in performance.

The hypernetwork is a ResNet34 that takes a channel-wise concatenation of the target and initial reconstruction images and outputs a 16x16x512 feature map. This feature map is further processed by refinement blocks that each produces the offsets for one of the generator layers. Each refinement block consists of a unique convolutional part and either a unique 2-layer MLP or one that is shared for all of the biggest non-toRGB layers (the ones with dimensions 3x3x512x512, i.e. the first layers of the generator). The final hypernetwork has 2.7b parameters, which is 89% fewer than the naive version.

Experimentally, the authors found that leaving the toRGB layer unchanged improved image quality since they mostly affect pixel-wise texture and color and alterations to the weights in these layers negatively impacts the image quality.

Finally, the hypernetwork is restricted to predict offsets only for the medium and fine layers of the generator since the information in the coarse layers is captured well with the initial inversion

**3) Training:**  
The refinement is performed in several iterations with each using the previous output as the initial reconstruction, and losses computed after each iteration. In effect, the encoder learns to optimize the weight offsets in an efficient almost feedforward manner, leading to increased expressiveness and better reconstruction quality. The basic perceptual and L2 losses along with an identity-based loss are used for training the HyperStyle model.

##### 📈 Experiment insights / Key takeaways:

- Baselines: pSp, e4e, ReStyle, IDInvert, PTI, direct optimization  
- Datasets: FFHQ, Celeb-A, Stanford cars, AFHQ Wild  

- HyperStyle is better at reconstruction and identity preservation than encoder-based approaches, and much faster than PTI with a similar level of quality  
- HyperStyle does away with the editability-reconstruction tradeoff  
- Ablation: per-channel offsets are more efficient than depthwise-separable convolutions for weight offset prediction  
- HyperStyle handles out-of-domain images just fine without any modifications  

***

##### 🖼️ Paper Poster:

![HyperStyle poster](/assets/images/hyperstyle.jpg "HyperStyle Poster")

***

##### 🛠 Possible Improvements:

- Figure out how to make Hyperstyle work on unaligned and unstructured domains

##### ✏️My Notes:

- (4/5) Not funny, but memorable for sure!  

- It goes without saying but dang, the results are mind-blowingly impressive  
- Interesting choice to go with StyleGAN2 since the aliasing is very noticeable on the animations. Is StyleGAN3 just not that invertible?  
- Hypernetworks are notoriously hard to get right, hence the presented results are that much more impressive.  
- Yes, the authors already used CLIP, and with great success, +10 Casual GAN Coins for the team!  

- I like the entire lineup of papers from this team, but the line: “The inversions of optimization, pSp, and ReStylep reside in poorly-behaved latent regions of W+.” just rubs me the wrong way since the authors sorta take a big dump on their previous papers, when at the time of writing they were hyping up those same papers as the peak achievements in GAN inversion :( I mean why weren’t these failure cases shown in earlier papers?  

- What do you think about HyperStyle? Write your comments in the chat!  

##### 🔗 Links:
[HyperStyle arxiv](https://github.com/yuval-alaluf/hyperstyle) / [HyperStyle github](https://github.com/yuval-alaluf/hyperstyle) / [HyperStyle Demo](https://colab.research.google.com/github/yuval-alaluf/hyperstyle/blob/master/notebooks/inference_playground.ipynb)

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [Are image transformers overhyped? - MetaFormer explained]({% post_url 2021-11-30-MetaFormer-explained %})  
- [GANs + Transformers = ? - GANsformer-2 explained]({% post_url 2021-11-24-GANsformer2-explained %})  
- [How to invert images with StyleGAN2 - The intuition explained]({% post_url 2021-11-19-AI-assisted-Image-Editing-Part1 %})  

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
