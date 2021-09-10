---
layout: post
title: "8: NeRF Explained"
meta_title: "NeRF Explained"
description: "NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis by Mildenhall et al. explained in 5 minutes."
date: 2021-4-6 19:00:00 -0000
categories: eccv-3d-novel-view-synthesis-differentiable-rendering-implicit-representation
---

#### NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis by Mildenhall et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![NeRF Samples](/assets/images/nerf_teaser.gif "NeRF Samples")

##### 🎯 At a glance:

The authors use a sparse set of views of a scene from different angles and positions in combination with a differentiable rendering engine to optimize a multi-layer perceptron (one per scene) that predicts the color and density of points in the scene from their coordinate and a viewing direction. Once trained, the model can render the  learned scene from an arbitrary viewpoint in space with incredible level of detail and occlusion effects.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
This is the paper that started the NeRF trend

***

##### 🔍 Main Ideas:

**1) Scene representation**  
The scene is represented as a deep fully-connected (FC) network that maps a 5D (3D location coordinate + 2D viewing direction) vector to a RGB value with a density (i.e. opacity). To encourage consistency across views of a scene only the 3D location is passed to the first 8 FC layers to predict the density value and a feature vector, and the virtual camera's viewing direction is concatenated to the resulting feature vector that is transformed by one more FC layer.

**2) Volume rendering**  
Ideas from classical volume rendering are used to produce the resulting 2D image representing a certain view of the scene. Essentially, to render a view it is required to predict the expected color of the rays cast from each pixel of the virtual camera by numerically estimating the integral of the color at each point along the ray, multiplied by the probability that a ray will not hit anything in the scene up to that point. The integral estimation is computed by splitting the ray into equal segments, sampling a single point in each segment, and summing them according to the quadrature rule, which is essentially just alpha composition of predicted colors along the ray with alphas equal to the predicted densities. Such an approach makes the scene continuous (i.e. no fixed resolution grid) since the points are resampled along the ray at each iteration.

**3) Positional Encoding**  
Simple (x, y, z) coordinates lack the expressiveness needed to represent fine details in a complex scene, hence the authors normalize the coordinates to -1, 1 and encode them as a higher-dimensional vector (10 for location, and 4 for direction) of sine and cosine functions with various frequencies evaluated at the coordinates.

**4) Hierarchical volume sampling**  
Simply splitting all rays into the same segments for point sampling inefficient since points would often be sampled from empty space, which is not informative for learning the geometry of the scene. The authors solve this inefficiency by simultaneously optimizing two networks. The first is used for coarse estimation using the approach described above for sampling a small number of points along the rays, and then using the information from the coarse network to sample additional points such that more samples are allocated to regions, where more content is expected to be in the scene. The final render is obtained by evaluating the "fine" network at the union of the coarse and fine points.

**5) Training:**  
The only loss used is MSE between the rendered and real color value of pixels for both the "coarse" and "fine" networks. An optimization of a single scene takes an astonishing 1-2 days on a V100 GPU.
##### 📈 Experiment insights / Key takeaways:

_Realistic Synthetic 360 (SSIM)_
The best is NeRF @ 0.947 (next best is LLFF @ 0.911)

_Realistic Forward-Facing (SSIM)_
The best is NeRF @ 0.811 (next best is LLFF @ 0.798) 

***

##### 🖼️ Paper Poster:

![NeRF Paper Poster](/assets/images/nerf.png "NeRF Paper Poster")

***

##### ✏️My Notes:

- 9/10 for the name, it spawned many puns and funny variations in follow-up papers
- You have to check out the video samples of scenes for yourself, static images do not do them justice
- This is the OG paper that started the NeRF hype train
- The idea to use a coordinate based MLP for implicit representations can be generalized to other domains such as video, sound, 2d images, etc. such as in SIREN
- The main downsides are:  
    1) performance, it takes forever to fit a single scene;  
    2) no support for dynamic scenes and no generalization across scenes   
    3) no way to change the lighting in the optimized scene  

##### 🔗 Links:
[NeRF arxiv](https://arxiv.org/abs/2003.08934) / [NeRF Github](https://github.com/bmild/nerf)

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
