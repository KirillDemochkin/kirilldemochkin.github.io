---
layout: post
title: "59: ISF-GAN"
meta_title: "ISF-GAN"
description: "ISF-GAN: An Implicit Style Function for High-Resolution Image-to-Image Translation by Yahui Liu et al. explained in 5 minutes."
date: 2021-10-1 16:00:00 -0000
categories: unsupervised_latent_space_exploration_image_to_image_GAN_editing
---

#### ISF-GAN: An Implicit Style Function for High-Resolution Image-to-Image Translation by Yahui Liu et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![ISF-GAN teaser](/assets/images/isf-gan_teaser.png "ISF-GAN Teaser")

##### 🎯 At a glance:

I often find myself wishing I knew how to edit images in photoshop but I remember that I already have a full-time job without attempting to learn photoshop. This is where ISF-GAN by Yahui Liu et al. comes in. This new model performs cost-effective multi-modal unsupervised image-to-image translations at high resolution using pre-trained unconditional GANs. ISF-GAN does this by modeling the latent style vector update with an MLP conditioned on a random vector and an attribute code.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [StyleGAN2](https://github.com/NVlabs/stylegan2)  
2) [StyleFlow](https://github.com/RameenAbdal/StyleFlow) (optional)  

***

##### 🚀 Motivation:

Training task-specific GANs in high resolutions is nontrivial and expensive, hence lately the focus has shifted to adapting pretrained unconditional generators with a proven track record (StyleGAN) to various editing and image-to-image tasks. Most methods follow the general pipeline of starting with a latent vector in W+, manipulating it in some way, and passing the updated style code through the pretrained generator to obtain the edited (upscaled, domain-shifted, etc) image. Unfortunately, existing approaches are limited to editing one attribute at a time or change the identity of the person on the photo while applying edits. Moreover, most such methods do not have an obvious way to produce multi-modal results for a single input image. ISF-GAN, on the other hand, uses Adaptive Layer Normalization (AdaLN) in its ISF module for conditional multi-modal latent code editing.

***

##### 🔍 Main Ideas:

**1) Overview:**  
The goal of this model is to output realistic multi-modal editing results for input images without changing their contents. The proposed way to achieve this goal is with an MLP that transforms the input style vector W+ conditioned on a random vector z and a task-specific vector d.

**2) Learning the ISF:**  
The ISF MLP is trained in an adversarial fashion with a discriminator that classifies the images based on the target attribute d in addition to predicting whether the output image is real or fake. Additionally, the LPIPS, L2, and cycle consistency losses ensure that the content is preserved between images, while a diversity-sensitive loss encourages multi-modal outputs.

**3) Injecting the domain and multi-modality:**  
The authors just switch instance norm for layer norm in the AdaIN and call in AdaLN. The intuition is that channels are correlated in W+ latent codes (the reasoning looks a bit iffy tbh). The complete ISF module is an AdaLN layer between two MLPs and does the following: the domain vector d is concatenated to the random noise vector, goes through the first MLP, and becomes the condition in the AdaLN layer that transforms the input W+ style code before passing it through the second MLP to obtain the final code.

##### 📈 Experiment insights / Key takeaways:

- ISF-GAN favorably compares to InterFaceGAN, StyleFlow, and StarGAN on a custom synthetic labeled dataset as well as the public test set from StyleFlow in terms of FID, LPIPS, Accuracy, and Arcface (used as a metric, not a loss, huh).
- The edited attributes are gender, smile, age, and eyeglasses.
- Mostly standard self-congratulatory GAN results are reported, nothing out of ordinary here.
- From ablations: content loss helps with interpolation and identity preservation
- replacing AdaLN with AdaIN decreases FID and FRS

***

##### 🖼️ Paper Poster:

![ISF-GAN paper poster](/assets/images/isf-gan.jpg "ISF-GAN Paper Poster")

***

##### 🛠 Possible Improvements:

- Authors didn’t really include any
- I would suggest thinking of a way to incorporate the encoder in an end-to-end fashion (or think of a way to skip it altogether)

##### ✏️My Notes:

- (3/5) so many utilitarian names lately :(. Although “Implicit Style Function” is clear and concise.
- Looks like a good (and much simpler IMHO) alternative to StyleFlow
- I wonder why there aren’t any comparisons to StyleCLIP since the ideas for the latent mapper are so similar
- I still think the main crux of all these amazing editing approaches is the inability to process real images off-the-shelf without an external encoder, in other words, your editing is only as good as your encoder
- I am surprised that the arcface loss is not used here at all for identity preservation
- Do you have any questions left about ISF-GAN? Let’s discuss in the comments!

##### 🔗 Links:
[ISF-GAN arxiv](https://arxiv.org/pdf/2109.12492.pdf) / [ISF-GAN github](https://github.com/yhlleo/stylegan-mmuit) <- Available later?

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [SOTA Single Video Generative Model - VGPNN]({% post_url 2021-9-27-VGPNN-explained %})
- [Instance Conditioned GAN - Arbitrary similar image generation]({% post_url 2021-9-24-Instance-Conditioned-GAN-explained %})
- [GSN - Generative Scene Networks (VR NeRF)]({% post_url 2021-9-21-GSN-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
