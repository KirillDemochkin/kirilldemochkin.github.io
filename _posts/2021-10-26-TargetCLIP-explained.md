---
layout: post
title: "66: TargetCLIP"
meta_title: "TargetCLIP"
description: "IMAGE-BASED CLIP-GUIDED ESSENCE TRANSFER by Hila Chefer et al. explained in 5 minutes"
date: 2021-10-26 19:00:00 -0000
categories: CLIP_image_to_image_style_transfer_essence_transfer
---

#### IMAGE-BASED CLIP-GUIDED ESSENCE TRANSFER by Hila Chefer et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![TargetCLIP teaser](/assets/images/targetclip_teaser.jpg "TargetCLIP Teaser")

##### 🎯 At a glance:

There has recently been a lot of interest concerning a new generation of style-transfer models. These work on a higher level of abstraction and rather than focusing on transferring colors and textures from one image to another, they combine the conceptual “style” of one image and the objective “content” of another in an entirely new image altogether. A recent paper by Hila Chefer and the team at Tel Aviv University does just that! The authors propose TargetCLIP, a blending operator that combines the powerful StyleGAN2 generator with a semantic network CLIP to achieve a more natural blending than with each model separately. On a practical level, this idea is implemented with two losses - one that ensures the output image is similar to the input in the CLIP space, the other - that the shifts in the CLIP space are linked to shifts in the StyleGAN space.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [CLIP]({% post_url 2021-9-14-CLIP-explained %})  
2) [StyleGAN](https://github.com/NVlabs/stylegan2)

***

##### 🚀 Motivation:

Unlike other existing methods, TargetCLIP does not use fixed losses based on the Gram matrix or Adaptive Instance Normalization commonly found in style transfer papers. Instead, style is guided by a pretrained CLIP model, intuitively this works because CLIP has an understanding of “objects” inside its latent space. In other words, objects and visual concepts with similar text descriptions are grouped together in the latent space. Hence, TargetCLIP is able to transfer higher-level style concepts, for example, given an image of the Joker, the model captures Joker’s green hair, pale face, and the iconic red smile instead of simply changing the color scheme or textures of the source image.

***

##### 🔍 Main Ideas:

**1) Overview:**  
At the core of the proposed method lies the idea of linearity, meaning that changes to the style vector in the generator’s latent space lead to a constant change in the CLIP space (global semantic direction). In other words, TargetCLIP maximizes similarity in the semantic (CLIP) space between the target and the generated images.

**2) The losses:**  
The transfer loss is implemented as a cosine distance between the CLIP encodings of the target (style) and source (content) images. The consistency loss is defined as the cosine distance in the CLIP space between the edits in the generator’s latent space across the source images. In other words, the semantic edits in the StyleGAN2 latent space should be consistent in the CLIP space for the source images.

**3) The process:**  
For each target image, 6 random source images are considered, the edit direction is either initialized with the target image latent vector (inverted with a StyleGAN2 encoder) or randomly initialized (for out of domain target images). Subsequently, the edit direction is optimized iteratively with a gradient descent optimizer with weight decay.

##### 📈 Experiment insights / Key takeaways:

- Baselines: CLIP blending, StyleGAN latent vector blending
- single-representation blending methods either change the identity or change unrelated semantic attributes, such as the background of the image, TargetCLIP is able to preserve the identity of the person in the source image and adopt only the essential semantic features from the target image
- Most results are qualitative in nature, check out the images!

***

##### 🖼️ Paper Poster:

![TargetCLIP poster](/assets/images/targetclip.jpg "TargetCLIP Paper Poster")

***

##### 🛠 Possible Improvements:

- Do the direction prediction in a single feed-forward pass instead of the iterative updates

##### ✏️My Notes:

- (3/5) TargetCLIP misses the target (pardon the pun) in the name department for me. I just feel that there is untapped potential for puns around the word CLIP.
- Interesting trend developing right before our eyes: this is already the second recent paper that I read that aims to combine content and style from two different sources into a single image
- Cool, this is basically StyleCLIP with images instead of prompts!
- I think this is the second paper that I have seen that does not use CLIP with text at all (the other is DietNeRF)
- I wonder how well this method would work on videos, neural masks seem like the perfect application of the idea.
- Share your thoughts on TargetCLIP in the comments!

##### 🔗 Links:
[TargetCLIP arxiv](https://arxiv.org/pdf/2110.12427.pdf) / [TargetCLIP github](https://github.com/hila-chefer/TargetCLIP) / [TargetCLIP Demo](https://colab.research.google.com/github/hila-chefer/TargetCLIP/blob/main/TargetCLIP_CLIP_guided_image_essence_transfer.ipynb)

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [SOTA High Resolution Inpainting - LaMa explained]({% post_url 2021-10-16-LaMa-explained %})  
- [Sensorium = disentangled content + style image-to-image translation]({% post_url 2021-10-19-Sensorium-explained %})  
- [CIPS-3D]({% post_url 2021-10-22-CIPS-3D-explained %})  

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
