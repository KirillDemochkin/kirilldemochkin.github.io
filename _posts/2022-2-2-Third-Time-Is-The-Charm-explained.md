---
layout: post
title: "79: StyleGAN3 Inversion & Editing"
meta_title: "StyleGAN3 Inversion & Editing"
description: "Third Time’s the Charm? Image and Video Editing with StyleGAN3 by Yuval Alaluf, Or Patashnik et al. explained in 5 minutes"
date: 2022-2-2 16:00:00 -0000
categories: alias-free-gan-stylegan3-inversion-video-editing
---

#### Third Time’s the Charm? Image and Video Editing with StyleGAN3 by Yuval Alaluf, Or Patashnik et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![Third Time’s the Charm](/assets/images/third_time_preview.gif "Third Time’s the Charm Teaser")

##### 🎯 At a glance:

Alias-free GAN more commonly known as StyleGAN3, the successor to the legendary StyleGAN2, came out last year, and … Well, and nothing really, despite the initial pique of interest and promising first results, StyleGAN3 did not set the world on fire, and the research community pretty quickly went back to the old but good StyleGAN2 for its well known latent space disentanglement and numerous other killer features, leaving its successor mostly in the shrinkwrap up on the bookshelf as an interesting, yet confusing toy.

Now, some 6 months later the team at the Tel-Aviv University, Hebrew University of Jerusalem, and Adobe Research finally released a comprehensive study of StyleGAN3’s applications in popular inversion and editing tasks, its pitfalls, and potential solutions, as well as highlights of the power of the Alias-free generator in tasks, where traditional image generators commonly underperform.

Let’s dive in, and learn, shall we?

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [Alias-free GAN]({% post_url 2021-6-25-AliasFreeGAN %})  
2) [ReStyle]({% post_url 2021-4-9-ReStyle-explained %})  
3) [StyleCLIP]({% post_url 2021-4-2-StyleCLIP-explained %})  
4) [PTI]({% post_url 2021-6-29-PTI %})  

***

##### 🚀 Motivation:

StyleGAN3 promised to solve numerous issues plaguing the StyleGAN 1 and 2 generators, namely texture-sticking, a nasty artifact that makes high-frequency textures like hair stay in one place, while the rest of the generated face moves around the image. Moreover, Alias-free GAN has a built-in explicit way to control the translation and rotation of generated faces in a way that is disentangled from the latent style vector that determines the semantic attributes of the synthesized faces. Yet, despite these improvements, StyleGAN3 is not a trivial drop-in replacement for StyleGAN2 in most tasks such as image and video inversion and editing.

***

##### 🔍 Main Ideas:

**1) StyleGAN3 Overview:**  
At this point stop and read the Alias-free digest if have not yet done so.  

Key differences from StyleGAN2: Fixed 16-layer generator, fourier features instead of the constant 4x4 input, the fourier features can be rotated and translated with four parameters predicted from the first style vector via a learned affine layer. The generator can be forced to output an aligned FFHQ-like image by setting the rotation and translation to 0, and the first style code to the average style code.

**2) Analysis:**  
At this point the paper goes into great detail about a series of experiments that I will cover in the next section. Aaaand, let’s get to it!

##### 📈 Experiment insights / Key takeaways:

**1) Rotation Control:**  
The rotation of the synthesized image is primarily controlled by the first two style codes. Note that the first two latent vectors also determine various other entangled attributes.  

**2) Disentanglement Analysis:**  
To compute attribute scores for generated images a pseudo-alignment procedure is used as mentioned earlier. Using the Style Space (outputs of the learned affine transforms at each of the generator’s layers) for editing StyleGAN3 images is preferred to both the noise and W spaces in terms of the DCI (disentanglement, completeness, informativeness) metrics. Unfortunately, using the popular W+ style codes with StyleGAN3 generates unnatural images.  

**3) Image Editing:**  
It is not obvious how to use an unaligned generator for image editing since the learned latent space directions are only available for an aligned generator, and attribute classifiers typically work best with aligned images. While using the pseudo-aligned images is one way to solve this issue, a better way is to generate all images with an aligned generator and apply user-defined rotation and translation to change the orientation of the image. Linear directions appear to be more entangled for the unaligned generator.  

Using the style space for editing images with StyleCLIP results in disentangled edits for both the aligned and unaligned generators, as opposed to edits performed in the noise and W spaces.  

**4) StyleGAN3 Inversion:**  
The authors leverage the insight that unaligned images can be edited with an aligned generator and train an encoder on aligned images only. At inference, they align the input image using an off-the-shelf facial landmarks detector and predict the translation and rotation between the unaligned input image and its pseudo-aligned variant. The aligned image is encoded, inverted, reconstructed into the unaligned version using the predicted translation and rotation parameters. The encoder is either the pSp or e4e version of ReStyle, with the later producing more editable reconstructions.  

The authors notice quick deterioration in the quality of StyleGAN3 generated images as they move awat from the W space towards W+, as it is not as well behaved as in StyleGAN2, hence the challenges in training inversion encoders for StyleGAN3.  

**5) Inverting and Editing Videos:**  
The pipeline for inverting and editing videos is as follows: All video frames are cropped and aligned before being inverted with a trained encoder. Additionally, the rotation and translation parameters are saved for each frame’s unaligned image. Optionallty, the edits are performed at this stage. The obtained vectors and input fourrier features are temporally smoothed to remove jittering between subsequent video frames.  

Next, Pivotal Tuning Inversion is performed on the original unaligned images to further improve the reconstructions for each frame.  

As if that was not impressive enough, the authors suggest a way to expand the field of view of videos, where a subject’s head moves out of the frame. They do this by predicting the offset transforms to fit the entire head into the frame, generating a second image with the missing parts of the head inside the frame, and combining the two images into a single one with an expanded FOV. This is not possible with StyleGAN2 as it is unable to handle unaligned images. Furthermore, coherent edits on the expanded images are possible with StyleGAN3 without boundary artifacts.  

***

##### 🖼️ Paper Poster:

![Third Time’s the Charm poster](/assets/images/third_time.jpg "Third Time’s the Charm Poster")

***

##### 🛠 Possible Improvements:

- Figure out why the latent space of StyleGAN3 is entangled, and how to make it more disentangled
- Do more experiments with non-facial domains
- Try to design encoder architectures around 1x1 convolutions and Fourier Features.  

##### ✏️My Notes:

- (5/5) Awesome, I love the wordplay around the number three! I usually take off a point here for not including a simple acronym to refer to the proposed method, but that does not make much sense in this paper  

- An interesting paper format, you don’t see this exploration-style research too often  
- Kinda sucks that there is not a definitive answer on whether StyleGAN3 is better than StyleGAN2 or not  
- If you look closely at the samples provided on the website, you can see a weird “bouncing” effect on some of the images, as if the texture is moving at different speeds at different coordinates, which makes some of the images appear to be made of Jell-O. I guess this could be a side effect of using 1x1 convolutions in the generator.  
- At the rate, this team is releasing papers they might as well just do an official takeover of Casual GAN Papers soon  
- “I am very intrigued, what the next couple of papers from this lab will be (besides the obvious fast video editing follow up), something in 3D maybe?” - well, this aged well in two weeks  

- What do you think about Third Time is the Charm? Leave a comment, and let’s discuss!  

##### 🔗 Links:

[Stitch it in Time arxiv](https://arxiv.org/pdf/2201.13433v1.pdf) / [Stitch it in Time Github](https://github.com/yuval-alaluf/stylegan3-editing)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Train a NeRF in Seconds - Instant Neural Graphics explained]({% post_url 2022-1-18-Instant-Neural-Graphics-Primitives-explained %})
- [Infinite Video Generation - StyleGAN-V explained]({% post_url 2022-1-11-StyleGAN-V-explained %})
- [Casual GAN Paper Awards - Best Papers of 2021]({% post_url 2022-1-28-Casual-GAN-Papers-Awards %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
