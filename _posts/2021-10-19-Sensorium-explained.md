---
layout: post
title: "64: Sensorium"
meta_title: "Sensorium"
description: "Harnessing the Conditioning Sensorium for Improved Image Translation by Nederhood et al. explained in 5 minutes"
date: 2021-10-19 19:00:00 -0000
categories: multimodal-style-conditioned-image-to-image-domain-translation
---

#### Harnessing the Conditioning Sensorium for Improved Image Translation by Nederhood et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![Sensorium teaser](/assets/images/sensorium_teaser.png "Sensorium Teaser")

##### 🎯 At a glance:

Image to image translation appears more or less “solved” on the surface, yet there are still several important challenges to overcome. One such challenge is the ambiguity in multi-modal, reference-guided image-to-image domain translation. Believing that the choice of what to preserve as the “content” of the input image, and “style” should be transferred from the target image during domain translation depends heavily on the task at hand, Cooper Nederhood and his colleagues propose Sensorium, a new model that conditions its output on the information from various off-the-shelf pretrained models depending on the task. Sensorium enables higher quality domain translation for more complex scenes.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [SPADE](https://github.com/NVlabs/SPADE)

***

##### 🚀 Motivation:

Existing domain transfer approaches excel in simple domains with clear structure (e.g. faces), yet they deteriorate as the domain complexity and the gap between domains grow. This is especially true if a 1-to-1 relationship does not exist for the input and target domains. Rather than doing conditional generation from scratch, the authors pose the task as image reconstruction from conditional information guided by a bottle-necked style representation. This approach implicitly disentangles content from style, one of the subtasks that typically trip up conventional models, which enables explicit control over the “content” of the image and boosts performance on more difficult datasets that require changing the structure of the image along with its texture.

***

##### 🔍 Main Ideas:

**1) Overview:**  
There are three trainable components of the Sensorium model: the domain invariant content network that maps the spatial conditioning features to a shared latent space, the domain-specific style network that learns to map an image to a global style vector used for weight-demodulation conditioning, and a generator network that synthesized the final image based on the two different conditions.

**2) Training Objective:**  
The model is trained with three losses: L1 feature reconstruction loss for VGG19 and Discriminator features and the hinge-GAN adversarial loss.

**3) Architecture:**  
The content encoder is a ResNet-based encoder that outputs feature maps at different scales, forming a U-Net structure with skip-connections to the Generator. The style encoder is also a ResNet-based encoder with a global average pooling layer that outputs a global style vector. The generator is based on SPADE with the added condition of content as well as style. Moreover, a fusion module is introduced that concatenates the spacially-replicated style vector with the highest resolution content feature map from the pyramid and maps it to the SPADE parameters with a small convnet unique for each layer. The multi-resolution patch-based discriminator from SPADE is used.

##### 📈 Experiment insights / Key takeaways:

- Datasets: CelebA-HQ, FFHQ-Wild, ClassicTV, Flickr Seasons, WikiArt Landscapes, auto-generated depth, human pose, segmentations, facial keypoints for all datasets
- Baselines: DS-Map (Wins on Male to Female task), StarGAN2 (wins on Female to Male task), SPADE, CoCosNet, Sensorium wins on the rest of the tasks in terms of FID for random combinations of style and content.

***

##### 🖼️ Paper Poster:

![Sensorium poster](/assets/images/sensorium.jpg "Sensorium Paper Poster")

***

##### 🛠 Possible Improvements:

- Improve Style Encoder via implicit style disentanglement
- Apply Sensorium to video translation
- Deeper exploration of the relationship between conditioning modalities and corresponding ‘null-space’ controlled by style
- Improve identity preservation

##### ✏️My Notes:

- (4/5) Sensorium sounds very strong, but I can’t really see what it has to do with the model.
- I was hoping for more interesting modalities such as text (CLIP, anyone?), and video/audio :(
- Surprisingly, I could not find the code for this paper, if you have seen it, please DM me, and I will add it to this post
- You could probably incorporate a W+ latent space into the style encoder with offsets akin to e4e
- In a way, this model can be thought of as the “Pretrained Model Prior” as it does not learn the prior on the target image structure from scratch and lifts it straight from the output extracted by a different pretrained model.
- I feel that this model shares many ideas with FOMM (which I have been meaning to cover for a while): you got the driving image (content) and the target image that gets warped (style)
- I feel like this model would benefit from some sort of an implicit NeRF-like (Or FLAME for faces) 3D module since intuitively it tries to produce domain-free spatial geometry and paint it with domain-specific textures
- Share your thoughts on Sensorium in the comments!

##### 🔗 Links:
[Sensorium arxiv](https://arxiv.org/abs/2110.06443) / Sensorium github (Could not find the code. Email me if you know where it is)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [SOTA High Resolution Inpainting - LaMa explained]({% post_url 2021-10-16-LaMa-explained %})
- [WarpedGANSpace - Unsupervised editing directions followup]({% post_url 2021-10-8-WarpedGANSpace-explained %})
- [StyleGAN + NeRF = StyleNeRF]({% post_url 2021-10-12-StyleNeRF-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
