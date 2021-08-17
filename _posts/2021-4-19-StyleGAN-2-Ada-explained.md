---
layout: post
title: "12: StyleGAN-2-Ada Explained"
meta_title: "StyleGAN-2-Ada Explained"
description: "Training Generative Adversarial Networks with Limited Data by Karras et al. explained in 5 minutes."
date: 2021-4-19 19:00:00 -0000
categories: limited-data-stylegan-training-differentiable-augmentations
---

#### Training Generative Adversarial Networks with Limited Data by Karras et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![StyleGAN-2-Ada Samples](/assets/images/stylegan2ada_teaser.png "StyleGAN-2-Ada Samples")

##### 🎯 At a glance:

The authors propose а novel method to train a StyleGAN on a small dataset (few thousand images) without overfitting. They achieve high visual quality of generated images by introducing a set of adaptive discriminator augmentations that stabilize training with limited data.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [StyleGAN-2](https://arxiv.org/abs/1706.03762)

***

##### 🔍 Main Ideas:

**1) Stochastic discriminator augmentation**  
By design all augmentations applied to images during training will leak, and appear on the generated images, which is an undesirable side effect of having to use augmentations to artificially increase the size of the training set. One way to prevent the augmentation from leaking from the training images is by requiring that discriminator output is consistent for an image when two different sets of augmentations are applied to it only when training the discriminator. However, this approach causes the discriminator to be "blind" to augmentations, which is not good since the generator can create augmented images without any penalty. The proposed approach is similar in that it applies a set of augmentations to the images shown to the discriminator, however they show only the augmented images to the discriminator, and also use the augmentations when training the generator.

**2) Designing augmentations that do not leak**  
It has been shown that GANs can implicitly undo corruptions when training only on corrupted images as long as the augmentations used to corrupt the images allow for a way to tell if two sets of augmented images are equal without knowing the underlying images without the augmentations. A simple example of stochastic non-leaking augmentation is randomly rotating images by 0, 90, 180, 270 degrees 10% of the time since this increases the relative occurence of images at 0 degrees, and the only way for the generator to match the distribution of real images is to generate images in the correct orientation. Most deterministic augmentations (additive noise, flips, shifts, scaling, etc) can be made non-leaking by applying them only p% of the time. A safe value of  p is less than 0.8.

**3) Augmentation pipeline**  
The authors use 18 augmentations in a predifined order all applied independently with the same probability. The large number of augmentations makes it extremely unlikely that the discriminator will ever see an image without augmentations, yet the generator is guided to produce only clean images, as long as p remains under the safe value.

**4) Adaptive discriminator augmentation**  
To avoid manual tuning of the strength of every augmentations the authors develop two heuristics to indicate the discriminator overfitting. The first heuristic expresses the discriminator predictions for the validation set relative to the train set and generated images. The second heuristic the portion of the training set that gets positive discriminator outputs. In practice the value of p is initialized with 0, and every couple of minibatches the two heuristics are calculated, and p is aggressively adjusted up or down to counteract overfitting.

##### 📈 Experiment insights / Key takeaways:

_MetFaces (FID)_  
The best is authors' ADA StyleGAN2 @ 18.22 for training from scratch and 0.81 for transferring from a pretrained StyleGAN2 (next best is default StyleGAN2 @ 57.26 for training from scratch and 3.16 for transferring from a pretrained StyleGAN2)

***

##### 🖼️ Paper Poster:

![StyleGAN-2-Ada Paper Poster](/assets/images/stylegan2ada.png "StyleGAN-2-Ada Paper Poster")

***

##### ✏️My Notes:

- Boring but recognizable model name, 6/10
- The results are beyond impressive, the images have no business looking this good for how small the training dataset can be
- It is interesting that for whatever reason the proposed augmentations have not yet become a standard part of the GAN pipeline.

##### 🔗 Links:
[StyleGAN-2-Ada arxiv](https://arxiv.org/abs/2006.06676) / [StyleGAN-2-Ada Github](https://github.com/NVlabs/stylegan2-ada)

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
