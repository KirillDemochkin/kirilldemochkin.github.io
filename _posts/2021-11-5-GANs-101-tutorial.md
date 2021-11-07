---
layout: post
title: "2B: GANs 101 Tutorial"
meta_title: "GANs 101 Tutorial"
description: "I GAN Explain: So you want to learn GANs? A generative AI primer by Casual GAN Papers"
date: 2021-10-31 19:00:00 -0000
categories: howto-GANs-generative-models-introduction
---

#### I GAN Explain: So you want to learn GANs? A generative AI primer by Casual GAN Papers

##### ⭐Tutorial difficulty: 🌕🌑🌑🌑🌑

***

![GANs 101 Teaser](/assets/images/gans101_teaser.jpg "GANs 101 Teaser")

**So you want to learn GANs!**

If you are reading this, I assume you already have some interest in GANs (Generative Adversarial Network), but just in case let’s quickly go over some reasons why it is the perfect time to get into generative models right now.

First, of all, they are making the world a better place everywhere: Google’s magic eraser, Snapchat’s anime filter, Avatarify’s deepfakes, Photoshop’s neural filters, and this is just the beginning! Did anyone hear about the Metaverse? Btw, not all generative models are GANs, but more on that later.

Second, generative models have the coolest looking results, see [this gif](https://twitter.com/sportsracer48/status/1456420539065634820) if you don’t believe me.

Last but not least, GANs are more accessible than ever. Most modern models come with a colab demo that often requires writing zero code to get started, and now huggingface spaces provide a clean UI with drag’n’drop functionality that takes less than a minute to start.

**Alright now that we have established that GANs are indeed the crown achievement of humanity, what the heck is required to use them?**  
Turns out it is not that much really. On the highest level, you will only need access to the internet and a lot of patience (these things typically take a couple of while to do anything worthwhile and usually not on the first attempt). Looking a little deeper you will find that understanding the mechanics of a few popular architectures comes in handy when you start building your own pipelines. Oh, did I mention that messing around with generative AI is a lot like playing legos? You essentially have a ton of building blocks for various effects and modalities that stack together quite nicely to form an end-to-end pipeline for creating generative art, avatars, VR scenes, etc.

**We know the “why” and the “what”, let’s now learn the basics of the “how”!**  
Let’s work through the intuition on how to generate images, for example, faces. The most intuitive way to think about image synthesis is to imagine a bunch of mugshots (pretty much an infinite amount) laid out in a grid on a table in front of you. They are pretty small, so you can’t see what face is on the photo until you pick it up and take a closer look, luckily you can access any picture on the table by its coordinates. This table with photos is called the “latent space” of a generative model and picking one up from specific coordinates is referred to as “sampling the model”.

Now, ideally, we would want these photographs to not be laid out randomly, and instead be organized in a sensible manner: similar-looking faces are close to one another. This is useful since it allows us to explore the latent space to discover editing directions (the basis of GAN-based photo editing) - how to find the coordinates of the mugshot of the same dude like the one we just picked up from the table but with a beard or glasses or a tattoo of a teardrop under his left eye. Although the last one probably wouldn’t work well as these directions work best for more common traits. Some other useful applications of a well-organized (semantically consistent) latent space are interpolation (smoothly blending two faces, POV - you want to know what your children with a hot celeb look like), inversion (not the Tenet kind) for finding the latent space coordinates of a random image of a stranger you just took (probably not a good idea, btw).

Now is a good time to check that the words “latent space” and “sampling” don’t freak you out. Assuming, you are ready, let’s do a fast track through the fascinating line of discoveries that led to a flood of [GAN-generated LinkedIn profiles](https://www.linkedin.com/in/nikola-milutinov-806491214/) (no kidding). To train a generative model means learning the structure of a latent space (table with mugshots) that you can then sample from (pick up pictures from specific coordinates).

**I was going to write a short history of GANs here, but it got so long (oh, the irony) that it deserves its own post. So instead, you get a three-sentence preview.**  
At first synthesized images were ugly, low res, and blurry, but then came GANs and changed the game by introducing the adversarial mechanic that greatly improved the realism of the synthesized images. The pinnacle of the evolution of GANs is arguably the StyleGAN model that introduced the style-conditioning mechanic that allowed granular control over the generated images. Unfortunately, StyleGAN works best for VERY structured images (faces) and can’t even for randomly posed images of cats and horses (nightmare-inducing, don’t google), landscapes, etc, and that is why VQGAN exists, which essentially builds an image from a library of image components not too different from how you might make a post-modern meme.

**You are probably thinking “Gee Kirill that’s a lot of words, can I do something cool already?”**  
No worries, as a wrap up for this quick “GAN 101” guide I listed a few demos with pretty results to try out next...

##### 🔗 Links:

- [Generate art from text with VQGAN + CLIP](https://www.casualganpapers.com/howto-clip-vqgan-text-guided-image-generation-explained/VQGAN-CLIP-tutorial.html) (requires a keyboard)  
- [Edit your photos with StyleCLIP](https://replicate.ai/orpatashnik/styleclip) (requires a selfie)  
- [Upscale low-res images with Real-ESRGAN](https://huggingface.co/spaces/akhaliq/Real-ESRGAN) (requires a crappy low-resolution image)  
- [Extra Credit: Train StyleGAN-2 ADA on your own data](https://github.com/NVlabs/stylegan2-ada-pytorch) (requires a small dataset & GPU)  

***

##### 🔥 Dive deeper:
- [VAE paper](https://arxiv.org/pdf/1312.6114.pdf)
- [OG GAN paper by Ian Goodfellow](https://arxiv.org/pdf/1406.2661.pdf)

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
