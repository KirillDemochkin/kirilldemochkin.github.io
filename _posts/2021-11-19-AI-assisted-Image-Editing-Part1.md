---
layout: post
title: "4B: How to edit images with GANs Part 1"
meta_title: "I GAN Explain: AI-assisted Image Editing. Part 1. Your metaverse avatar."
description: "I GAN Explain: AI-assisted Image Editing. Part 1. Your metaverse avatar."
date: 2021-11-19 19:00:00 -0000
categories: gan-inversion-image-editing-metaverse-avatar
---

#### I GAN Explain: AI-assisted Image Editing. Part 1. Your Digital Metaverse Avatar.

##### ⭐Tutorial difficulty: 🌕🌕🌑🌑🌑

![Metaverse Avatar](/assets/images/metaverse_avatar.jpg "Metaverse Avatar")

***

##### Overview:  
Let’s face it - Photoshop is hard. Learning it can sometimes feel like doing work after work. Luckily, modern neural networks are pretty good at doing all sorts of image enhancements automatically, from simple things like portrait mode found in almost all smartphones today to the much more complicated 3d-aware image relighting model in Google’s latest Pixel phones.  

Today, however, we will focus on the intuition behind the sort of GAN-based image editing found in the popular FaceApp, Gradient, and Snapchat apps. Specifically, this tutorial will cover the intuition behind creating your digital avatar that can be manipulated and transformed in a myriad of ways.

##### Mirror, mirror on the wall:  
Hopefully, you read the previous posts in the series and have a basic idea about GANs. If not, consider first reading posts [2B]({% post_url 2021-11-5-GANs-101-tutorial %}) and [3B]({% post_url 2021-11-12-GAN-architectures-overview %}), this post will make much more sense!  

Alright, let’s assume that you want to give yourself a lush beard for that rugged photo session, or make yourself look really old and grey-haired in an image, or see what you would look like as the opposite gender. How might you do this? Let’s remember that StyleGAN is scarily good at generating all kinds of photorealistic faces (men and women, young and old, beards, bald people, you name it) and that it uses a style code to control the combination of attributes that appear in each generated image.  

If StyleGAN can generate such a large variety of faces from these latent codes, could it be possible to find a random code that encodes the parameters of your own face?! Indeed, you can use any one of the many ways to invert an image into the latent space of StyleGAN. Whereas normally the generator produces an image from a style code, inverting an image is the process of discovering a style code that best matches the image that we want the generator to output (i.e. the photo of you). Once you have a style code that produces your input image when plugged into StyleGAN you can manipulate the code to selectively change certain attributes on the image  

##### Meet your evil clone:  
Turns out that this is a far more challenging task than it might seem at first. Oftentimes the reconstructed images (outputs of the generator from the inverted code) fall into an uncanny valley, where the person on the photo is not your exact match but the likeness is high enough to make you think you are seeing your evil twin from the darkest timeline. I did say that StyleGAN is REALLY good, so why does this distortion happen then? Turns out there exists a tradeoff between how well you can reconstruct an image and how well you are able to edit it by slightly changing the obtained style code.  

When StyleGAN is trained it learns a layout of codes that are meaningfully grouped together but do not necessarily cover all the infinite possible faces that could be generated. As long as your style code is somewhere inside this learned grouping you will have no problem manipulating it to alter your appearance on an image, however it sort of breaks apart the further you move away from this well-defined space of nicely arranged style codes. Here is where the tradeoff happens: making the reconstructed image look more like yourself, causes the inverted style code to move further away from the “nice” learned latent space. The good news is that the most popular StyleGAN Inversion models do a good-enough job at balancing the reconstruction quality and editability of your inverted images.  

##### See you in the Metaverse:  
Now that you have access to the style code that you can generate your digital avatar from, it becomes possible to do everything from the face edits that we discussed earlier to anime and cartoonization effects that you see flooding the social networks every now and then. As for how it is done, stay tuned for the next week’s “I GAN Explain” post, where we will cover how editing directions are discovered, how Disney princesses help create a cartoon version of yourself, and more!  

***

##### 🔗 Continue learning:

- [Generate art from text with VQGAN + CLIP]({% post_url 2021-10-31-VQGAN-CLIP-tutorial %})
- [GAN quick start]({% post_url 2021-11-5-GANs-101-tutorial %})
- [The evolution of GANs]({% post_url 2021-11-12-GAN-architectures-overview %})

##### 🔥 Dive deeper:
- [Pixel2Style2Pixel (pSp) explained](https://t.me/casual_gan/16)
- [Encoders for editing (e4e) explained]({% post_url 2021-4-13-e4e-explained %})
- [ReStyle explained]({% post_url 2021-4-9-ReStyle-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
