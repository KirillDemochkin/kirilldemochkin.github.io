---
layout: post
title:  "31: Alias-Free GAN Explained"
date:   2021-6-25 19:00:00 -0000
categories: digital-signal-processing alias-free-gan texture-sticking styleGAN2 latent-interpolation
---
  
#### Alias-Free GAN by Tero Karras  et al. explained in 10 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌕

***
![Alias-Free GAN by Tero Karras et al. samples](/assets/images/alias_free_poster.gif "Alias-Free GAN by Tero Karras")
Look at the beard on the image on the left. It is moving separately from the face  
![Alias-Free GAN by Tero Karras et al. samples](/assets/images/aliasfree_teaser.png "Alias-Free GAN by Tero Karras")
Alias-Free GAN vs StyleGAN2

##### 🎯 At a glance:

StyleGAN2 is king, except apparently it isn't. Tero Karras and his pals at NVIDIA developed a modification of StyleGAN2 that is just as good in terms of image quality, yet drastically improves the translational and rotational equivariance of images. In other words, the synthesis process no longer depends on absolute pixel coordinates, textures are not sticking to coordinates, instead moving together with the corresponding objects. This is a big deal since slight changes to the architecture solve fundamental problems with the generator's design making GANs better suited for video and animation.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core contributions of this paper):*
1. [Aliasing](https://en.wikipedia.org/wiki/Aliasing)
2. [StyleGAN-2](https://github.com/NVlabs/stylegan2-ada-pytorch)

***

##### 🔍 Main Ideas:

**1) Motivation:**
Authors claim that current GAN architectures do not synthesize images in a hierarchical manner despite being designed to. Instead coarse features tend to control the presence of finer details but their location is nailed to specific pixel coordinates. This causes a "texture sticking artifact" in latent interpolations breaking the illusion of a solid and coherent object moving in space. They further claim this a side-effect of non-ideal upsampling filters and pointwise application of non-linearities which causes aliasing that is amplified through the layers of the generator. Inspired by digital signal processing the authors apply similar ideas to overhaul the design of the generator, namely they treat images as discrete sample grids that represent [bandlimited functions](https://en.wikipedia.org/wiki/Bandlimiting) on a continuous domain. Moreover, they enforce continuous translational and rotational equivariance, and design alias-suppressing upsampling filters. (Refer to section 2 of the paper for technical details)

**2) Fourier features and baseline simplifications (configs B-D):**
First, the learned constant input to the generator is replaced with fixed Fourier features (uniformly sampled frequencies within the circular frequency band = 2) matching the original 4x4 resolution. Next, the per-pixel noise inputs are removed completely since they are independent of any transofrmation of underlying features. Finally, the mapping network's depth is reduced, and both mixing and path length regularizations are disabled. Interestingly, skip connections to the output are replaced with a normalization of feature maps before each convolution by the square root of the exponential moving average of all pixels and feature maps during training. All of these changes keep FID the same, and slightly improve translation equivariance.

**3) Boundaries and upsampling (config E):**
It is known that border padding leaks absolute image coordinates into the internal representations, hence a 10-pixel margin is maintained around the target canvas, cropping to this extended canvas after each layer. The 2x bilinear upsampling filter is replaced with a [sinc filter](https://en.wikipedia.org/wiki/Sinc_filter) with a [Kaiser window](https://en.wikipedia.org/wiki/Kaiser_window) of 6 (each input pixel affects 6 output pixels, and each output pixel is affected by 6 input pixels). At this point the filter cutoff is set exactly at the bandlimit (half the width of the canvas in pixels). This is called [critical sampling](https://en.wikipedia.org/wiki/Nyquist%E2%80%93Shannon_sampling_theorem#Critical_frequency).

**4) Filtered nonlinearities (config F):**
Applying ReLU in the continuous domain may introduce very high frequencies that cannot be represented in the output. This is approximately solved by wrapping each leaky ReLU in an upsample and a downsample (up -> ReLU -> down). For efficiency the nonlinearity upsampling is fused with the 2x upsampling introduced earlier. This is done with a custom CUDA kernel for a 10x speedup.

**5) Non-critical sampling (config G):**
TO further suppress aliasing a lower cutoff frequency is used on all layers except for the highest-resolution ones. The number of feature maps is set inversely proportional to the cutoff frequency to compensate for the reduced amount of spatial information in the signal.

**6) Transformed Fourier features (config H):**
An additional affine transformation is added before the fourier features that allows the generator to have an explicit rotation and translation parameters based on the style vector.

**7) Flexible layer specifications (config T):**
The authors use 14 layers with varying cutoff frequencies heuristically finetuned to maximize the suppression of aliasing.

**8) Rotation equivariance (config R):**
Rotation equivariance is obtained by replacing all 3x3 convolutions with 1x1 convolutions, doubling the number of feature maps and the replacing sinc-based filter (for all but the last two layers) with a [jinc-based](https://legacy.imagemagick.org/Usage/filter/#jinc) one.

##### 📈 Experiment insights / Key takeaways:
- This is already a huge post so I'll just say this: Alias-free GAN is as good as StyleGAN2 in terms of feed, slightly more computationally expensive, and produces latent interpolations that make StyleGAN-2 look like a school project by comparison.

***

##### 🖼️ Paper Poster:

![Alias-Free GAN by Tero Karras et al. explained in 10 minutes.](/assets/images/aliasfreegan.png "Alias-Free GAN by Tero Karras Paper Poster")

***

##### ✏️My Notes:
- (3/5) for the name; simple and to the point; no meme detected
- Man, a very technical paper. If you want the nitty-gritty details, check out the full paper, as I am waaay underqualified to explain all this digital signal processing stuff. Additionally there are links to DSP related stuff throughout the post
- Another paper in the MLP in computer vision trend
- You don't notice it, but when you see the samples from the paper it becomes apparent how far from perfect StyleGAN is
- I wonder how this affects all of the encoders for StyleGAN, especially the editability of inverted images
- I really like their "config A", "config D" convention, makes it sound like we are looking at different Tesla models.
##### 🔗 Links:
[Paper](https://arxiv.org/pdf/2101.04061.pdf) / [Code](https://github.com/NVlabs/alias-free-gan)

***

##### 👋 Thanks for reading!
*If you found this paper digest useful, subscribe and share the post with your friends and colleagues to support Casual GAN Papers!*

*Join [the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
