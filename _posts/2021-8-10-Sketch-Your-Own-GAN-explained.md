---
layout: post
title:  "44: Sketch Your Own GAN Explained"
meta_title: "Sketch Your Own GAN Explained"
description:  "Sketch Your Own GAN by Sheng-Yu Wang et al. explained in 5 minutes."
date: 2021-8-10 22:00:00 -0000
categories: few-shot-user-guided-gan-domain-adaptation
---

#### Sketch Your Own GAN by Sheng-Yu Wang et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![Sketch Your Own GAN Samples](/assets/images/sketchgan.gif "Sketch Your Own GAN teaser")

##### 🎯 At a glance:

Want to quickly train an entire GAN that generates realistic images from just two quick sketches done by hand? Heng-Yu Wang and team got you covered! They propose a new method to fine-tune a GAN to a small set of user-provided sketches that determine the shapes and poses of the objects on the synthesized images. They use domain adversarial loss and different regularization methods to preserve the original model's diversity and image quality.

##### ⌛️ Prerequisites:

**(Check these out if you have extra time):**  
Nothing really, enjoy the summary 🙂

***

##### 🚀 Motivation:

The authors motivate the necessity of their approach mainly with the fact that training conditional GANs from scratch is simply a lot of work: you need powerful GPUs, annotated data, careful alignment, and pre-processing. In order for an end-user to generate images of a cats in a specific pose a very large number of such images is required, however with the proposed approach only a couple of sketches and a pretrained GAN is needed to create a new GAN that synthesizes images resembling the shape and orientation of sketches, and retains the diversity and quality of the original model. The resulting models can be used for random sampling, latent space interpolation and photo editing.

***

##### 🔍 Main Ideas:

**1) Cross-Domain Adversarial Learning:**  
The proposed approach is to modify the weights of a pretrained GAN model from distribution X (the natural images) so that it generates images from X with corresponding sketches (obtained from a pretrained image -> sketch network) from distribution Y (the set of user-provided sketches). In this setup the discriminator looks at "real" sketches (user input), and the sketches created by a pretrained domain translation network (Photosketch in this case).

**2) Image Space Regularization:**  
Using just the sketches results in severe image quality degradation, hence  a second adversarial loss is used on the generated images (before transforming them to sketches), and the real images from X (the original train set). Weight regularization is considered as an alternative, but it slightly worse than using an adversarial loss.

**3) Optimization:**  
Tuning the whole generator leads to severe artifacts due to overfitting, hence only the weights of the mapping network are updated, while the rest of the generator is frozen. In other words, the input noise vectors are remapped to a different latent space W. Authors use a pretrained Photosketch model with frozen weights, while the. A translation augmentation is used for training the model with just a few user sketches.

##### 📈 Experiment insights / Key takeaways:

- The considered baselines are: Sketch Based Image Retrieval (SBIR), and Chamfer Distance matcher (both are beaten in terms of FID)
- Not all sketches benefit from augmentations
- Regularization is vital for image quality and sketch matching
- The method generalizes to real human sketches well

***

##### 🖼️ Paper Poster:

![Sketch Your Own GAN paper poster](/assets/images/sketchgan.png "Sketch Your Own GAN Paper Poster")

***

##### ✏️My Notes:

- (4/5) Sketch your GAN is a hilarious name
- I wish there was a colab demo to play with 😕
- Would love to see this as an interactive editing app, where you draw a beard or glasses on an image to change it.
- I think a cool idea would be to train an MLP that maps a Z/W vector from one latent space to a Z/W vector in a different generator's latent space
- I'll do another domain adaptation paper next, and we can compare them, stay tuned!
- Have you tried Sketch your GAN? Let me know in the comments!

##### 🔗 Links:
[Sketch Your Own GAN arxiv](https://arxiv.org/pdf/2108.02774.pdf) / [Sketch Your Own GAN github](https://github.com/PeterWang512/GANSketching)

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
