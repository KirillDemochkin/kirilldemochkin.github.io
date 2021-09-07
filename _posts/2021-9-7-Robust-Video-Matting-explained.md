---
layout: post
title:  "52: Robust Video Matting"
meta_title: "Robust Video Matting"
description:  "Robust High-Resolution Video Matting with Temporal Guidance by Shanchuan Lin et al. explained in 5 minutes. explained in 5 minutes."
date: 2021-9-7 16:00:00 -0000
categories: cross-modal-fully-attentional-transformer
---

#### Robust High-Resolution Video Matting with Temporal Guidance by Shanchuan Lin et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

<iframe width="1813" height="788" src="https://www.youtube.com/embed/Jvzltozpbpk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

##### 🎯 At a glance:

Do you own a green screen? If you do, you might want to look into selling it because thanks to Shanchuan Lin and his gang from UW and ByteDance green screens might soon be nothing more than off-brand red carpets. Their proposed approach leverages a recurrent architecture and a novel training strategy tо beat existing approaches on matting quality and consistence as well as speed (4k @ 76FPS on a 1080ti GPU) and size (42% fewer parameters).

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
Nothing essential, enjoy the paper!

***

##### 🚀 Motivation:

Matting involves predicting an alpha mask that separates an object (or a person) in the foreground from the rest of the scene in the background. Most existing methods process video frames one-by-one, losing important temporal information. Temporal information is essential for reducing flicker, keeping predictions consistent between frames, and learn about the background to simplify matting for future frames. Training matting models usually involves synthetic datasets that do not generalize well to real data, hence the authors simultaneously train their model with a segmentation objective on real human segmentation tasks.

***

##### 🔍 Main Ideas:

**1)  Feature-Extraction Encoder:**  
The overall architecture of the encoder module follows SOTA semantic segmentation models, specifically using a MobileNetV3-Large as the backbone followed by the LR-ASPP. The encoder extracts features at 1/2, 1/4, 1/8, and 1/16 scales for the decoder.

**2)  Recurrent Decoder:**  
Interestingly, the authors go with a recurrent decoder over a transformer-based one motivated by the fact that recurrent models learn an internal update rule (what to keep and what to drop) on a continuous stream of data, whereas a hard-coded update rule is required for attention-based models. In practice, the decoder is a multi-scale ConvGRU. The Bottleneck block processes the 1/16 scale and only works with half the channels to save resources. The Upsampling blocks work on 1/8, 1/4, and 1/2 scales and concatenate the output of the previous layer with the skip-connections from the encoder before passing it to the ConvGRU, and bilinearly upsampling the result. The Output block uses regular convolutions to refine the results, and outputs a 1-channel alpha prediction, a 3-channel foreground prediction, and a 1-channel segmentation prediction.

**3) Deep Guided Filter Module:**  
The DGF module is used to upsample low-res outputs of the network for faster 4K and HD input processing.

**4) Training:**  
Image matting and segmentation training iterations are interleaved, and the whole process gets progressively harder samples at higher resolution and with more frames. The model learns from the following losses: L1 and pyramid Laplacian on the alphas, L2 on the temporal gradients of alphas, and L1 and L2 on the foregrounds, and temporal gradients of foregrounds.
   
##### 📈 Experiment insights / Key takeaways:

- Evaluated favorably against DeepLabV3, FBA, BGMv2, and MODNet on accuracy, size, and speed
- Better handles fast-moving body parts than MODNet on real-time matting
- Known limitations: does not do too well when there are people in the background, works best with simpler backgrounds.

***

##### 🖼️ Paper Poster:

![Robust Video Matting paper poster](/assets/images/deepgreen.png "Robust Video Matting Paper Poster")

***

##### ✏️My Notes:

- (2/5) - I am deeply saddened that this model is not called Deep-GREEN 😕
- I found the "prefers simple backgrounds" limitation a tad strange. Are the other models so bad that they can not handle even simple backgrounds?
- I honestly had no idea that people were not using segmentation datasets for matting tasks Oo
- Have you tried Robust Video Matting? Let's discuss in the comments!

##### 🔗 Links:
[Robust Video Matting arxiv](https://arxiv.org/abs/2108.11515) / [Robust Video Matting github](https://github.com/PeterL1n/RobustVideoMatting)

***

##### 👋 Thanks for reading!
<
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
