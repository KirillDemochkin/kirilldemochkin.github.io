---
layout: post
title: "61: WarpedGANSpace"
meta_title: "Unsupervised Discovery of GAN Directions"
description: "WarpedGANSpace: Finding non-linear RBF paths in GAN latent space by Tzeplis et al. explained in 5 minutes."
date: 2021-10-8 19:00:00 -0000
categories: unsupervised-discovery-nonlinear-latent-editing-directions-generator
---

#### WarpedGANSpace: Finding non-linear RBF paths in GAN latent space by Tzeplis et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![WarpedGANSpace teaser](/assets/images/warpganspace_teaser.png "WarpedGANSpace Teaser")

##### 🎯 At a glance:

Linear directions are great for GAN-based image editing, but who is to say that going straight across the latent space is the best option? Well, according to Christos Tzelepis and his colleagues from the Queen Mary University of London non-linear paths in the latent space lead to more disentangled and interpretable changes in the synthesized images compared to existing SOTA methods! Their method, which is based on optimizing a set of RBF warp functions, works without supervision and learns a set of easily distinguishable image editing directions such as pose and facial expressions.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [StyleGAN2](https://github.com/NVlabs/stylegan2)  
2) [Unsupervised Direction Discovery](https://t.me/casual_gan/136)

***

##### 🚀 Motivation:

Here is all the motivation you will need to understand, why disentangled latent space directions are paramount for computer vision tasks of all sorts: ever had an idea for an image-to-image model but paired data is expensive/unavailable. You can probably find a pretrained generator that can synthesize an endless stream of images from your domain. The only hurdle is that you have no explicit control over what attributes are and aren’t in the generated images. If only there existed a way to select the latent codes that have the exact set of attributes you require. Lucky for you, it does, and it only requires knowing the latent space directions controlling those attributes. With everything ready, you sample a latent code, move it in the direction of the desired attribute, synthesize both images with a pretrained generator, and voila, you got a pair of images for your dataset! Rinse and repeat, and before long, you will have enough pairs of images to train your model.

There are two main problems with existing approaches: they find directions that are linear (independent of the latent code), and their evaluation requires a pretrained classifier or a human expert. WarpedGANSpace successfully (at least according to the authors) addresses both issues.

***

##### 🔍 Main Ideas:
**1) Vector Space Warping and Traversal:**  
The main intuition is that the editing direction is not the same at every point in the latent space! The core technical idea of the paper is to learn a set of parametric Gaussian RBFs that warp the latent space of a pretrained generator in a differentiable way. In actuality, each point in the space gets an additional dimension corresponding to the weighted sum of these RBFs. The centers of the RBFs form a support set that defines each warping. The actual editing direction for a latent code is given by the curve formed from a series of local gradient steps in the warped vector space from the initial point.
**2) Learning non-linear interpretable curves in GAN’s latent space:**  
The RBF parameters are packed into a tensor and two matrices (since RBFs are defined as triples of center, weight, and scale respectively) and learned via regular backprop. Each warping is defined as pairs of opposite weighted vectors with the same scale that controls the degree of nonlinearity (smaller -> more linear). Linear directions can be derived as a special case of this approach.
**3) Learning Process:**  
The pipeline is fully differentiable and consists of a warping network (learned RBFs), the ResNet-based reconstructor that learns to predict which support set was used to produce a pair of images, and the magnitude of the shift in the latent space between two images. The only losses used are on the outputs of the reconstructor.

##### 📈 Experiment insights / Key takeaways:

- SN-GAN, BigGAN, ProgGAN, and StyleGAN are considered
- Baselines: linear paths, and GANSpace
- Result: paths with more distinguishable changes in the image space
- Result: interpretable paths with steeper and more disentangled changes in the image space
- Result: Nonlinear interpretable paths with steeper and more disentangled changes in the image space

***

##### 🖼️ Paper Poster:

![WarpedGANSpace poster](/assets/images/warpganspace.jpg "WarpedGANSpace Paper Poster")

***

##### 🛠 Possible Improvements:

- My suggestion: Try doing the same thing in W/W+ instead of Z, and for W+ only use a small fraction of vectors for each direction

##### ✏️My Notes:

- (3.5/5) for the name. I feel like the warp space pun is underutilized (or perhaps entirely accidental)
- I feel like there is probably at least one paper among ICLR 2022 submissions that takes the ideas presented here even further
- The idea really clicked in my brain once I realized that this is basically gradient descent in the latent space that finds the point that corresponds to the image with the edited attributed turned up to the max. While the endpoint is the same regardless of where you started (since it is the center of the cluster of images with this attribute) the optimization path that you take controls the reconstruction/editing tradeoff
- I am really bullish on these zero-shot image-to-image/domain transfer pipelines since they could sidestep the necessity to invert a real image into the latent space of a GAN with an imperfect encoder!
- I think that detecting directions in w+ is non-optimal in terms of disentanglement, there needs to be some regularization that only selects a couple of layers for each direction
- I think the main intuition correlates with the iterative approach to CLIP-guided optimization and gives a major clue on how to design a feed-forward VQGAN+CLIP solution!
- Wait, never mind, they are actually moving in the Z space, not even W. What? I guess that Z is smoother for warping?
- Share your thoughts on WarpedGANSpace in the comments!

##### 🔗 Links:
[WarpedGANSpace arxiv](https://arxiv.org/pdf/2109.13357v1.pdf) / [WarpedGANSpace github](https://github.com/chi0tzp/WarpedGANSpace)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Unsupervised Editing Directions Discovery in GANs]({% post_url 2021-10-5-Unsupervised-Directions-Discovery-explained %})
- [Instance Conditioned GAN - Arbitrary similar image generation]({% post_url 2021-9-24-Instance-Conditioned-GAN-explained %})
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
