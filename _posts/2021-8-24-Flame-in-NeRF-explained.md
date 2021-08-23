---
layout: post
title:  "48: FLAME-in-NeRF"
meta_title: "FLAME-in-NeRF"
description:  "🔥FLAME-in-NeRF🔥: Neural control of Radiance Fields for Free View Face Animation by ShahRukh Athar et al. explained in 5 minutes."
date: 2021-8-24 19:00:00 -0000
categories: 3D-aware-novel-view-head-neural-rendering-synthesis
---

#### 🔥FLAME-in-NeRF🔥: Neural control of Radiance Fields for Free View Face Animation by ShahRukh Athar et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![FLAME-in-NeRF Samples](/assets/images/flamingnerf_teaser.webp "FLAME-in-NeRF teaser")

##### 🎯 At a glance:

How to model dynamic controllable faces for portrait video synthesis? It seems that the answer lies in combining two popular approaches - NeRF and 3D Morphable Face Model (3DMM) as presented in a new paper by ShahRukh Athar and his colleagues from Stony Brook University and Adobe Research. The authors propose using the expression space of 3DMM to condition a NeRF function and disentangle scene appearance from facial actions for controllable face videos. The only requirement for the model to work is a short video of the subject captured by a mobile device.

##### ⌛️ Prerequisites:

**(Check these out if you have extra time):**  
1) [NeRF](https://t.me/casual_gan/22)  
2) [FLAME](https://flame.is.tue.mpg.de/)  
3) [Nerfies](https://nerfies.github.io/)  

***

##### 🚀 Motivation:

Simply put: controllable head modelling in natural scenes with arbitraty view synthesis is yet to be solved. On the one hand, a 3DMM by itself lacks fine details such as hair, skin details and accessories. Moreover, 3DMM does not help to view the scene around the head from novel directions at all. On the other, NeRF is able to model complex 3D scenes, but it does not work with dynamic scenes, required for facial animation. Like Nerfies, Flamin-NeRF maps the video to a canonical frame and assumes that the head motion is small. Unlike Deferred Neural Rendering and Point-Based Graphics Flamin-NeRF does not require explicit scene geometry.

***

##### 🔍 Main Ideas:

**1) Deformable Neural Radiance Fields:**  
To account for subtle movements during the video capture process (can't humans just stand perfectly still while filming themselves on their phones? 🙄) the 3D points for a each frame are mapped to a canonical space via a deformation function (an MLP, duh) conditioned on a unique latent deformation code for each frame. Coarse-to-fine deformation regularization is used prevent the deformations from becoming to large at early stages of training (hurts generalization later on) by linearly increasing the weight of larger frequencies  of the positional encoding starting at zero.

**2) Expression Control:**  
To control the facial expression in each frame the deformavble NeRF is conditioned on the output of a FLAME model that predicts a set of 3DMM parameters.

**3) Spatial Prior for Ray Sampling:**  
To insure that only the points inside the predicted head model are conditioned on the expression parameters a binary mask of the rendered FLAME silhouette is used. The points outside of the mask have the expression condition zeroed out. The head is assumed to be static in each frame and the deformation network is penalized for moving the 3DMM with an L1 (not explicitly stated) loss.

##### 📈 Experiment insights / Key takeaways:

- The training data is captured on IPhone XR
- Without the spatial prior the model changes background along with the facial expression
- Flamin-NeRF beats Nerfies on the validation set of scenes
- It is possible to reanimate a subject (strange choice of words) using a driving video of another person

***

##### 🖼️ Paper Poster:

![FLAME-in-NeRF paper poster](/assets/images/flaminnerf.png "FLAME-in-NeRF Paper Poster")

***

##### ✏️My Notes:

- (5/5) - Emojis in a paper title and a hot Cheetos reference?! Are you kidding me, this is awesome!
- Unfortunately no code to play with at the time of writing this
- There is definitely a trend of enriching the neural features in generative networks with 3D geometry or refining low-res 3D meshes with high-res neural renderers, whichever way you want to look at it.

##### 🔗 Links:
[FLAME-in-NeRF arxiv](https://arxiv.org/pdf/2108.04913.pdf) / [FLAME-in-NeRF github](http://shahrukhathar.github.io/2021/08/12/FLAMEinNeRF.html)

***

##### 👋 Thanks for reading!
<
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
