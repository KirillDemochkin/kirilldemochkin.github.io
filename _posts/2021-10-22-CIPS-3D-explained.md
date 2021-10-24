---
layout: post
title: "65: CIPS-3D"
meta_title: "CIPS-3D"
description: "CIPS-3D: A 3D-Aware Generator of GANs Based on Conditionally-Independent Pixel Synthesis by Peng Zhou et al. explained in 5 minutes"
date: 2021-10-22 19:00:00 -0000
categories: 3D-aware-GAN-based-on-CIPS-and-NeRF
---

#### CIPS-3D: A 3D-Aware Generator of GANs Based on Conditionally-Independent Pixel Synthesis by Peng Zhou et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![CIPS-3D teaser](/assets/images/cips3d_teaser.gif "CIPS-3D Teaser")

##### 🎯 At a glance:

If you have been following generative ML for a while you might have noticed more and more GAN papers focusing on the underlying 3D representation of the generated images. CIPS-3D is a 3D-aware GAN model proposed by Peng Zhou and the team at Shanghai Jiao Tong University & Huawei that combines a low-res NeRF (surprise) with a CIPS generator (genuine surprise) to achieve high quality 256x256 3D-aware image synthesis as well as transfer learning and 3D-aware face stylization.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [CIPS]({% post_url 2021-6-12-CIPS-explained %})  
2) [NeRF]({% post_url 2021-4-6-NeRF-explained %})

***

##### 🚀 Motivation:

Existing methods either use a full NeRF that is limited to low resolution due to memory constraints or a small NeRF that is combined with a 2D CNN upsampler with an unfortunate aliasing side-effect due to an unideal filter. Whereas CIPS synthesizes each pixel independently without any upsampling, hence bypassing this issue altogether. Furthermore, many 3D-aware GANs suffer from a mirror symmetry problem that is solved in CIPS-3D with an auxiliary discriminator that regularizes the NeRF output. CIPS training is typically very time-intensive, and CIPS-3D provides a faster modulated fully connected layer for faster training.

***

##### 🔍 Main Ideas:

**1) Modified NeRF:**  
A shallow NeRF with 3 SIREN blocks is used as the first few layers of the generator network. Each block consists of a fully connected layer followed by FiLM conditioning (a fancy way to say AdaIN, where instead of mean and std you have frequency and phase) and a Sine activation function. The NeRF module does not use viewing direction and predicts a generalized feature vector instead of a color. The output of this shallow NeRF is computed via classical volume rendering techniques.

**2) INR Network for Appearance:**  
The shallow NeRF is followed by a pixel-wise CIPS generator that produces the final RGB image by summing up the outputs of the intermediate toRGB layers after each block. Partial gradient propagation is used to train on high-res images. This technique leverages the pixel-wise nature of the generator and backpropagates the gradient through the NeRF module for a random subset of all sampled pixels to fit the model into memory. Additionally, the modulated fully connected layer is reimplemented in a more efficient way using batch matrix multiplication.

**3) Overcoming Mirror Symmetry:**  
Apparently, 3D-aware models suffer from a mirror symmetry problem, where synthesized images suddenly flip horizontally when the camera passes the center line of symmetry. CIPS-3D solves this problem using learnable positional encoding along with an auxiliary discriminator that looks directly at the shallow NeRF output

##### 📈 Experiment insights / Key takeaways:

- Datasets: FFHQ
- Baselines: CIPS-3D outperforms GIRAFFE, pi-GAN, StyleNeRF in terms of FID and KID and almost matches StyleGAN-2 on 2D images
- 27.78 batches per second with batch size of 4096 on 8 V100
- Ablations: adding viewing direction - identity inconsistencies, learned positional encoding without the auxiliary discriminator hurts FID, 96x96 pixels is enough for the partial backpropagation to work almost as good as using all pixels
- By freezing the NeRF layer and fine-tuning the INR, it is possible to do transfer learning to a different domain
- Swapping or interpolating INR layers between different models has the same effect as cartoonization in StyleGAN2

***

##### 🖼️ Paper Poster:

![CIPS-3D poster](/assets/images/cips3d.jpg "CIPS-3D Paper Poster")

***

##### 🛠 Possible Improvements:

- Current stylization only works for generated images, since there isn’t an encoder for real images

##### ✏️My Notes:

- (2/5) CIPS-3D is a so-so name - not very clever nor funny 
- Real happy to see a CIPS follow-up!
- Partial gradients are a great idea, I had a lot of headaches fitting things into memory when inverting CIPS
- Unfortunately, there is still the heavy texture sticking that plagued CIPS, and the results are in 256x256
- I wonder if the authors tried to train progressively to upscale CIPS-3D to 1024x1024
- The transfer learning is awesome, just one question - why no experiments with CLIP?!
- Share your thoughts on CIPS-3D in the comments!

##### 🔗 Links:
[CIPS-3D arxiv](https://arxiv.org/pdf/2110.09788.pdf) / [CIPS-3D github](https://github.com/PeterouZh/CIPS-3D)

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [SOTA High Resolution Inpainting - LaMa explained]({% post_url 2021-10-16-LaMa-explained %})
- [StyleGAN + NeRF = StyleNeRF]({% post_url 2021-10-12-StyleNeRF-explained %})  

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
