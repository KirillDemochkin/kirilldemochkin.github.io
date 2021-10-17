---
layout: post
title: "63: LaMa"
meta_title: "LaMa"
description: "Resolution-robust Large Mask Inpainting with Fourier Convolutions by Suvorov et al."
date: 2021-10-16 19:00:00 -0000
categories: large-masks-fourier-convolutions-inpainting
---

#### Resolution-robust Large Mask Inpainting with Fourier Convolutions by Suvorov et al.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![LaMa teaser](/assets/images/lama_teaser.gif "LaMa Teaser")

##### 🎯 At a glance:

Ever tried to take a scenic picture just to be photobombed by some random tourists? Don’t worry, Roman Suvorov and the team at SAIC-Moscow recently unveiled a model called LaMa (large mask inpainting) that takes care of it for you. The model excels at inpainting large irregular masks using fast Fourier convolutions that have a receptive field equal to the entire image and a specialized wide receptive field perceptual loss that boosts the consistency for distant regions of an image.! A surprising yet extremely useful outcome of the paper is that the pretrained model scales up to 2k resolutions quite trivially.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [Fast Fourier Convolutions](https://papers.nips.cc/paper/2020/hash/2fd5d41ec6cfab47e32164d5624269b1-Abstract.html)

***

##### 🚀 Motivation:

Traditionally two-stage systems are used to train inpainting models, however, LaMa skips the intermediate predictions (blurred image/segmentation masks) and achieves SOTA results in a single step. Moreover, a large effective receptive field is required to correctly inpaint missing parts of an image since for large and wide masks the model might receive an entirely empty signal if the whole receptive field falls into the missing region. Whereas existing approaches convolutional models with slowly-growing receptive fields, LaMa acquires global context right away using Fast Fourier Convolutions that have an image-wide effective receptive field, hence the model is able to use information from all parts of an image to correctly fill in even large masked regions.

***

##### 🔍 Main Ideas:

**1) Global context within early layers:** 
LaMa receives a masked image along with the mask as a 4-channel tensor and outputs the resulting RGB image in a fully convolutional manner. It is essential for inpainting models to have access to global context ASAP since otherwise, the generator might observe regions containing just the missing pixels, and as a result, computational resources and model parameters would be wasted to build up the context over a series of layers. LaMa addresses this problem with a fully-differentiable Fast Fourier Convolutions, a recently proposed layer that processes input in two branches: a local branch uses standard convolutions and a global branch that uses a channel-wise real FFT to transform the input into the frequency domain, performs a 1x1 convolution in the frequency domain and recovers the spatial structure with a real IFFT. The outputs of the two branches are concatenated channel-wise. The use of fast Fourier Convolutions has two important benefits: they give the inpainting model access to the image-wide global context right away and capture periodic structures often found in images (e.g. brick walls, ladders, etc).

**2) Loss functions:**  
To better capture the global structure the authors propose a high receptive field (HRF) perceptual loss. It is computed as an interlayer mean of intralayer means of a squared difference between the HRF base functions of the input and output images. The HRF can be computed with dilated or Fourier convolutions. Importantly, the perceptual loss’ base model trained on segmentation works much better than the ones trained on classification.

**3) Adversarial loss:**  
LaMa is trained with a non-saturating adversarial loss, where the discriminator works on a local patch level and receives “fake” labels only for areas that intersect with the masks. Additionally, a feature matching loss on the discriminator features is used.

**4) Generating masks:**  
LaMa is trained with aggressively generated large masks that uses samples from polygonal chains dilated by a high random width and rectangles of arbitrary aspect ratios. The increased diversity of masks is beneficial for inpainting.

##### 📈 Experiment insights / Key takeaways:

- Datasets: Places, CelebaA
- Baselines: CoModGAN, MADF. Both use 3-4 times more parameters and generate noticeably worse results.
- Ablations: FFT convolutions, large masks, and HRF perceptual loss boost FID especially for high-res inputs generalization (this model is 20% slower and 40% smaller than the variant without FFT convolutions)
- LaMa trained on 256x256 patches of 512x512 images generalizes to 1536x1536 without additional training.

***

##### 🖼️ Paper Poster:

![LaMa poster](/assets/images/lama.jpg "LaMa Paper Poster")

***

##### 🛠 Possible Improvements:

- LaMa struggles with strong perspective shifts on out-of-distribution images
- Incorporate ViT as an alternative to FFT convolutions

##### ✏️My Notes:

- (5/5) for the name. 🦙Lamas are cute, and the model name is top-notch!
- The images look insanely good, high-res versions really caught me by surprise since I am used to seeing blurry repetitive textures
- disclaimer: this paper is written by my colleagues
- The ability to generalize to higher resolution is the killer feature here for sure
- I really want an online demo of this with an intuitive UI for making masks
- Could be interesting to have multimodal inpainting by adding some sort of a style vector condition to the generator
- I would be interested to see CLIP-guided inpainting, where you could tell the model, what should be on the image instead of the mask.
- Share your thoughts on LaMa in the comments!

##### 🔗 Links:
[LaMa arxiv](https://arxiv.org/pdf/2109.07161.pdf) / [LaMa github](https://github.com/saic-mdal/lama)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Unsupervised Editing Directions Discovery in GANs]({% post_url 2021-10-5-Unsupervised-Directions-Discovery-explained %})
- [WarpedGANSpace - Unsupervised editing directions followup]({% post_url 2021-10-8-WarpedGANSpace-explained %})
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
