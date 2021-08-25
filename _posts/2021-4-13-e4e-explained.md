---
layout: post
title: "10: е4е Explained"
meta_title: "е4е Explained"
description: "Designing an Encoder for StyleGAN Image Manipulation by Omer et al. explained in 5 minutes."
date: 2021-4-13 19:00:00 -0000
categories: stylegan-encoder-latent-projection-gan-inversion-image-editing
---

#### Designing an Encoder for StyleGAN Image Manipulation by Omer et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![e4e Samples](/assets/images/e4e_teaser.jpg "e4e Samples")

##### 🎯 At a glance:

This architecture is the go to for StyleGAN inversion and image editing at the moment. The authors build on the ideas proposed in pSp and generalize the proposed method beyond the face domain. Moreover, the proposed method achieves a balance between the reconstruction quality of the images and the ability to edit them.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [pSp](https://t.me/casual_gan/16)

***

##### 🔍 Main Ideas:

**1) The inversion tradeoff:**  
One of the key ideas of the paper is that there exists a tradeoff between the quality of the reconstruction of an inverted image and the ability to coherently edit it by manipulating the corresponding latent vector. The authors state that better reconstruction quality is achieved by inverting images to W+, however edits for images inverted into the W space are more consistent at the cost of perceived similarity to the source image One possible reason for this behavior is that StyleGAN is trained in W, and while inverting in W+ has greater expressive power, the encoded latent codes lie further from the true distribution of the latent vectors learned by the generator. To balance these aspects of image inversion a W^k space is proposed, which has the ability to control the tradeoff by its proximity to W.

**2) Inverting images to W^k:**  
The authors propose two things to get W^k closer to W while retaining the expressiveness of W+.  
First, the encoder predicts a single latent code and a set of offsets equal to the number of layers in the StyleGAN generator that are added to the latent code to obtain the final set of vectors that is passed to the pretrained generator. In order to keep those offsets small, and the latent codes in turn closer to W, the offsets are regularized with L2.  
Second, the authors introduce a progressive scheme, where for the first 20k iterations the encoder predicts just the single latent vector that is used for each of the generator's layers. Then the model learns to predict the latent vector and the offset for the second layer of the generator (the predicted latent vector is used for all of the layers in the generator, except the ones for which an offset is predicted). A new offset is added every 2k iterations.  
It is important to note that the offsets are computed from different levels of the feature maps of the encoder taken from pSp with the intuition that the network learns to predict the coarse structure of the images and progressively learn to refine it with the learned offsets.  

**3) Latent discriminator:**  
To make sure the inverted latent codes do not fall out of the true distribution of the latent space learned by the generator, the authors employ a small discriminator that learns to distinguish between the real latent vectors sampled from StyleGAN's mapping network and fakes ones from the encoder.

**4) Generalization beyond the faces domain:**  
For all domains other than the facial domain the authors replace the ArcFace id loss with a cosine distance between features extracted from a ResNet-50 trained with MOCO2.

**5) Latent editing consistency:**  
The metric measures how well behaved the latent edits are and how well the image is reconstructed. Calculating LEC consists of 4 steps: inverting an image, performing an edit, inverting the edited image, and doing an inverse edit on it. Ideally the first and last images along with the latent codes are equal.

##### 📈 Experiment insights / Key takeaways:

The metrics provided are mostly inconclusive since there is a tradeoff between editability and distortion (quote from the paper): "However, the perceptual metrics contradict each other, resulting in no clear winner. As discussed, perceptual quality is best evaluated using human judgement"

***

##### 🖼️ Paper Poster:

![e4e Paper Poster](/assets/images/e4e.png "e4e Paper Poster")

***

##### ✏️My Notes:

- Decent name, alas not a meme, 3/5.
- While the model does wonders for FFHQ/CelebA images, it showed some pretty lackluster results for my own selfies that I tried to invert. Not only did the backgrounds not get inverted, which is expected, but the identity loss on the inverted selfies was quite severe.
- I do believe that this is mostly a limitation of StyleGAN not e4e as the authors state that it is possible to improve reconstruction by sacrificing some editability.
- It works nicely as a plug and play module in a StylGAN editing/inversion pipeline as shown by ReStyle, and StyleCLIP

##### 🔗 Links:
[e4e arxiv](https://arxiv.org/abs/2102.02766) / [e4e Github](https://github.com/omertov/encoder4editing)

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
