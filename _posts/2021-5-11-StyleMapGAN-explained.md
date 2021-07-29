---
layout: post
title: "18: StyleMapGAN Explained"
meta_title: "StyleMapGAN Explained"
description: "Exploiting Spatial Dimensions of Latent in GAN for Real-time Image Editing by Hyunsu Kim et al. explained in 5 minutes."
date: 2021-5-11 19:00:00 -0000
categories: spatial-latent-image-editing-GAN-inversion
---

#### Exploiting Spatial Dimensions of Latent in GAN for Real-time Image Editing by Hyunsu Kim et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![Exploiting Spatial Dimensions of Latent in GAN for Real-time Image Editing Samples](/assets/images/gaussianized_teaser.jpg "StyleMapGAN Samples")

##### 🎯 At a glance:

One more paper about inverting images into latent spaces of generators. This time with the twist that it uses explicit spatial styles (style tensors instead of style vectors) in the generator, and the encoder, hence making it possible to perform local edits, and smoothly swap parts of images. Overall the authors show that their approach outperforms other baseline in the aforementioned tasks as well as image interpolation.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [e4e](https://t.me/casual_gan/25)

***

##### 🔍 Main Ideas:

**1) Spatial Styles:**  
The authors of StyleMapGAN propose to use latent tensors instead of latent styles. To do that they reshape the output of their mapping network into a 64 by 8 by 8 tensor and then pass it through a resizer network that is made up of convolution and upsampling layers and outputs a set of of style tensors with spatial size matching the intermidiate feature maps in the generator. The style tensors are then used to predict modulation mean and variance for each of the generator layers.
An interesting change from the StyleGAN generator is the absence of type B noise since it added spatial variation to the generated image, and spatial variation is baked into the style tensors.

**2) Encoder:**  
The encoder network is pretty much the same thing as the patch discriminator used in StyleGAN except that it does not do minibatch discrimination, and instead predicts a single style tensor that later goes into the resizer network.

**3) Training:**  
All networks are jointly trained with multiple losses: MSE and LPIPS between real and generated images for the generator and encoder networks, adversarial loss for all of the models, and MSE loss between the predicted/sampled style tensor and the style tensor that is obtained by passing the generated image back through the encoder (In-Domain loss).

**4) Local editing:**  
The source and reference images are inverted with the encoder to obtain their style tensors that are then alpha blended in W+ according to the provided editing mask.

##### 📈 Experiment insights / Key takeaways:

- The authors introduce an interesting new metric called FID-lerp to measure how well images can be interpolated in the latent space. It is computed by measuring FID on images that are randomly interpolated between pairs of images from the test set.
- To measure the effectiveness of local edits the authors compute two types of MSE: one between the source parts of the image, and one between the inserted parts of the reference image
- Quite obviously increased style tensor resolution improves reconstruction drastically. However, it is not stated, whether the StyleMap overfits at higher resolution since there is a known tradeoff between reconstruction quality and editability.
- The authors claim the best FID for real image projections but strangely do not provide comparisons with pSp, e4e or any other encoder based architectures. Only claiming speedup over optimization based methods.

***

##### 🖼️ Paper Poster:

![Exploiting Spatial Dimensions of Latent in GAN for Real-time Image Editing Paper Poster](/assets/images/stylemapgan.jpg "StyleMapGAN Paper Poster")

***

##### ✏️My Notes:

- 3/5 - ok name, does not quite roll off the tongue.
- It is not mentioned in the paper, whether there are any engineering tricks for fitting the whole pipeline into memory as it seems the total memory requirement should be considerable.
- The most surprising property of the mode is that alignment is not required between images when editing them.

##### 🔗 Links:
[StyleMapGAN arxiv](https://arxiv.org/pdf/2104.14754.pdf) / [StyleMapGAN Github](https://github.com/naver-ai/StyleMapGAN)

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
