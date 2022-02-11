---
layout: post
title: "80: FOMM"
meta_title: "FOMM"
description: "First Order Motion Model for Image Animation by Aliaksandr Siarohin et al. explained in 5 minutes"
date: 2022-2-10 16:00:00 -0000
categories: self-supervised-image-animation-image-driving
---

#### First Order Motion Model for Image Animation by Aliaksandr Siarohin et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌕

***

![First Order Motion Model](/assets/images/fomm_teaser.gif "First Order Motion Model Teaser")

##### 🎯 At a glance:

If you have ever used a face animation app, you have probably interacted with First Order Motion Model. Perhaps the reason that this method became ubiquitous is due to its ability to animate arbitrary objects. Aliaksandr Siarohin and the team from DISI, University of Trento, and Snap leverage a self-supervised approach to learn a specialized keypoint detector for a class of similar objects from a set of videos that warps the source frame according to a motion field from a reference frame.

From the birds-eye view, the pipeline works like this: first, a set of keypoints is predicted for each of the two frames along with local affine transforms around the keypoints (this was the most confusing part for me, luckily we will cover it in detail later in the post). This information from two frames is combined to predict the motion field that tells where each pixel in the source frame should move to line up with the driving frame along with an occlusion mask that shows the image areas that need to be inpainted. As for the details.

Let’s dive in, and learn, shall we?

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
 

***

##### 🚀 Motivation:

Animating still images has many applications in various industries, yet most existing approaches are object-specific, meaning they require a specialized annotated dataset that might be difficult or costly to obtain. On the contrary, we have object-free methods that animate any type of object that the model is trained on. The only data that is required by object-agnostic models is a set of videos of objects from that category without any need for annotations. The main weakness of such existing approaches is their poor quality since they lack the complexity to model the object deformations precisely.

Another key contribution of the proposed approach is the explicit occlusion model that indicates which areas of the image can be warped, and what missing regions require inpainting.

***

##### 🔍 Main Ideas:

**1) Overview:**  
The method, composed of the motion estimation module and the image generation module, learns from pairs of frames extracted from the same video in a self-supervised manner by learning to “encode motion as a combination of motion-specific keypoint displacements and local affine transformations”. An important detail in the proposed approach is the assumption that some sort of a reference frame exists that both the source and the target frames are warped to. Even though the reference frame is never explicitly rendered its existence allows the source and driver frames to be processed independently.

**2) Local Affine Transformations for Approximate Motion Description:**  
Alright, if you have been following Casual GAN Papers for a while (if not, welcome!) I don’t even attempt to explain the math in the papers I cover, hence we will just pretend that this entire setup works automagically.

With that out of the way, the authors pitch this idea that there is some intermediary reference frame that helps connect the source and driving frames. In practice though, they simply apply the self-supervised keypoint detector (a U-Net) that predicts a normalized heatmap for each keypoint along with 4 parameters for an affine transform corresponding to that keypoint. Intuitively the affine transform contains information on how moving the corresponding keypoint moves pixels near it, while the heatmap shows which pixels are more or less affected by moving each keypoint. In other words, FOMM assumes that the animated object is made of a number of rigid parts that move together, e.g. the entire arm of an animated human moves as a single object, as does each of the legs, the torso, and the head. These local motions are combined with a convolutional network into a single motion field mapping where each pixel in the source image should move to line up with the pose in the driving frame.

**3) Occlusion-aware Image Generation:**  
Since the source image gets deformed to resemble the driving frame, some discontinuities are bound to happen, for example, if the face moves to the side slightly, the background behind it needs to be inpainted. FOMM handles these occlusions via an explicit occlusion mask that is predicted along with the dense motion field. The mask is used to block out the regions that the final generator needs to inpaint from scratch.

**4) Training Losses:**  
Standard stuff (LPIPS) and an equivariance constrain, which simply means that the motion deformations should have the transitive property (the deformation  X -> Y should be the same as the composition of transformations X -> R -> Y)

##### 📈 Experiment insights / Key takeaways:

- Datasets: VoxCeleb, UvA-Nemo, BAIR Robot, Tai-Chi-HD  
- The models are evaluated on the task of video reconstruction from the first frame and the sparse motion data as well as a user study for comparing the full animated videos  
- Reported Metrics: L1, Average Keypoint Distance (with a pretrained keypoint detector), Missing keypoint rate, Average euclidian distance between the embeddings of the target and source frames  
- Ablations: the addition of pyramid loss and occlusion mask boost almost all reported metrics, without the equivariance constraint, the Jacobians become unstable, which leads to poor motion estimation  
- FOMM is miles ahead of X2Face and MonkeNet on all considered datasets  

***

##### 🖼️ Paper Poster:

![FOMM poster](/assets/images/fomm.jpg "FOMM Poster")

***

##### 🛠 Possible Improvements:

- None mentioned in the paper, hence we will need to study the new paper by the authors that seem to be focused on animating more complex articulated objects  
- I sorta-kinda feel like there is a self-attention-shaped hole somewhere in this method. Maybe it could be possible to swap the softmax heatmaps for the duplex attention from GANsformer to predict which pixels are affected by which keypoints. Although a point could be made that the locality assumption (that the motion of each keypoint only affects the pixels in the direct vicinity of the keypoint) is enough for adequate animation.  

##### ✏️My Notes:

- (3/5) So-so model name, FOMM is really awkward to pronounce and is not meme-able at all. Try harder!  

- Does a better image animation model exist? Because I could not find anything besides the new paper from the same team  
- Overall, the core intuition turned out to be very elegant, but I spent two days trying to dig through the math and confusing figures to figure it out. The paper UX could really use some work.  
- To be honest, the image depicting the pipeline did more harm than good in helping me understand the intuition when I read the paper. The rectangles representing the affine transforms confused the heck out of me.  
- It still blows my mind how good the results look even a couple of years after the paper came out  
- I was particularly surprised by how little FOMM cares about alignment between the target and source frames, it is curious that this self-supervised keypoint detector idea is not used in GANs that aim to decouple position from style (hello Alias-free GAN)  
- I am curious how well FOMM handles out of domain samples. Can it animate animal faces or other images that look like a face, but aren’t a photorealistic depiction of a face. Because it seems that the animated cartoons are somewhat of a cheat as they simply use inversion and StyleGAN blending to transform the animated frames into their cartoon version.  

##### 🔗 Links:

[First Order Motion Model arxiv](https://papers.nips.cc/paper/2019/file/31c0b36aef265d9221af80872ceb62f9-Paper.pdf) / [First Order Motion Model Github](https://aliaksandrsiarohin.github.io/first-order-model-website/)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Train a NeRF in Seconds - Instant Neural Graphics explained]({% post_url 2022-1-18-Instant-Neural-Graphics-Primitives-explained %})
- [Is StyleGAN Good? - Third Time is the Chart explained]({% post_url 2022-2-2-Third-Time-Is-The-Charm-explained %})
- [Casual GAN Paper Awards - Best Papers of 2021]({% post_url 2022-1-28-Casual-GAN-Papers-Awards %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
