---
layout: post
title: "73: Dream Fields"
meta_title: "Dream Fields"
description: "Zero-Shot Text-Guided Object Generation with Dream Fields by Ajay Jain, et al. explained in 5 minutes"
date: 2021-12-3 16:00:00 -0000
categories: image-editing-stylegan2-encoder-generator-tuning-inversion
---

#### Zero-Shot Text-Guided Object Generation with Dream Fields by Ajay Jain, et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![Dream Fields](/assets/images/dreamfields_teaser.gif "Dream Fields Teaser")

##### 🎯 At a glance:

Do you like generative art? I love it, and it is about to get a whole lot crazier because Ajay Jain and the minds at Google behind the original NeRF have dropped a hot new paper. That is right, we all thought about it and they did it - CLIP + NeRF. With Dream Fields it is possible to train a view-consistent NeRF for an object without any images, using just a text prompt. Dream Fields leverages the fact that an object (e.g. an apple) should resemble an apple regardless of the direction that you look at it from, which is one of the core features of CLIP. The basic setup is simple - render a randomly-initiated NeRF from a random viewpoint, and score this image against a text prompt, update the NeRF, and repeat until convergence. As for the juicy details, well continue reading to find out!

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [CLIP]({% post_url 2021-9-14-CLIP-explained %})  
2) [NeRF]({% post_url 2021-4-6-NeRF-explained %})   

***

##### 🚀 Motivation:

While there isn’t enough captioned 3D data in the world to train a text-conditioned NeRF generator, luckily CLIP exists and it is perfect for tasks without paired “text -> ?” data. Let’s remember that CLIP was trained to match images with text captions, hence the model inherently learns that no matter how you look at it, a door always looks like a door, and a banana looks like a banana. This is an important property since a 3D model should be view-consistent, meaning that the rendered image should match the input text query from any camera position as shown in Diet-NeRF (except in that paper just the image encoder is used to enforce multi-view consistency without any text).  

Combining this view-consistent property with the CLIP + VQGAN iterative optimization-based approach to finding the VQGAN latent codes that create the image closest to the input prompt in the CLIP text-image space. Generation yields the pipeline presented in Dream Fields: start from a randomly-initiated NeRF, render it from a random viewpoint, compute the CLIP loss between the rendered image and the text prompt, update the weights of the MLP inside NeRF, rinse and repeat until the resulting 3D object looks good enough. It is actually a tad bit more complex, and without further ado, let’s learn the clever ideas that actually make this seemingly simple approach work as well as it does.  

***

##### 🔍 Main Ideas:

**1) Object representation:**  
The underlying representation is almost the same as NeRF - an MLP that takes in 3D coordinates (no viewing direction) and outputs the volume density and color at that point. For any camera pose, rays are cast into the scene, and the final image is rendered by integrating the density and color of points sampled on these rays.

**2) Objective:**  
Dream Fields are trained with a simple CLIP loss that measures the distance between the rendered image and the text prompt in the CLIP space. Interestingly, authors also report experiments with LiT - an alternate version of CLIP, trained on a larger dataset of higher-resolution images with noisy captions.

**3) Challenges with CLIP guidance:**  
Bruteforce training Dream Fields does not, unfortunately, work, since the underlying NeRF tends to learn half-transparent floating regions of density and tries to fill the entire rendered image with artifacts instead of concentrating the object in the center of the scene.

**4) Pose sampling:**  
Inspired by 2D image augmentations, the authors randomly sample camera azimuth (position on the circle around the center of the scene, pointing inwards) so that the object is always seen from a different perspective, which encourages consistent geometry. Augmenting camera elevation, focal length, and distance from the object did not improve the 3D object’s quality.

**5) Encouraging coherent objects through sparsity:**  
To encourage sparsity, the authors introduce a transmittance loss that measures how far along a ray the camera can see and compares that to a target value. Setting a high target value like 88% and placing a random smooth Gaussian noise, checkerboard, or Fourier texture background at the back of the scene forces Dream Fields to learn a sparse scene with many empty rays and a dense object in the center.

**6) Localizing objects and bounding scene:**  
Since CLIP was trained on natural images that do not necessarily have a center, the authors encourage Dream Fields to generate dense objects by tracking the center of mass of rendered density and pointing the rays from the camera towards it. Moreover, the scene is bounded with a cube, and density is zeroed out with a softplus outside of it.

**7) Neural scene representation architecture:**  
The architecture is a residual 8-layer MLP with skip connections every 2 layers and Layer Normalization for predicting density and 2 additional layers on top of it for predicting the radiance. ReLU is replaced with Swish to combat vanishing gradients.

##### 📈 Experiment insights / Key takeaways:

- Since no ground truth data is available Dream Fields are evaluated with a CLIP precision-recall metric from novel views  
- Object-centric captions/images for evaluation are curated from COCO  

- The most significant improvements come from sparsity, scene bounds and architecture  
- Larger LiT model improves the sharpness and visual quality at higher resolutions  

- Pretty much all the same rules apply here as they do with DALL-E: you can mix and match parts of a prompt that define geometry and style  

***

##### 🖼️ Paper Poster:

![Dream Fields poster](/assets/images/dreamfields.jpg "Dream Fields Poster")

***

##### 🛠 Possible Improvements:

- The next obvious (and probably hardest) step is to skip optimization and do a feed-forward generative model akin to DALL-E for NeRF (CLIP + GIRAFFE)  
- Use different prompt variations during training  
- Use meta-learning and amortization to speed things up  
- Include scene layouts in post-processing  
- Negate biases in the scoring model (CLIP)  


##### ✏️My Notes:

- (3.5/5) the name is alright, though I am not sure what the obsession is with the word “dream” for generative models. Just going to leave my suggestion here for the taking: HalluciNeRF.  

- We are just a couple of steps away from a full VR AI-dungeon game  
- I can not wait to try out Dream Fields for myself, gotta know how it handles the “Greg Rutkowski” magic modifier.  
- All of the generated shapes appear to be very smooth and round, I wonder if it is possible to condition the style of geometry with specific prompts  
- Has anyone tried generating optical illusions with CLIP?  

- I think use can use a pretrained NeRF as the starting point for optimization, hence there will be some very trippy videos when this model is released  
- I don’t remember for sure, but I guess it is possible to rotate and zoom NeRFs, which means Dream Fields could probably generate something similar to those infinitely zooming and rotating VQGAN+CLIP videos but with full camera movement. So… 4D content?  

- I would have assumed the ray sampling to be biased towards the center of the scene since Dream Fields pretty much exclusively renders objects at the center of the scene anyways.  

- I suggest you get familiar with Dream Fields, because I Expect to see many follow-ups and colabs featuring this model.  
- What do you think about Dream Fields? Write your comments in the chat  

##### 🔗 Links:
[Dream Fields arxiv](hhttps://arxiv.org/abs/2112.01455) / Dream Fields github (Not Available at the Time of Writing)) / Dream Fields Demo

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [Are image transformers overhyped? - MetaFormer explained]({% post_url 2021-11-30-MetaFormer-explained %})  
- [GANs + Transformers = ? - GANsformer-2 explained]({% post_url 2021-11-24-GANsformer2-explained %})  
- [SOTA StyleGAN inversion - HyperStyle explained]({% post_url 2021-12-3-HyperStyle-explained %})  

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
