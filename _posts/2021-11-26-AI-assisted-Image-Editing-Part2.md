---
layout: post
title: "5B: How to edit images with GANs Part 2"
meta_title: "I GAN Explain: AI-assisted Image Editing Tutorial. Part 2. How to edit images without photoshop."
description: "I GAN Explain: AI-assisted Image Editing Tutorial. Part 2. How to edit images without photoshop."
date: 2021-11-26 19:00:00 -0000
categories: gan-inversion-image-editing-free-photoshop-alternative
---

#### I GAN Explain: AI-assisted Image Editing. Part 2. How to edit images without photoshop.

##### ⭐Tutorial difficulty: 🌕🌕🌑🌑🌑

***

###### Your digital avatar (Recap)
Let’s start with a quick refresher on Part 1 - creating a digital clone of yourself inside a GAN (StyleGAN-2 if we are specific). You might remember that we start with a pretrained generator that can synthesize a myriad of random faces with photorealistic quality and we aim to obtain a style code that the generator can use to recreate our own face as closely as possible without sacrificing the ability to edit the obtained image by manipulating the style code.  
We do this either iteratively - making a series of small adjustments to a style code (or a set of style codes that are all close to each other in the latent space) for each image that we want to ‘invert’ into the latent space of the generator. Or we can train an encoder network that is trained to predict the style code for any input image in a single step.  
At this point, let’s assume that you used one of the state-of-the-art style encoders (e4e, ReStyle, etc) on a close-up photo of your face and obtained a style code that corresponds to a generated copy of your face. You now need to manipulate the style code in some manner to change some aspects of the photo. Honestly, you can change pretty much whatever you can think of (although not all changes equally pleasing to the eye) only limited by your imagination.  
In its simplest form, this manipulation is performed by adding an offset to the starting vector, in effect, making a small step in the latent space in the direction of the target attribute.  

![editing directions](/assets/images/distilling_teaser.png "Editing Directions")

###### Asking for directions (in the latent space)
Here are some things we got to keep in mind to succeed with the task at hand: whatever changes we make to the style code, they must be local. Shifting the code too much will likely result in the synthesized photo losing any resemblance to your likeness. Recall that one of the amazing features of StyleGAN is the ability to pass different style vectors to each layer controlling different aspects of the generated face. We can use this to our advantage by restricting the style code changes only to the few layers that contribute to the target feature that we are editing, be that the shape of the mouth, hairstyle, or eye color.  
Obtaining the editing direction is far from trivial for a number of reasons. Most methods require pretrained classifiers for each attribute, moreover, editing directions are often entangled, meaning that changing one aspect of an image inadvertently causes changes in other aspects as well.  
One possible strategy is to sample many faces, filter and group them by target attributes using a pretrained classifier, take the average style code of each group (e.g. people with longest and shortest hair), and find the attribute’s editing direction by subtracting the mean vector of the group that does not have the specified attribute (e.g. short hair) from the mean vector of the group that does (e.g. long hair). And voila, moving along this direction vector will change the hair length of any generated face including your digital doppelganger. This is actually the same idea as the vector arithmetic in word embedding models (“king” - “man” + “woman” = “queen”).  
I also want to mention the approaches that use CLIP to edit images with text. They leverage CLIP’s ability to score images against a text query to find directions corresponding to find the latent space directions based on text, and let me tell you, it gets wild! Since CLIP replaces the per-attribute classifiers, it becomes possible to make insane edits that do not even need to make sense in the real world (ever wanted to know what you would look like as Santa with Rick Sanchez’s hair?).  

![Edit images with text](/assets/images/styleclip_teaser.gif "Edit images with text")

###### Knowledge distillation 
The final step in this face editing pipeline is often to train a knowledge distillation model. Typically, this is a lightweight (faster and smaller than the generator) image-to-image model that has just a single ‘hardcoded’ edit that it can perform. It learns from pairs of images generated using the editing direction vectors from the previous step. The intuition behind this design decision is that knowledge distillation is similar to using presets in Lightroom. Once you found something that works, you save it and apply it to images in a single click instead of tuning the parameters from scratch each time, and as a bonus: you do not need to keep the generator around.  

***

And that is it, thanks for reading!  
Now you know how FaceApp, Gradient and other popular face-editing apps work by following this general framework.  
Come back next week to learn about neural filters that turn photos into Van Gogh paintings.

##### 🔗 Continue learning:

- [How to edit images with GANs. Part 1]({% post_url 2021-10-19-AI-assisted-Image-Editing-Part1 %})  
- [GAN quick start]({% post_url 2021-11-5-GANs-101-tutorial %})  
- [The evolution of GANs]({% post_url 2021-11-12-GAN-architectures-overview %})

##### 🔥 Dive deeper:
- [Distilling StyleGAN-2 explained]({% post_url 2021-5-4-Distilling-StyleGAN2 %})  
- [Unsupervised Discovery of Editing Directions]({% post_url 2021-10-5-Unsupervised-Directions-Discovery-explained %})  
- [How to Find Advanced Editing Directions]({% post_url 2021-10-8-WarpedGANSpace-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
