---
layout: post
title: "3B: 8 years of GAN history explained in 5 minutes"
meta_title: "A quick overview of StyleGAN, VQGAN, VAE, and the original GAN"
description: "I GAN Explain: 8 years of GAN evolution, and the intuition behind it."
date: 2021-11-12 19:00:00 -0000
categories: history-of-GANs-survey-of-popular-architectures
---

#### I GAN Explain: 8 years of GAN evolution, and the intuition behind it.

##### ⭐Tutorial difficulty: 🌕🌕🌑🌑🌑

***
![The evolution of GANs](/assets/images/gans_history_0.jpg "Evolution of GANs")

##### The first steps:
The most basic way to do this is to learn a **VAE (Variational Auto Encoder)**. Please don’t run away yet, it is not as scary as it sounds. VAE consists of two modules: an encoder that turns an image into a latent vector (a fancy way to say “mugshot table coordinates” from last week) and a decoder that takes a latent vector and spits out the corresponding image. The two modules learn to deconstruct and reconstruct images in tandem by encoding input images and decoding the results to reproduce the input images as close as possible to the original. Intuitively, the two models The variational part comes from a trick that is used to enable sampling from this model. A small amount of random noise is added to the latent vector each time to make sure that the latent space is tightly packed and not full of empty spaces without any images. Interestingly, only the decoder is needed to create new images since it takes the latent vector (coordinates of the image) and returns the corresponding image.

**TLDR: Variational AutoEncoders are pretty easy to train, but the produced images are blurry.**  
![The evolution of GANs: VAE](/assets/images/gans_history_1.png "Evolution of GANs: VAE")  
_VAE generated images. [Source](https://github.com/WojciechMormul/vae)_

##### The paradigm shift:
The next obvious step was to come up with a way to generate sharper, more realistic images.

This is where **GANs** make their grand entrance. Proposed by Ian Goodfellow in 2014, they introduce a new mechanic into the mix: use two networks, generator, and discriminator, that compete with each other. The best way to grasp the intuition behind this idea is by thinking of the generator as a counterfeit money maker that tries to fool the bank clerk (the discriminator). In this arms race, the counterfeit maker receives feedback about which of their bills were marked as fake and adjusts the process accordingly to fix the known issues, while the bank clerk is shown both real and fake money and learns to distinguish between the two. As the generator gets better, so does the discriminator, and the training reaches its logical conclusion when the discriminator can no longer distinguish between real samples from training data and the fake images synthesized by the generator.

This approach is notoriously difficult to execute as one of the networks tends to dominate the other: either the generator fools the weak discriminator with a stupidly simple solution (usually showing nonsensical images that somehow hit just right so that the discriminator can’t learn anything useful from them) or the discriminator is way too strong and leaves the generator no chance to get good and catch up so that it can fool the discriminator at least a little bit.

Luckily, over the years researchers came up with a multitude of tricks and hacks to stabilize the training, and nowadays most common models work well out-of-the-box.

**TLDR: Old School GANs produce sharper images (still low resolution and with a ton of artifacts) but are somewhat difficult to train**  
![The evolution of GANs: GAN](/assets/images/gans_history_2.png "Evolution of GANs: GAN")  
_Samples from the original 2014 GAN paper. The rightmost column shows real images next to the fake generated samples._

##### All hail the king:
There was a lot of rapid improvement in the following years but the real breakthrough happened in 2018 with the introduction of StyleGAN and its next year follow-up StyleGAN2 that is still widely used today for face editing, cartoon/anime filters, and more.

The idea at the core of StyleGAN is simple: images from a single domain, for example, faces, all share the same basic structure - a nose, a mouth, two eyes, two years, you get it. What makes every image unique is the style of the face: the slight variations in the shape of the eyes, the hair color, the width of the nose, etc. To capture this idea StyleGAN restructured the generator to always start from a constant low-resolution tensor (think of this as a generic face blueprint) that is progressively molded into a unique high-resolution face according to a style vector from a learned latent space. Interestingly, the effect of the style vector in each layer of the generator is consistent across samples. Earlier layers control the pose and shape of the face, while the later layers control the finer details such as hair color, takin texture, etc. By changing the latent style vectors in each layer you can more or less controllably change the image, and this is how StyleGAN-based image editing essentially works. It is even possible to swap styles between images to mix them in fun ways.

**TLDR: StyleGAN achieves amazing high-resolution results for images that share the overall structure but not the style.**  
![The evolution of GANs: StyleGAN](/assets/images/gans_history_3.png "Evolution of GANs: StyleGAN")  
_Notice how the mixed images combine the appearance of the two source images differently depending on the layers, for which the styles are swapped_

##### The hypebeast:
As good as StyleGAN is at generating structured images, it sucks at generating images without a clear shared structure (landscapes, abstract paintings, animals in random poses, etc)

![The evolution of GANs: StyleGAN](/assets/images/gans_history_4.png "Evolution of GANs: StyleGAN")  
_Do these look like animals to you?_

And so the game was reinvented once again with the release of **VQGAN** (yep the other half of CLIP + VQGAN), a model that builds an image from a library of learned latent components. Let’s try to get the intuition for how it might do that. Imagine that you are shown a bunch of random images and asked to list a small number (say twenty) of lego pieces that can be used to build the objects from the pictures. You would likely try to select the most versatile pieces that can be combined in various ways to represent as many objects as possible. This is exactly what happens in the first stage of the VQGAN training - the model learns a library of the most versatile image components (they are just vectors though, not actual image pieces) that can combine in some way to reconstruct whatever image is shown to the model.

Alright, we can decompose an image into its components, now how might we go about generating new images? Now that we have a library of universal image components, a second model is trained just like a regular GAN with a discriminator that learns to distinguish real and fake images, but instead of random input noise, it uses some combination of vectors from the learned library.

In our lego example, this is equivalent to scattering all the available lego pieces on a table and starting by picking up one, then another, connecting them, picking the next piece, connecting, and so on until you built something that looks like it could be a real thing.

**TLDR: VQGAN is great for generating unstructured images but it lacks the properties of StyleGAN that makes it great for image editing**
![The evolution of GANs: VQGAN](/assets/images/gans_history_5.png "Evolution of GANs: VQGAN")  
_The generated images are incredibly diverse and consistent_

##### 🔗 Links:

- [Generate art from text with VQGAN + CLIP]({% post_url 2021-10-31-VQGAN-CLIP-tutorial %})
- [GAN quick start]({% post_url 2021-11-5-GANs-101-tutorial %})

***

##### 🔥 Dive deeper:
- [StyleGAN2-ADA Paper explained]({% post_url 2021-4-19-StyleGAN-2-Ada-explained %})
- [VQ-VAE Paper explained]({% post_url 2021-4-23-VQ-VAE-2-explained %})
- [VQGAN Paper explained]({% post_url 2021-6-1-VQGAN %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
