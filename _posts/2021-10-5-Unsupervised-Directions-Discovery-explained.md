---
layout: post
title: "60: Unsupervised Discovery of GAN Directions"
meta_title: "Unsupervised Discovery of GAN Directions"
description: "Unsupervised Discovery of Interpretable Directions in the GAN Latent Space by Voynov and Babenko explained in 5 minutes."
date: 2021-10-5 16:00:00 -0000
categories: unsupervised-discovery-editing-directions-gan-latent-space
---

#### Unsupervised Discovery of Interpretable Directions in the GAN Latent Space by Voynov and Babenko explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌑🌑🌑🌑

***

![Unsupervised Directions Discovery teaser](/assets/images/unsupervised_dir_teaser.gif "Unsupervised Directions Discovery Teaser")

##### 🎯 At a glance:

GAN-based editing is great, we all know that! Do you know what isn’t? Figuring out what the heck you are supposed to do with a latent vector to edit the corresponding image in a coherent way. Turns out taking a small step in a random direction will most likely change more than one aspect of the photo since latent spaces of most well-known generators are rather entangled, meaning that by adding a smile to the generated face you are likely to also unintentionally change the hair color, the eye shape or any number of other wacky things. In this paper by Andrey Voynov and Artem Babenko from Yandex, a new unsupervised method is introduced that discovers meaningful disentangled editing directions for simple attributes such as gender, age, etc as well as less obvious ones such as background removal, rotation, and background blur.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [StyleGAN2](https://github.com/NVlabs/stylegan2)  

***

##### 🚀 Motivation:

GAN models are notoriously tough to interpret. Currently, disentangled editing directions are discovered with supervision from pretrained attribute classifiers, yet those require pretrained models for each desired editing direction. Moreover, for some less obvious editing directions training data for the classifier may not exist at all. Alternatively, there exist GAN models that are trained from scratch with the goal of achieving an interpretable latent space out of the box, however, they often lack image quality. As such, the main contributions of this paper are 1) the first unsupervised editing direction discovery without generator re-training 2) non-trivial directions identified for popular generators 3) the new method is used in weakly-supervised saliency detection.

***

##### 🔍 Main Ideas:

**1) The Intuition:**  
How do we find directions that do not interfere with each other when manipulated? Easy, we take orthogonal vectors as the basis for our editing directions, hence moving along one of them will not affect the rest.

**2) The Method:**  
Essentially, the method consists of two parts. A matrix with either orthonormal columns (parametrized with a skew-symmetric matrix) or columns of length one (normalized) holds the vectors corresponding to the editing directions. Note: you should try both options and see which one gives better results in your case. A ResNet-18 reconstructor that receives two images generated from a random latent code and its shifted version (a randomly scaled direction vector is added from the edit matrix) and predicts the id of the applied edit vector along with the shift magnitude. The model is trained with a classification loss on the edit vector id and a regression loss on the shift magnitude.

##### 📈 Experiment insights / Key takeaways:

- Datasets covered: MNIST, AnimeFaces, CelebA-HQ, ImageNet
- Generators: ProgGAN & BigGAN (No StyleGAN experiments?)
- Metrics measured: Reconstructor Classification Accuracy & Individual Interpretability (human evaluation)
- From ablations: a small number of directions don’t learn anything, and extreme values of the regression loss collapse directions.

***

##### 🖼️ Paper Poster:

![Unsupervised Directions Discovery paper poster](/assets/images/unsupervised_dir.jpg "Unsupervised Directions Discovery Paper Poster")

***

##### 🛠 Possible Improvements:

- use RBF instead of linear directions (spoiler: this is the topic of the next paper)

##### ✏️My Notes:

- (-/5) Did the authors forget to name their model?
- Gonna be Captain Obvious, but a good idea is to model directions in a nonlinear way, and it is actually the topic of a recent paper that will be covered later this week
- I am now wondering if it is possible to discover unsupervised CLIP directions? Or maybe refine ambiguous directions such as “photo of a pretty person”
- I was thinking, it would be interesting to do self-supervised direction discovery, where you show a pair of edited photos and a pair of different photos, and ask the model to say, which are edits, and which are random samples
- Seems like it would be possible to use some tricks from VQGAN here where you have a codebook of editing primitives that combine into full-on editing directions.
- Sadly, there aren’t any experiments reported in the paper for real images
- Interestingly, all edits are performed in w and not w+
- There don’t seem to be any safeguards to handle cases when an edited attribute is already in the target state (trying to add glasses when glasses already exist)
- Do you have any questions left about (Insert Model Name)? Let’s discuss in the comments!

##### 🔗 Links:
[Unsupervised Directions Discovery arxiv](https://arxiv.org/pdf/2002.03754.pdf) / [Unsupervised Directions Discovery github](https://github.com/anvoynov/GanLatentDiscovery)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [SOTA Single Video Generative Model - VGPNN]({% post_url 2021-9-27-VGPNN-explained %})
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
