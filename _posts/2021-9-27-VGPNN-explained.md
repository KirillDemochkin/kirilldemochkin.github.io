---
layout: post
title: "58: VGPNN"
meta_title: "VGPNN"
description: "Diverse Generation from a Single Video Made Possible by Niv Haim, Ben Feinstein et al."
date: 2021-9-27 16:00:00 -0000
categories: patch-matching-single-video-input-diverse-video-generation
---

#### Diverse Generation from a Single Video Made Possible by Niv Haim, Ben Feinstein et al.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![VGPNN teaser](/assets/images/vgpnn_teaser.gif "VGPNN Teaser")

##### 🎯 At a glance:

Imagine a model that can take a single video, and generate diverse high-quality variations of the input video, perform spatial and temporal retargeting, and even create video analogies, and do conditional video inpainting. All in a matter of seconds. From a single video. Let that sink in. Now get ready, because this model actually exists! VGPNN is introduced in a 2021 paper by Niv Haim, Ben Feinstein, and the team at the Weizmann Institute of Science. VGPNN uses a generative image patch nearest neighbor approach to put existing single video GANs to shame by reducing the runtime from days for low-res videos to minutes for Full-HD clips.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [PatchMatch](https://gfx.cs.princeton.edu/pubs/Barnes_2009_PAR/patchmatch.pdf)
2) [GPNN](https://arxiv.org/abs/2103.15545)

***

##### 🚀 Motivation:

Video editing and generation remain difficult tasks due to their high variability and large dimensionality. The most common modern approach is to train a generative model on a large collection of videos, however, these approaches are mostly limited to a few domains such as facial expressions or robot hand movements, and cannot handle arbitrary videos. Single video GANs, on the other hand, can learn a generative video model for any single video, although single video GANs take an insane time (days) to train for a single short low-res video. VGPNN (Video-Based Generative Patch Nearest Neighbors) generates many random novel video outputs with matching distribution of space-time patches of the original video by adapting GPNN to videos. GPNN is optimized with Weighted PatchMatch, an approximate nearest neighbor method that uses a globally weighted patch similarity space to search for matches.

***

##### 🔍 Main Ideas:

**1) Spatio-Temporal Pyramid:**  
The input video is transformed into a pyramid of multiple scales (downscaling via cubic interpolation is done in both spatial and temporal dimensions). The video is generated in a coarse-to-fine manner with the initial input being the coarsest scale input video with added Gaussian noise (random for spatial locations, but replicated along the temporal dimension). The output at the coarsest scale is produced by replacing each space-time patch in the initial guess with the patches from the corresponding scale of the videos in the pyramid and setting each pixel to the median value of the pixels in all overlapping patches. The output video is passed several times through the same layer before getting bicubically upscaled to become the input for the next scale.

**2) QKV Scheme:**  
Patch comparison in VGPNN is done via a query-key-value scheme. Since the Query, the upscaled input video front the previous layer is slightly blurry, comparing it with the sharp video in the pyramid would likely result in poor matches, hence the authors of VGPNN upsample the video from the previous scale of the pyramid and use it as the Key, and the sharp video becomes the Value. Two very useful side effects of this approach are the ability to use a different video as the key to perform structural analogies and the ability to generate videos of any shape since the QKV scheme is agnostic to the input sizes.

**3) WeightedPatchMatch:**  
This is the same thing as PatchMatch but with weights for the distance metric, which provides globally-dependent information. In other words, it solves for the patch that has the smallest distance query patch multiplied by that patch’s weight.

**4) Completeness Score:**  
The weights are approximated by running a regular PatchMatch where the Query and Key patches swap roles. These weights are in effect an approximate normalized similarity score between patches that encourages visual completeness while maintaining visual coherence.

##### 📈 Experiment insights / Key takeaways:

- Compared to SinGAN-GIF and HP-VAE-GAN VGPNN achieves a speedup factor of around 35000 as well as better qualitative & quantitative results.
- VGPNN can be used for Spatial Retargetting, Temporal Retargetting, Video Conditional Inpainting, and my favorite - Video Analogies.

***

##### 🖼️ Paper Poster:

![VGPNN paper poster](/assets/images/vgpnn.jpg "VGPNN Paper Poster")

***

##### 🛠 Possible Improvements:

- VGPNN lacks any high-level semantic information in terms of global scene structure
- VGPNN sometimes fails when a video contains global camera motion of a rigid scenes since VGPNN often generates local deformations to the rigid scene that look unrealistic in dynamics.

##### ✏️My Notes:

- (4/5) for the name. VGPNN is neither clever nor funny, yet it sorta sounds like a neural network-based video format, and that somehow makes sense here. Very strange indeed.
- This method intuitively does the same thing as generating realistic faces from cartoon characters except that it only uses information from the input, hence I believe a deep video prior is sorely lacking here for enriching the output video with details not found in the original video
- There is a lot of fluff in the introduction section of the paper, and I found myself going like: “yeah yeah, I get it, your ideas are dope, just say what they are already!”
- Do I even need to say it at this point? This method just screams to get CLIPed. Input video + text description of the output video  = the perfect setup for CLIP guidance.
- This method really needs a deep video prior for a global understanding of scene dynamics
- Do you have any questions left about VGPNN? Let’s discuss in the comments!

##### 🔗 Links:
[VGPNN arxiv](https://nivha.github.io/vgpnn/VGPNN_paper.pdf) / [VGPNN github](https://nivha.github.io/vgpnn/)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Editable 3D Scenes - Object-NeRF]({% post_url 2021-9-18-Object-NeRF-explained %})
- [Instance Conditioned GAN - Arbitrary similar image generation]({% post_url 2021-9-24-Instance-Conditioned-GAN-explained %})
- [GSN - Generative Scene Networks (VR NeRF)]({% post_url 2021-9-21-GSN-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
