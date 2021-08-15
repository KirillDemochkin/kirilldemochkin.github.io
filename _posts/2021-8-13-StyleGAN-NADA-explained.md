---
layout: post
title:  "45: StyleGAN-NADA"
meta_title: "StyleGAN-NADA"
description:  "StyleGAN-NADA: CLIP-Guided Domain Adaptation of Image Generators by Rinon Gal et al. explained in 5 minutes."
date: 2021-8-10 22:00:00 -0000
categories: text-guided-CLIP-GAN-domain-adaptation
---

#### StyleGAN-NADA: CLIP-Guided Domain Adaptation of Image Generators by Rinon Gal et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![StyleGAN-NADA Samples](/assets/images/stylegannada_teaser.jpg "StyleGAN-NADA teaser")

##### 🎯 At a glance:

How insane does it sound to describe a GAN with text (e.g. Human -> Werewolf) and get a complete generator that synthesizes images corresponding to the provided text query in any domain?! Rinon Gal and colleagues leverage the semantic power of CLIP's text-image latent space to shift a pretrained generator to a new domain. All it takes is a natural text prompts and a few minutes of training. The domains that StyleGAN-NADA covers are outright bizzare (and creepily specific) - Fernando Botero Painting, Dog → Nicolas Cage (WTF 😂), and more.

##### ⌛️ Prerequisites:

**(Check these out if you have extra time):**  
1)[StyleCLIP](https://t.me/casual_gan/18)

***

##### 🚀 Motivation:

Usually it is hard (or outright impossible) to obtain a large number of images from a specific domain required to train a GAN. One can leverage the information learned by Vision-Language models such as CLIP, yet applying these models to manipulate pretrained generators to synthesize out-of-domain images is far from trivial. The authors propose to use dual generators and an adaptive layer selection procedure to increase training stability. Unlike prior works StyleGAN-NADA works in zero-shot manner and automatically selects a subset of layers to update at each iteration.

***

##### 🔍 Main Ideas:

**1) Network Architecture:**  
The model consists of two pretrained StyleGAN2 generators with a shared mapping network (i.e. same shared space). The goal is to change the domain of one of these generators with a CLIP-based loss, and a layer-freezing scheme that adaptively selects which layers to update at each iteration.

**2) CLIP-based Guidance:**  
The most intuitive CLIP loss (global) that minimizes the CLIP-space cosine distance between the generated images and the target text either collapses to a single image or fools CLIP by adding per-pixel noise to the images. A more advanced type of loss is the directional variant that seeks to align the direction of CLIP embeddings between images from two domains to the CLIP direction of the corresponding text queries. A third variant is also considered - embedding-norm that uses a regularized version of StyleCLIP's latent mapper is used to reduce the number of semantic artifacts on synthesized images.

**3) Layer-Freezing:**  
Intuitively some layers of the generator are more important for specific domains than other, hence at each iterations a set of W+ vectors is generated (a separate style vector for each layer in the generator), and a number of StyleCLIP global optimization steps are performed to measure, which layers changed the most. Only those layers are updated, and all other layers are frozen for that iteration.

**4) Latent-Mapper:**  
For some domain (e.g. cats to dogs) the resulting generator can output both cats, and dogs (and all things in-between), therefore a StyleCLIP latent mapper can be learned to map all latent codes to the dog region of the latent space.
   
##### 📈 Experiment insights / Key takeaways:

- The images are must see!
- GAN editing methods can be used with StyleGAN-NADA
- Latent editing directions in the fine-tuned generator is aligned with the original generator's latent space

***

##### 🖼️ Paper Poster:

![StyleGAN-NADA paper poster](/assets/images/stylegananada.png "StyleGAN-NADA Paper Poster")

***

##### ✏️My Notes:

- (4/5) for whatever reason it cracks me up every time I see StyleGAN-NADA. I really want to see a Multiverse themed model name for a domain adaptation paper.
- Huge shoutout to the team at Tel-Aviv university, and NVIDA. Their papers are some of my favorites to cover, especially the ones about GANs+CLIP, imho it is one of the most interesting research directions for GAN based editing

##### 🔗 Links:
[StyleGAN-NADA arxiv](https://arxiv.org/pdf/2108.00946.pdf) / [StyleGAN-NADA github](https://github.com/rinongal/StyleGAN-nada)

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
