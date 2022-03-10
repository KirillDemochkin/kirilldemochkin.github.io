---
layout: post
title: "83: CLIP-GEN"
meta_title: "CLIP-GEN"
description: "CLIP-GEN: Language-Free Training of a Text-to-Image Generator with CLIP by Zihao Wang et al. explained in 5 minutes"
date: 2022-3-9 16:00:00 -0000
categories: fast-VQGAN-CLIP-text-to-image-generation
---

#### CLIP-GEN: Language-Free Training of a Text-to-Image Generator with CLIP by Zihao Wang et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![CLIP-GEN Model](/assets/images/clipgen_preview.png "CLIP-GEN Teaser")

##### 🎯 At a glance:

Text-to-image generation models have been in the spotlight since last year, with the VQGAN+CLIP combo garnering perhaps the most attention from the generative art community. Zihao Wang and the team at ByteDance present a clever twist on that idea. Instead of doing iterative optimization, the authors leverage CLIP’s shared text-image latent space to generate an image from text with a VQGAN decoder guided by CLIP in just a single step! The resulting images are diverse and on par with the SOTA text-to-image generators such as DALL-e and CogView.

As for the details, let’s dive in, shall we?

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1. [VQGAN]({% post_url 2021-6-1-VQGAN %})  
2. [CLIP]({% post_url 2021-9-14-CLIP-explained %})  

***

##### 🚀 Motivation:

As demonstrated on twitter by some very smart people, the generative capabilities of VQGAN+CLIP are limited only by the artist’s imagination. That, and the fact that the iterative update process is very slow, which is exactly the problem that the authors of CLIP-GEN attempt to solve. Their pitch is simple: encode a text query with CLIP, turn the encoded feature vector into a series of latent codes from a pretrained VQGAN codebook, and decode these tokens into an image with a pretrained generator (VGQAN decoder). There are two main caveats with this approach: how to map CLIP embeddings to VQGAN tokens? Where to find a big enough text-image dataset to learn this mapping?

Luckily, the first can be easily solved with a transformer model that is well known for its sequence-modeling ability, and the second - with CLIP’s shared latent space. Meaning that the entire pipeline is trained as an autoencoder using the image encoder from CLIP, and teaching the transformer to map the image embeddings to VQGAN tokens. Meanwhile, during inference, the input to CLIP is switched to text, yet from the perspective of the latent transformer nothing changes, since the text embeddings share the same latent space as the image embeddings. What an elegant and clever idea, very impressive.

***

##### 🔍 Main Ideas:

This is a bit strange, because the “Approach” section of the paper basically summarizes CLIP and VQGAN papers, both of which I already covered, hence I will leave just a single point here, explaining the novelty, and ask you to refresh the ideas from the “Required Reading” section above.

The authors for some reason decided to train the VQGAN from scratch, to which they refer as “the first stage”. Whereas the conditional transformer and the pretrained CLIP model are introduces in “the second stage”. The conditional transformer learns from the sum of the rmaximum likelihood loss on the predicted VQGAN tokens, and the embedding reconstruction loss. The second loss is somewhat unclear from the text, but from what I understand it is some sort of a log-distance between the CLIP embedding of the input image and the CLIP embedding of the generated image. Sorry, I honestly have no idea, what “log s of (thing1, thing2)” could mean, and why they didn’t just use an MSE loss like everybody else.

##### 📈 Experiment insights / Key takeaways:

- Baselines: DF-GAN, DM-GAN, AttnGAN, CogView, VQGAN-CLIP, BigGAN-CLIP  
- Datasets: MS-COCO, ImageNet  

- Not sure, what is different between FID-0, FID-1, and FID-2, but CLIP-GEN beats all other baselines in terms of FID-0, and FID-1 on MS-COCO, and in terms of FID on ImageNet  
- CLIP-GEN captures semantic concepts from text but fails to understand numeric concepts  
- CLIP-GEN can generalize to various drawing styles without any augmentations during training  

***

##### 🖼️ Paper Poster:

![CLIP-GEN poster](/assets/images/clipgen.jpg "CLIP-GEN Poster")

***

##### 🛠 Possible Improvements:

- Nothing from the authors, unfortunately.

##### ✏️My Notes:

- (Naming: 3/5) - Not bad, but not memorable either.  
- (Paper UX: 3.5/5) - The figures are mostly well designed, even if the two architecture diagrams are redundant. I could have used a stronger pitch in the abstract introduction. The setup lacks the punch, and reads more like a “we did this fun thing, and we are surprised it even works at all, but check it out anyway!”. I do appreciate the clear contribution statement, however. The required reading is summarized concisely, while not comprehensive, it is enough to remind oneself as to what the VQGAN and CLIP papers are about. The amount of different variables, on the other hand, is overwhelming. I have to hand it to the authors for trying to unify the naming across various papers, but there is a lower case “e” that stands for the embedddings of CLIP, and a capital case “E” that represents the encoder in VQGAN. Overall, you gotta think to keep track of everything, and that breaks the first rule of good UX - don’t make people think. Tables are well formatted and easy to follow. Overall, the paper reads smoothly without confusing wording and strange word choice.  

- What an awesome idea to use CLIP instead of a paired dataset, I think using pretrained cross-modal models for tasks, where paired data is hard to come by could become a big trend.  
- As a fan of deep learning, I am always happy to see these “fun” ideas that make you go “what, how has nobody tried this before”.  
- I wish the authors included some more fun and artistic samples in addition to the photorealistic images.  

##### 🔗 Links:

[CLIP-GEN arxiv](https://arxiv.org/pdf/2203.00386v1.pdf) / CLIP-GEN GitHub (Not released at the time of writing)

##### 💸 If you enjoy my posts, consider donating ETH to support Casual GAN Papers:  
**0x7c2a650437f58664BED719D680F2BE120bE5623b**

***

##### 🔥 Read More Popular AI Paper Summaries:
- [Improved VQGAN - MaskGIT explained]({% post_url 2022-2-18-MaskGIT-explained %})
- [How to geenrate videos from static images - FILM explained]({% post_url 2022-2-26-FILM-explained %})
- [Best Facial Animation Tool - First Order Motion Model]({% post_url 2022-2-10-First-Order-Motion-Model-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
