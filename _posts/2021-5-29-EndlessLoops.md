---
layout: post
title:  "25: Endless Loops Explained"
meta_title: "Endless Loops Explained"
description:  "Endless Loops: Detecting and Animating Periodic Patterns in Still Images by Tavi Halperin et al. explained in 5 minutes."
date:   2021-5-29 19:00:00 -0000
categories: periodic-image-animation-cinemagraph-generation 
---

#### Endless Loops: Detecting and Animating Periodic Patterns in Still Images by Tavi Halperin et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![Endless Loops: Detecting and Animating Periodic Patterns in Still Images teaser](/assets/images/endless_loops_teaser.gif "Endless Loops teaser")

##### 🎯 At a glance:

Have you ever taken a still photo and later realized how cool it would have been to take a video instead. The authors of the "Endless Loops" paper got you covered. They propose a novel method that creates seamless animated loops from single images. The algorithm is able to detect periodic structures in the input images that it uses to predict a motion field for the region, and finally smoothly warps the image to produce a continuous animation loop.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
No prior knowledge is required to understand this paper

***

##### 🔍 Main Ideas:

**1) Overview:**  
The algorithm takes as input an image to be animated, a binary mask indicating the pixels to be animated, and a general direction of motion. The authors observed that displacement fields suitable for cinemagraph creation can mostly be constructed from a main direction vector together with a small number of offsets. The dense CRF solver is quadratic in the number of displacement vectors , hence keeping that number small is vital to the inference time of the model.

**2) Detecting repetitions:**  
The authors use a two stage approach to generate the displacement field. They first solve a simpler problem in 1D. Specifically, they ample a wide band of pixels around the main direction line going through the center of mass of the masked region. The goal is to match successive occurrences of the repeating pattern (windows, tile, etc). The problem is solved by finding the shortest path spanning the main diagonal of the self-similarity matrix of the band of pixels via dynamic programming. The shortest path has successive, non-decreasing indices of points in both dimensions, and no more than two points can stay on the same matrix row. To avoid a trivial path along the main diagonal (1, 1) -> (2, 2) -> .. (n, n) the elements of the matrix on and around the main diagonal are given infinite weight. The authors found that the best results are obtained when reflecting the original sample width-wise before computing the shortest path and taking the middle part of the computed path.

**3) Displacement assignment with CRF:**  
To compute the displacement field over every pixel in the region the authors choose to fit a CRF on an extended set of motion vectors obtained in the previous step. Since more angular freedom is wanted for assigning motion vectors to pixels, the set of vectors from the previous step is expanded with additional vectors up to 30 degree angular deviation. Each pixel is assigned a starting value extrapolated from the 1D displacement vectors.
The CRF solver uses feature vectors from the first two layers of a VGG-16 model to minimize the perceptual distance between each pixel and its displacement. The solution is regularized to stay close to the original guess. Cosine similarity is used as the label compatibility function.

**4) Inverting the flow:**  
An invertible motion field is required to generate video frames via backward warping according to the flow, hence the predicted motion field is subsampled after a gaussian kernel is applied to it. Subsequently, the sparse flow is interpolated to all pixels using a polyharmonic spline with radial basis function. This operation ensures the dense field is smooth, has not holes or collisions and serves as a good approximation of the inverse flow.

**5) Video generation:**  
The first and last frames of the animation are required to be the input image for a seamless loop. However, in most images the periodic patterns are similar but not exact copies of themselves. Therefore, to make a seamless animation loop the authors use alpha-blending between the animation (forward), and a time-shifted version of itself (backward). The alpha value is given by a function that ensure that the blended frames are corresponding to a unit shift for a periodic pattern, and that each animation is multiplied with a zero blend factor exactly at the time of its discontinuity.

##### 📈 Experiment insights / Key takeaways:

- It is possible to have multiple animated areas on a single image all moving in different directions
- It is possible to use the algorithm on a video by applying it on every frame and estimating planar homography transformation of the masked pixels from the first frame to all other frames to warp the object in future frames.
- Better perceptual quality is achieved by placing anchor points with 0 displacement around the masked region to smooth out the sharp motion boundary.
- The entire pipeline takes 0.5-1 seconds per image on an iPhone XS
- Limitations include ghosting when animating fluids, failure to detect repetitions with a strong perspective distortion
***

##### 🖼️ Paper Poster:

![Endless Loops: Detecting and Animating Periodic Patterns in Still Images paper poster](/assets/images/endless_loops.png "Endless Loops Paper Poster")

***

##### ✏️My Notes:

- Unfortunately the model does not have a cool name to rate :(
- (Not Sponsored)The authors actually released an app with this model. It is called Motionleap and is available for download on both iPhone and Android devices.
- What is your favorite use of computer vision methods in photo editing apps, and which models/methods would you like to see in photo editors? Let me know in the comments to this post!

##### 🔗 Links:
[Endless Loops arxiv](https://storage.googleapis.com/ltx-public-images/Endless_Loops__Detecting_and_animating_periodic_patterns_in_still_images.pdf) / [Endless Loops github](https://pub.res.lightricks.com/endless-loops/)

***

##### 👋 Thanks for reading!

<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
