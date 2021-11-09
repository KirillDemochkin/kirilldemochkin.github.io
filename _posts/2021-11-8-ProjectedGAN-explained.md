---
layout: post
title: "68: Projected GANs"
meta_title: "Projected GANs"
description: "Projected GANs Converge Faster by Axel Sauer et al. explained in 5 minutes"
date: 2021-11-8 19:00:00 -0000
categories: data-efficient-fast-gan-training-small-datasets
---

#### Projected GANs Converge Faster by Axel Sauer et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![Projected GANs teaser](/assets/images/projectedgan_teaser.png "Projected GANs Teaser")

##### 🎯 At a glance:

Despite significant progress in the field training GANs from scratch is still no easy task, especially for smaller datasets. Luckily Axel Sauer and the team at the University of Tübingen came up with a Projected GAN that achieves SOTA-level FID in hours instead of days and works on even the tiniest datasets. The new training method works by utilizing a pretrained network to obtain embeddings for real and fake images that the discriminator processes. Additionally, feature pyramids provide multi-scale feedback from multiple discriminators and random projections better utilize deeper layers of the pretrained network.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [StyleGAN-ADA]({% post_url 2021-4-19-StyleGAN-2-Ada-explained %})  
2) [FastGAN](https://arxiv.org/abs/2101.04775)


***

##### 🚀 Motivation:

The discriminator in a GAN first projects samples into a shared latent space and then performs classification on their embeddings. The authors suggest that using a pretrained model as an encoder improves the quality of feedback that the discriminator provides. Unfortunately, using pretrained models to control and improve GANs is not trivial as the results are typically a step below regular GANs and do not work for a more challenging unconditional setting. Another potential vector of work for GAN design is using multi-scale discriminators for improving consistency by obtaining feedback at various resolutions. However, this setup is not often used due to high memory and computational costs. Using low dimensional representations from a pretrained model, however, appears to be a good solution to this issue.

***

##### 🔍 Main Ideas:

**1) Consistency:**  
First things first, the authors use a fixed pretrained to project fake and real samples into the discriminator’s latent space. The generator and discriminator now optimize the distribution of samples in the pretrained feature space, not in the image space.

**2) Model Overview:**  
The multi-scale discriminator works on features obtained from several layers of the pretrained encoder. Each discriminator is a convolutional network with spectral normalization that all output logits at 4x4 resolution. The total loss is the sum of all logits of all discriminators. It is hypothesized that deeper layers of the pretrained model are significantly harder to cover, hence the discriminator can just ignore parts of the latent space altogether. One possible strategy to dilute dominating features is Cross-Channel Mixing (CCM). Random projection should be information preserving and not easily invertible. CCM is implemented as a randomly-initialized 1x1 convolution without activation. This random projection is applied at each scale before the features are passed to the discriminator.  
The other is Cross-Scale Mixing (CSM). CSM is implemented as a simple U-Net with random 3x3 convolutions and an upsample.

##### 📈 Experiment insights / Key takeaways:

- Starting from ablations on LSUN-Church: Multiple discriminators with augmentations are better than one, compact efficient-nets are better feature extractors than ResNets (including CLIP) and transformers, using both CCM and CSM leads to the best FID.
- Baselines: StyleGAN2-ADA, FastGAN
- ProjectedGAN training helps both baselines, and Projected FastGAN reaches StyleGAN2 FID in just 1.1 mil images (3 hours) compared to 88 mil (5 days) for the original result
- ProjectedGAN outperforms the baselines on all datasets with limited data/training time
- Training ProjectedGAN for longer improves the generated sample diversity
- ProjectedGAN works even on images not from ImageNet that the feature extractor was trained on

***

##### 🖼️ Paper Poster:

![Projected GANs poster](/assets/images/projectedgan.jpg "Projected GANs Paper Poster")

***

##### 🛠 Possible Improvements:

- Fix floating heads on blurry backgrounds
- Improve random sample quality on FFHQ
- Design a generator that combines StyleGAN and FastGAN specifically for projected training

##### ✏️My Notes:

- (4/5) No jokes, but a powerful name, nonetheless. On par with StyleGAN
- I really like the focus of this paper on improving the often overlooked discriminator
- I was going to write about a CLIP discriminator… but the authors are one step ahead on this one, props!
- A pokemon dataset reported in the paper?! Awesome!!!
- It is mentioned that the generator’s latent space is volatile during training, I wonder how well it is suited for inversion, editing, interpolation, etc
- What do you think about ProjectedGAN? Share your thoughts in the comments!

##### 🔗 Links:
[Projected GANs arxiv](https://studios.disneyresearch.com/app/uploads/2021/04/Adaptive-Convolutions-for-Structure-Aware-Style-Transfer.pdf) / [Projected GANs github](https://github.com/RElbers/ada-conv-pytorch) / Projected GANs Demo - ?

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [Essence transfer with TargetCLIP explained]({% post_url 2021-10-26-TargetCLIP-explained %})  
- [AdaConv explained - the best artistic style transfer model]({% post_url 2021-11-3-AdaConv-explained %})  
- [CIPS-3D]({% post_url 2021-10-22-CIPS-3D-explained %})  

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
