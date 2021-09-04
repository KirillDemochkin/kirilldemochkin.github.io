---
layout: post
title: "9: ReStyle Explained"
meta_title: "ReStyle Explained"
description: "ReStyle: A Residual-Based StyleGAN Encoder via Iterative Refinement by Yuval et al. explained in 5 minutes."
date: 2021-4-9 19:00:00 -0000
categories: iterative-styleGAN-inversion-residual-encoding
---

#### ReStyle: A Residual-Based StyleGAN Encoder via Iterative Refinement by Yuval et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![ReStyle Samples](/assets/images/restyle_teaser.jpg "ReStyle Samples")

##### 🎯 At a glance:

The authors propose a fast iterative method of image inversion into the latent space of a pretrained StyleGAN generator that acheives SOTA quality at a lower inference time. The core idea is to start from the average latent vector in W+ and predict an offset that would make the generated image look more like the target, then repeat this step with the new image and latent vector as the starting point. With the proposed approach a good inversion can be obtained in about 10 steps.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [pSp](https://t.me/casual_gan/16)
2) [e4e]({% post_url 2021-4-13-e4e-explained %})

***

##### 🔍 Main Ideas:

**1) model architecture**  
Interestingly, ReStyle is agnostic to the encoder architecture and can be used with any StyleGAN encoder.  However, due to the self-correcting multi-step nature of the model the authors sought to reduce the complexity of the base encoder, and used a simplified version of the [pSp](https://t.me/casual_gan/16) and [e4e]({% post_url 2021-4-13-e4e-explained %}) encoders (only the 16x16 feature maps are used in the [map2style blocks](https://t.me/casual_gan/16) instead of the multiple scales of feature maps in the original encoders) in most of their experiments. The losses from the underlying encoder models are all used as is.

**2) Iterative refinement**  
Each training iteration consists of ~10 repeated steps. At each step the encoder receives the current image (initialized with the image produced from the average latent vector) concatenated along the channel dimension with the target image and produces a set of W+ vector offsets that are added to the latent code from the previous step (initialized with the average latent code). The resulting W+ vectors are given to the pretrained generator to produce an image that is used as the input in the next step.

**3) Insights from experiments:**  
Upon observing the magnitude of changes in the inverted images the authors concluded that their iterative scheme works in a coarse to fine manner, first focusing on low frequency features and then refining the smaller details. In their experiments the authors show that the editability of e4e encoded images remains intact with the proposed modifications.

**4) Encoder bootstrapping:**  
Authors consider the task of creating a cartoon character from an input photo using ReStyle. The obvious way to do this is to initialize the starting latent code with the average latent vector from the cartoon generator, and do ~10 ReStyle steps to obtain the final image. However, a smarter way is to first invert the input image into the real face domain of StyleGAN using a pretrained encoder, and initialize ReStyle's starting latent code with the resulting W+ vector. Doing so allows to obtain a high quality cartoon reconstruction of the input image in just a few ReStyle steps.

##### 📈 Experiment insights / Key takeaways:
It is hard to definitively judge the quantitative comparison of the methods since there is a tradeoff between inference speed and reconstruction quality. See Fig. 5 in the paper for more details.

CelebA-HQ (ID Similarity / Inference time)
The best (at inference time of 0.5 second) is authors' ReStyle pSp encoder @ 0.66 (next best is authors' ReStyle e4e encoder @ 0.51)
***

##### 🖼️ Paper Poster:

![ReStyle Paper Poster](/assets/images/restyle.png "ReStyle Paper Poster")

***

##### ✏️My Notes:

- The name is good, 8/10
- I played around with their demo, and while the inverted real world images are very good, I am not sure about the editability of these inversions. For example I could not get [StyleCLIP](https://t.me/casual_gan/18) to edit the inverted images at all compared to images inverted with [e4e]({% post_url 2021-4-13-e4e-explained %}) (although these inversions do not look very much like the real world image inputs)
- I think the question is: how can we do a single nonlinear update to the latent code instead of 10 linear steps to invert the image in a single forward pass? 
- Also I wonder if it is possible to "prune" the encoder during inference, and do just 2 or 3 big approximate steps that combine the smaller steps that the encoder learns during training.

##### 🔗 Links:
[ReStyle arxiv](https://arxiv.org/abs/2103.00020) / [ReStyle Github](https://github.com/yuval-alaluf/restyle-encoder)

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
