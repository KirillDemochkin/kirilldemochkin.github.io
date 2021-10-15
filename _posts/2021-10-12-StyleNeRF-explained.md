---
layout: post
title: "62: StyleNeRF"
meta_title: "StyleNeRF"
description: "StyleNeRF by Anonymous ICLR 2022 submission explained in 5 minutes."
date: 2021-10-12 19:00:00 -0000
categories: unsupervised-discovery-nonlinear-latent-editing-directions-generator
---

#### StyleNeRF by Anonymous ICLR 2022 submission explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![StyleNeRF teaser](/assets/images/stylenerf_teaser.png "StyleNeRF Teaser")

##### 🎯 At a glance:

It’s a NeRF, it’s a GAN it’s Superman StyleNeRF. But no for real, it happened, two of the biggest (probably) breakthroughs of the last couple of years are joining forces. StyleGAN is great at generating structured 2D images but it has zero knowledge about the 3D world. NeRF, on the other hand, is great at understanding complex 3D scenes but struggles to generate view-consistent scenes when trained on unposed images. StyleNeRF fuses the two into a style-conditioned radiance field generator with explicit camera pose control. Seems like a perfect match! Let’s find out if it really lives up to the hype.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [StyleGAN2](https://github.com/NVlabs/stylegan2)  
2) [NeRF]({% post_url 2021-4-6-NeRF-explained %})  
3) [NeRF++](https://arxiv.org/abs/2010.07492)  
4) [PixelShuffle](https://arxiv.org/pdf/1609.05158v2.pdf)  
5) [Alias-Free GAN]({% post_url 2021-6-25-AliasFreeGAN %})  
6) [StyleCLIP](https://t.me/casual_gan/18)  

***

##### 🚀 Motivation:

Photo-realistic free-view image synthesis of real-world scenes traditionally requires difficult to obtain datasets of 3D scenes, whereas GANs can be trained on unposed images, although without any explicit control over the 3D structure of synthesized images. Although methods for generative NeRFs exist, they have issues with view consistency and cannot for the most part generate high-resolution results. StyleNeRF addresses these issues by using a style-conditioned NeRF to produce a low-resolution feature map that is upscaled to high-res via a new upsampler and a clever regularization scheme that forces the render result to be consistent with the original NeRF render. The cherry on top is that StyleNeRF is trained on unstructured (I think the authors mean “unposed” since faces are VERY structured) real-world images.

***

##### 🔍 Main Ideas:

**1) Style-based Generative Neural Radiance Field & Rendering:**  
The scene is modeled with a standard NeRF that additionally has a mapping network that projects random N(0, 1) noise to a learned latent space. The output of the mapping network is used to condition the NeRF via weight modulation of the MLP layers. Rendering is lifted from NeRF, the only change is that the MLP has more layers for color prediction than for density.

**2) Approximation for high-resolution image generation:**  
Since training a NeRF at 1024x1024 is troublesome, to say the least, the authors propose to do two things. First, they swap the order of operations in NeRF, and pre-aggregate all of the points along each ray into a 2D space so that the MLP is only called once per ray, and each pixel has a single corresponding latent vector associated with it. Second, the authors borrow an idea from Deferred Neural Rendering and output a low-res feature map from the NeRF that is then upsampled and rendered in high-res by a separate module. Unfortunately, this simple setup suffers from 3D inconsistency problems due to imperfect upsampling.

**3) Preserving 3D consistency:**  
Authors must love playing legos because they keep combing ideas from all sorts of papers. Their upsampler consists of a low-pass filter with a learnable 2-layer MLP (basically a nearest neighbor upsample + learned 1x1 Conv + blur) followed by PixelShuffle. This choice to combine the two is explained as a solution to the “checkerboard” artifact that PixelShuffle suffers from and the “bubble” artifacts that appear on images generated by the smoother low-pass upsamplers (bilinear interpolation) as the two cancel each other out, I guess. Additional tricks used to enforce multi-view consistency are: removing view direction from color prediction along with 2D noise injection (since it does not depend on the 3D structure), and using NeRF path regularization by sub-sampling pixels from the output and comparing them to those generated by the low-res NeRF (it is unclear from the paper, but I assume a separate “full” NeRF model without pre-aggregation is trained in low-res along with the rest of the pipeline)

**4) StyleNeRF Architecture:**  
More referenced papers, yay! The authors actually use NeRF++ and its dual branch setup for separate background and foreground prediction. The training idea is taken from yet another paper - ProgGAN, where the generator is sequentially trained on increasing image resolutions. Camera poses are randomly sampled during training.
##### 📈 Experiment insights / Key takeaways:

- Datasets: FFHQ, MetFaces, AFHQ, CompCars
- Baselines (in 256x256): HoloGAN, [GRAF]({% post_url 2021-7-2-GRAF %}), piGAN, [GIRAFFE]({% post_url 2021-7-8-GIRAFFE %})
- Result: StyleNeRF wins since it has much fewer artifacts at higher resolution (512+). Authors dunk on GIRAFFE for using 3x3 Conv layers
- FID is not as good as StyleGAN2 but StyleNeRF has 3D consistency
- StyleNeRF has explicit camera control, style mixing, interpolation, and editing via [StyleCLIP](https://t.me/casual_gan/18)-like interface (STILL INTRODUCING NEW PAPERS😬)
- Ablations: w/o NeRF regularization the model can produce flat shapes, view-direction reduces multi-view consistency, and the removal of progressive training lowers the quality of the end results.
***

##### 🖼️ Paper Poster:

![StyleNeRF poster](/assets/images/stylenerf.jpg "StyleNeRF Paper Poster")

***

##### 🛠 Possible Improvements:

- Guarantee 3d consistency more strictly
- Improve recovered geometry from learned density

##### ✏️My Notes:

- (4/5) for the name. A bit of low-hanging fruit, but who am I kidding, this is a solid name!
- I am astounded that there is no comparison to [FLAME-in-NeRF]({% post_url 2021-8-24-Flame-in-NeRF-explained %})
- The NeRF pre-aggregation scheme is very interesting and got me thinking about using a ConvNet inside a NeRF that treats points along the rays as channels. This would essentially make non-flat (3D warped) feature maps.
- CLIP, anyone? All jokes aside, people are already making crazy stuff with StyleGAN3 + CLIP (aka [Alias-free GAN]({% post_url 2021-6-25-AliasFreeGAN %})), and StyleNeRF will be thrown into the mix for sure when the code is released
- Dang, this paper is dense with references, did the authors bet on how many different ideas they could stuff into this paper? Luckily for you, we covered the important references on Casual GAN Papers already.😉
- Share your thoughts on StyleNeRF in the comments!

##### 🔗 Links:
[WarpedGANSpace arxiv](https://openreview.net/forum?id=iUuzzTMUw9K) / WarpedGANSpace github (Coming Soon)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Unsupervised Editing Directions Discovery in GANs]({% post_url 2021-10-5-Unsupervised-Directions-Discovery-explained %})
- [WarpedGANSpace - Unsupervised editing directions followup]({% post_url 2021-10-8-WarpedGANSpace-explained %})
- [ISF-GAN - SOTA GAN Editing]({% post_url 2021-10-1-ISF-GAN-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!