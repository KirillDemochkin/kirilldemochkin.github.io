---
layout: post
title: "74: Plenoxels"
meta_title: "Plenoxels"
description: "Plenoxels: Radiance Fields without Neural Networks by Alex Yu,  Sara Fridovich-Keil, et al. explained in 5 minutes"
date: 2021-12-18 16:00:00 -0000
categories: NeRF-3D-voxels-without-neural-networks
---

#### Plenoxels: Radiance Fields without Neural Networks by Alex Yu,  Sara Fridovich-Keil, et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![Plenoxels](/assets/images/plenoxels_teaser.gif "Plenoxels Teaser")

##### 🎯 At a glance:

Every now and then comes along an idea so pertinent that it makes all alternatives look too drab and uninteresting to even consider. NeRF, the 3D neural rendering phenomenon from last year, is one such idea… Yet, despite of the hype around it Alex Yu,  Sara Fridovich-Keil, and the team at UC Berkley chose another approach to focus on. Perhaps surprisingly, without any neural networks at all (yes, you are still reading a blog about AI papers), and even more surprisingly, their approach, coined Plenoxels, works really well! The authors replace the core component of NeRF, the color and density predicting MLP, with a sparse 3D grid of spherical harmonics. As a result, learning Plenoxels for scenes is two orders of magnitude faster than optimizing a NeRF, and there is no noticeable drop in quality whatsoever. Crazy? Yeah, let’s learn how they did it!

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**
1[NeRF]({% post_url 2021-4-6-NeRF-explained %})   

***

##### 🚀 Motivation:

NeRFs are real pretty, but they take forever to train (1 to 4 days). What can be done about that? Well, one intuition the authors had was that the power of NeRF came from the differentiable rendering engine, not the underlying model (MLP), and swapping the underlying model for simpler and faster primitives would speed things up significantly without sacrificing the visual quality. The proposed view-dependent sparse voxel grid can be further sped up with several clever tricks such as pruning, and an efficient GPU octree implementation.

***

##### 🔍 Main Ideas:

**1) Volume rendering:**  
Literally, the same as NeRF.

**2) Voxel Grid with Spherical Harmonics:**  
The grid is stored as a dense array of indices pointing to an array of values for all occupied voxels. Each occupied voxel stores its density and 9 spherical harmonic coefficients (Math Warning: https://en.wikipedia.org/wiki/Spherical_harmonics) for each color channel (27 total) with the low degree harmonics encoding smooth changes in color and higher degree harmonics - higher frequency features. The resulting color is the sum of the harmonic basis functions multiplied by the stored coefficients.

**3) Interpolation:**  
Trilinear interpolation is used to define a continuos plenoptic function throughout the volume. This approach increases the effective resolution by representing sub-voxel fluctuations in color and opacity.

**4) Coarse-to-fine:**  
The optimization is done in several stages at different resolutions. At every stage empty voxels are pruned via a threshold on the maximum weight/density over all training rays. Additionally, a voxel is pruned only if both itself and its neighbours are considered empty. Then, the resolution is doubled, and the new voxel grid is initialized with trilinear interpolation of the starting grid. Finally, the optimization process continues with the higher resolution voxel grid.

**5) Optimization:**  
The training loss is simply the MSE over pixels between rendered and ground-truth pixels combined with a TV regularizer. RMSProp is used in place of a true second-order optimization algorithm to help with the ill-conditioned nature of the direct voxel coefficient optimization problem. Additionally, a sparsity prior based on a Cauchy loss is used (simply put it encourages voxels to be empty by penalizing ray transmittance)


##### 📈 Experiment insights / Key takeaways:

- Tested on the 8 NeRF scenes against NeRF++, JAXNeRF, and LLFF

- Plenoxels are way-way faster than all other models
- Plenoxels outperform the considered baselines in terms of SSIM and LPIPS on the test set, but fall slightly short in terms of PSNR

- Ablations: trilinear interpolation heavily boosts the fidelity of the rendered scenes; increased TV-regularization allows Plenoxels to beat NeRF in a data-limited setup; going beyond second-degree harmonic functions does not lead to a noticeable improvement

***

##### 🖼️ Paper Poster:

![Plenoxels poster](/assets/images/plenoxels.jpg "Plenoxels Poster")

***

##### 🛠 Possible Improvements:

- Remove artifacts (interestingly, they appear  visually different compared to NeRF artifacts) by studying the different priors and regularizers
- Automatically select the best loss coefficients and other hyperparameters for each scene

##### ✏️My Notes:

- (4.5/5) Plenoxels is an extremely catchy and self-explanatory model name, even if it isn’t at all funny

- I really enjoy these papers (“Masked Autoencoders” is another recent example) that take shots at some of the extremely hyped new methods. Waiting for some paper to roast CLIP any day now.
- What do you think about Plenoxels? Write your comments in the chat!

##### 🔗 Links:
[Plenoxels arxiv](https://arxiv.org/abs/2112.05131) / [Plenoxels Github](https://github.com/sxyu/svox2) / [Plenoxels Site](https://alexyu.net/plenoxels/)

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [Are image transformers overhyped? - MetaFormer explained]({% post_url 2021-11-30-MetaFormer-explained %})  
- [NeRF + CLIP = Insanity - DreamFields explained]({% post_url 2021-12-7-DreamFields-explained %})  
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
