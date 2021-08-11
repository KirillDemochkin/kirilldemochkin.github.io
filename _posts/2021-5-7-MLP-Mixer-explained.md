---
layout: post
title: "17: MLP-Mixer Explained"
meta_title: "MLP-Mixer Explained"
description: "MLP-Mixer: An all-MLP Architecture for Vision by Tolstikhin et al explained in 5 minutes."
date: 2021-5-7 19:00:00 -0000
categories: mlp_vision_transformer_no_convolution
---

#### MLP-Mixer: An all-MLP Architecture for Vision by Tolstikhin et al explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![MLP-Mixer: An all-MLP Architecture for Vision Samples](/assets/images/mlpmixer_teaser.jpg "MLP-Mixer Samples")

##### 🎯 At a glance:

This paper is a spiritual successor of Vision Transformer from last year. This time around the authors once again come up with an all-MLP (multi layer perceptron) model for solving computer vision tasks. This time around, no self-attention blocks are used either (!) instead two types of "mixing" layers are proposed. The first is for interaction of features inside patches , and the second - between patches.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [ViT]({% post_url 2021-4-30-ViT-explained %})

***

##### 🔍 Main Ideas:

**1) Big picture:** 
There are no convolutions or spatial attention blocks used anywhere in the network architecture. As input it accepts a sequence of linearly projected image patches (tokens) and operates on them without changing the dimensionality (patches x channels table). All patches are projected using the same projection matrix. The overall structure goes like this: an image patch embedding layer followed by a number of Mixer layers that are connected to a global average pooling layer and a fully connected layer that predicts the resulting class.

**2) Mixer layer:** 
The core idea of this novel layer is to separate feature mixing at spatial locations (channel-mixing) and across spatial locations (token-mixing). Each mixer layer consists of a layer norm, two MLP blocks (two fully-connected layers with a GELU nonlinearity in-between), and two skip connections between the input, and outputs of each MLP block.
The first block operates on transposed features (channels x tokens). For each channel it combines feature values across all tokens. The features are then transposed back to their original shape (tokens x channels), layer-normed, and passed to a second MLP that processes and reweights all channels for each token.
The same MLP is reused for each channel in the first case, and for each token in the second block.
No positional encoding is used anywhere in the model since token-mixing MLPs can become location aware through training.

##### 📈 Experiment insights / Key takeaways:

- Mixer improves faster with more training data than  other baselines
- Mixer runs faster than ViT
- Mixer achieves 87.94% top-1 accuracy on ImageNet when pretrained on JFT-300M
- Mixer overfits faster than ViT on smaller datasets

***

##### 🖼️ Paper Poster:

![MLP-Mixer: An all-MLP Architecture for Vision Paper Poster](/assets/images/mlpmixer.png "MLP-Mixer Paper Poster")

***

##### ✏️My Notes:

- 5/5 - such a dope name!
- Same thing as with [ViT](https://t.me/casual_gan/33), MLP-Mixer is very data-hungry
- As the authors point out, it is very interesting to see what inductive biases are hidden in the feature learned by the model, and how they relate to generalization
- Interesting to see where else these ideas could be applied: NLP, music, generative tasks?

##### 🔗 Links:
[MLP-Mixer arxiv](https://arxiv.org/pdf/2105.01601.pdf) / [MLP-Mixer Github](https://github.com/google-research/vision_transformer)

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
