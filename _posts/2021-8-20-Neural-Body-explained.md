---
layout: post
title:  "47: Neural Body"
meta_title: "Neural Body"
description:  "Neural Body: Implicit Neural Representations with Structured Latent Codes for Novel View Synthesis of Dynamic Humans by Sida Peng et al explained in 5 minutes."
date: 2021-8-20 19:00:00 -0000
categories: implicit-neural-representation-full-body-avatar-novel-view-synthesis
---

#### Neural Body: Implicit Neural Representations with Structured Latent Codes for Novel View Synthesis of Dynamic Humans by Sida Peng et al explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![Neural Body Samples Bird](/assets/images/neuralbody_teaser.gif "Neural Body teaser bird")

##### 🎯 At a glance:

Want to dance like a pro? Just fit a neural body to a sparse set of shots from different camera poses and animate it to your heart's desire! This new human body representation is proposed in a CVPR 2021 best paper candidate work by Sida Peng and his teammates. At the core of the paper is the insight that the neural representations of different frames share the same set of latent codes anchored to a deformable mesh. Neural Body outperforms prior works by a wide margin.

##### ⌛️ Prerequisites:

**(Check these out if you have extra time):**  
1)[NeRF](https://t.me/casual_gan/22)

***

##### 🚀 Motivation:

Previously, free view video required a specialized array of depth sensors and cameras. This work, however focuses on the problem of novel view synthesis from a sparse multi-view video with just a few cameras. Traditional rendering methods require dense input views, while reconstruction-based ones struggle with occlusion. The recently proposed NeRF works alright, but does not handle well highly sparse input views. The authors pitch that the key to solving the sparse view problem lies in aggregating all observations over different video frames. Instead of learning different implicit fields for each frame, Neural Body anchors latent vectors to keypoints on a defformable human model (SMPL) and learns to predict denstity and color of each point based on the corresponding latent codes, and their spatial location. Everything is learned jointly from all video frames.

***

##### 🔍 Main Ideas:

**1) Overview:**  
Neural body starts from a bunch of latent codes attached to the surface of a deformable SMPL. The code for any location is obtained with diffusion, decoded into density and color by two models, and the entire image is generated with volume rendering. The model as well as the codes are jointly learned by minimizing the difference between the input and rendered images.

**2) Structured latent codes:**  
SMPL outputs a posed 3D mesh with 6890 vertices, and Neural Body has a latent vector for each vertex. The spatial locations of the codes are transformed based on the mesh pose.

**3) Code diffusion:**  
Since implicit fields require sampling codes at continuous 3d locations, and trilinear interpolation leads to 0 vectors in most points due to sparsity, the Neural Body latent codes are diffused to nearby 3d space. The bounding box around the human body mesh is split into small voxels, and the codes inside voxels are averaged for non-empty voxels. Next, a SparseConvNet uses 3D convolutions to output 2x, 4x, 8x, 16x downsampled volumes, and interpolated codes from across all scales are concatenated into the vectors that form the latent code volume. The code for any 3D point is trilinearly interpolated from the latent code volume after transforming its coordinates to SMPL coordinate system.

**4) Density and color regression:**  
Density and Color fields are represented by MLP networks NeRF style.  In addition to using positional encoding a temporal encoding is used for each fram in a video to account for small changes in lighting.

**5) Volume rendering:**  
Standard volume rendering to a 2D image to compute a pixel-wise loss.

##### 📈 Experiment insights / Key takeaways:

- All experiments are conducted on ZJU-Mocap  
- Neural Body (NB) outperforms Neural Texture, Neural Volume, and NHR on novel view synthesis  
- NB beats PIFuHD on 3D reconstruction  

***

##### 🖼️ Paper Poster:

![Neural Body paper poster](/assets/images/neuralbody.png "Neural Body Paper Poster")

***

##### ✏️My Notes:

- (4/5) Neural body and Latent Code Volume give me strong Evangelion vibes for some reason 😂. Definitely a like!  
- Have you tried Neural Body? Let me know in the comments!  

##### 🔗 Links:
[Neural Body arxiv](https://arxiv.org/pdf/2108.03798.pdf) / [Neural Body github](https://github.com/huage001/painttransformer)

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
