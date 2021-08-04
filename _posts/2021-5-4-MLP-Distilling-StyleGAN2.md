---
layout: post
title: "16: Distilling StyleGAN2 Explained"
meta_title: "Distilling StyleGAN2 Explained"
description: "StyleGAN2 Distillation for Feed-forward Image Manipulation by Viazovetskyi et al. explained in 5 minutes."
date: 2021-5-4 19:00:00 -0000
categories: feedforward-latent-image-editing-direction-discovery-stylegan
---

#### StyleGAN2 Distillation for Feed-forward Image Manipulation by Viazovetskyi et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌑🌑🌑🌑

***

![StyleGAN2 Distillation for Feed-forward Image Manipulation Samples](/assets/images/distilling_teaser.png "Distilling StyleGAN2 Samples")

##### 🎯 At a glance:

n this paper from October, 2020 the authors propose a pipeline to discover semantic editing directions in StyleGAN in an unsupervised way, gather a paired synthetic dataset using these directions, and use it to train a light Image2Image model that can perform one specific edit (add a smile, change hair color, etc) on any new image with a single forward pass.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
Nothing here, enjoy the paper!

***

##### 🔍 Main Ideas:

**1) Data collection:**  
The main contribution of this paper is a pipeline that enables an unsupervised creation of synthetic paired datasets that can be used to apply chosen edits to real images. This pipeline can be described in 7 steps:
1) sample a lot latent vectors and corresponding images from a pretrained generator
2) get attribute predictions from a pretrained classifier network
3) select images with high classification scores for desired attributes
4) find the editing direction by subtracting the average latent vector of the images with the highest negative scores for the attribute from images with the most positive scores.
5) sample sets of latent vectors that are made up of a random vector, a vector with the edit direction subtracted from it and one with the edit direction added
6) predict the attribute scores for all sampled images
7) from each set of images select a pair based on the classification scores such that the two images belong to opposite classes with high certainty

**2) Distilation:**  
Once the dataset is gathered any Image2Image model can be trained with it. The authors use the vanilla Pix2PixHD framework without any modifications at 512x512 resolution.

**3) Stylemixing distillation:**  
Besides showing impressive results for distilling attribute edits from a StyleGAN2 generator into a Pix2PixHD model the authors also show that stylemixing operations can be distilled in the same way. To train such a model it is necessary to collect a dataset of triplets: two images, and their mixture, and feed a concatenation of the two input images to the Pix2PixHD model. Another possibility is image blending, where the third image in the triplet corresponds to the average of the two input latent vectors.

##### 📈 Experiment insights / Key takeaways:
*FFHQ (FID)*  
The best is authors' approach @ 14.7 (next best is StarGANv2 @ 25.6) | real data is @ 3.3

***

##### 🖼️ Paper Poster:

![StyleGAN2 Distillation for Feed-forward Image Manipulation Paper Poster](/assets/images/distilling.png "Distilling StyleGAN2 Paper Poster")

***

##### ✏️My Notes:

- No rating for the name, since the paper does  not have a model/pipeline name 🤷‍♂️
- There is an identity gap between the original and edited images in the synthetic dataset, hence on real images the change in identity becomes even more apparent
- It is non trivial to get disentangled directions in a StyleGAN generator, especially with a simple linear vector addition as proposed here. Some papers try to solve this by modeling the edit as a nonlinear transformation between two vectors in the latent space.

##### 🔗 Links:
[Distilling StyleGAN2 arxiv](https://arxiv.org/abs/2003.03581) / [Distilling StyleGAN2 Github](https://github.com/EvgenyKashin/stylegan2-distillation)

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
