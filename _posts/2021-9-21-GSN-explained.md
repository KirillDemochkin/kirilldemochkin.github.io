---
layout: post
title: "56: GSN"
meta_title: "GSN"
description: "Unconstrained Scene Generation with Locally Conditioned Radiance Fields by Terrance DeVries et al. explained in 5 minutes."
date: 2021-9-21 16:00:00 -0000
categories: ICCV-2021-large-free-moving-indoor-scenes-NeRF
---

#### Unconstrained Scene Generation with Locally Conditioned Radiance Fields by Terrance DeVries et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![GSN teaser](/assets/images/gsn_teaser.gif "GSN Teaser")

##### 🎯 At a glance:

NeRFs are great, yet they are primarily used for interpolating views in single object scenes and have severely limited capabilities for extrapolating beyond the input views. Generative Scene Networks (GSN), proposed by Terrance DeVries and his colleagues at Apple University of Guelph and Vector Institute, learn to decompose scenes into a collection of many local radiance fields. This enables the model to be used as a prior to generate novel scenes or complete scenes from sparse 2D observations at higher quality than existing models.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [NeRF]({% post_url 2021-4-6-NeRF-explained %})
2) [GRAF]({% post_url 2021-7-2-GRAF %})

***

##### 🚀 Motivation:

Current SOTA 3D novel view models mostly focus on reconstructing single object scenes (with the camera pointed at the object) with complex lighting and reflection effects by learning multi-view consistent interpolations. However, they mostly ignore larger indoor scenes that require extrapolating to arbitrary novel views far outside of the set of input views. The authors connect this shortcoming to the lack of a prior over learned scenes. Contrary to other recent works they design their model to have perpetual generation with built-in loop closure, which enables them to have a free-moving camera that explores a scene with a scene prior instead of an explicit memory store that helps the model handle unseen viewpoints.

***

##### 🔍 Main Ideas:

**1) Global Generator:**  
The generator first maps a random noise vector to a 2D grid of local latent codes that represent portions of the entire scene (think of this as an implicit floormap). This can easily be extended to a 3D grid for multi-floor scenes.

**2) Locally Conditioned Radiance Field:**  
To render an image from the camera pose and the 2D grid of latents the model samples (bilinearly) a latent code from the grid via projection and uses it to condition the radiance-predicting MLP. Interestingly, GSN only uses local coordinates for each code to prevent the model from overfitting to global coordinates for predicting radiance. The final image is upsampled via a small convolutional network from a low-res feature map output by the MLP.

**3) Sampling Camera Poses:**  
Unlike GRAF and piGAN that use scenes with an object at the origin of the coordinate system, GSN has additional constraints for camera poses such that it does not sample viewpoints from inside the walls (or outside of them) or other solid objects in the scene. GSN handles this by sampling from a set of candidate locations weighted by their predicted occupancy.

**4) Discriminator and Training:**  
The discriminator is taken mostly from StyleGAN2 with the addition of a small decoder to enforce a reconstruction penalty on real images similar to the self-supervised discriminator. It predicts whether a view is real or not from an input image and the corresponding depth map. GSN uses the non-saturating GAN loss with R1 gradient penalty and the discriminator reconstruction objective

##### 📈 Experiment insights / Key takeaways:

- GSN beats GRAF and pi-GAN on VizDoom, Replica and Active Vision Datasets in terms of FID and SwAV-FID
- GSN beats GTM-SM an ISS on Memorization and Hallucinations tasks in terms of L1 and SSIM metrics in most cases
- Generative performance can be improved by decomposing the scene into many independent locally conditioned radiance fields
- A local coordinate system is significantly more robust to rearranging local latent codes than a global one
- Models trained with long trajectories do not struggle when evaluated on short trajectories
- Depth resolution can be reduced to a single pixel without reduction in the generated image quality

***

##### 🖼️ Paper Poster:

![GSN paper poster](/assets/images/gsn.png "GSN Paper Poster")

***

##### 🛠 Possible Improvements:

- Improving the rendering performance
- Training on large-scale datasets
- Exploring down-stream tasks that use the learned scene prior in RL, SLAM, 3D photography, etc
- Bonus from the appendix: Improve scene editing (e.g., rearranging the objects)

##### ✏️My Notes:

- (3/5) GSN is a decent name, but there are way too many 3-letter Gxx acronyms that all blend together:  GFP, GFN, etc. I am going to call it
- Overall a solid paper. The writing is concise, the formulas are clear, the ideas are simple, yet elegant, my favorite was the floorplan analogy. Great job!
- My first though when I read this paper was about applying GSN to video game level generation. Pretty much any 3D game (Especially in VR) with photorealistic indoor environments would love to have a generative tool such as this. I think the two key improvements that are needed are: high-res output and the ability to condition the scene on an approximate layout of the room (and optionally the style of the room)
- Foveated rendering could probably save a ton of memory bandwidth for VR since it can render in a higher resolution in the part of the scene that the player is currently looking at
- There are stairs in some scenes, and I have so many questions. Can the camera go upstairs? Can we make these scenes with multiple floors? Can we connect scenes via open/closed doors?
- Hey, GSN cited CIPS, always happy to see our model being useful for other research teams!
- Do you have any lingering questions about GSN? Let’s discuss it in the comments!

##### 🔗 Links:
[GSN arxiv](https://arxiv.org/pdf/2104.00670.pdf) / [GSN github](https://github.com/apple/ml-gsn)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [SOTA NLP model for everything - FLAN]({% post_url 2021-9-11-FLAN-explained %})
- [The second half of VQGAN + CLIP for text-image similarity - CLIP]({% post_url 2021-9-14-CLIP-explained %})
- [SOTA - Robust Video Matting]({% post_url 2021-9-7-Robust-Video-Matting-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
