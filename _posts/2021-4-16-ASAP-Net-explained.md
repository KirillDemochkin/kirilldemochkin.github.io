---
layout: post
title: "11: ASAP-Net Explained"
meta_title: "ASAP-Net Explained"
description: "Spatially-Adaptive Pixelwise Networks for Fast Image Translation by Shaham et al. explained in 5 minutes."
date: 2021-4-16 19:00:00 -0000
categories: coordinate-based-mlp-fast-image-to-image-Pix2PixHD
---

#### Spatially-Adaptive Pixelwise Networks for Fast Image Translation by Shaham et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![ASAP-Net Samples](/assets/images/asapnet_teaser.png "ASAP-Net Samples")

##### 🎯 At a glance:

The authors propose а novel architecture for efficient high resolution image to image translation. At the core of the method is a pixel-wise model with spatially varying parameters that are predicted by a convolutional network from a low-resolution version of the input. Reportedly, an 18x speedup is achieved over baseline methods with a similar visual quality.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [Pix2PixHD](https://arxiv.org/abs/1706.03762)

***

##### 🔍 Main Ideas:

**1) Spatially Adaptive Pixelwise Networks:**  
The output image is obtained independently pixel by pixel (can be parallelized) from a multilayered perceptron with spatially varying weights. Spatial information is preserved by passing the pixel's coordinates along with its color value. Moreover, while the MLP architecture is shared for all pixels, its weights vary for different pixels. In essence, this means that the model learns to predict how to process each image, and then every image is processed by a different set of pixelwise networks based on the input.

**2) Predicting network parameters from a low resolution input:**  
A convolution network takes a downsampled version of the input image and predicts a low resolution (16x16 for 1024x1024 images) grid of parameters for the pixelwise networks. The parameter grid is upsampled to the required resolution via nearest neighbor interpolation.

**3) Positional encoding:**  
To help the model learn to synthesize high frequency features the authors project the pixels' coordinates to a higher dimensional space. Namely, they use fourier features, which are a set of sine and cosine functions with different frequencies evaluated at the

**4) Training and implementation details:**  
The training procedure, discriminator, and losses are the same as for Pix2PixHD and SPADE. Without positional encoding the network is unable to generate high frequency details. Without spatial variations the expressiveness of the model is severely limited. There is a tradeoff between faster inference for more aggressive downsampling and visual quality for less downsampling.

##### 📈 Experiment insights / Key takeaways:

See the attached picture bellow!  
The main takeaway is that the model works way faster than the competing methods while maintaining a similar visual quality.

***

##### 🖼️ Paper Poster:

![ASAP-Net Paper Poster](/assets/images/asapnet.png "ASAP-Net Paper Poster")

***

##### ✏️My Notes:

- The name sounds dope, but a bit on the nose with the words that make up the acronym, 8/10  
- Interesting that there are no artifacts on the edges of the regions with different network parameters after parameter grid upsampling  
- I wonder if it is possible to do modify this approach for resolution free image synthesis if instead of fixing the coordinates for each pixel, the coordinates were sampled from regions with the same network parameters on the upsampled grid  
- I have not tried their demo, feel free to comment on the results if you have played with the authors' code  
- Finally a straightforward paper that can be explained in under 5 minutes 😁  

##### 🔗 Links:
[ASAP-Net arxiv](https://arxiv.org/abs/2012.02992) / [ASAP-Net Github](https://tamarott.github.io/ASAPNet_web)

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
