---
layout: post
title: "15: ViT Explained"
meta_title: "ViT Explained"
description: "An Image Is Worth 16X16 Words: Transformers For Image Recognition At Scale by Dosovitskiy et al. explained in 5 minutes."
date: 2021-4-30 19:00:00 -0000
categories: #large-scale-image-transformers-pretraining-classification
---

#### An Image Is Worth 16X16 Words: Transformers For Image Recognition At Scale by Dosovitskiy et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![An Image Is Worth 16X16 Words: Transformers For Image Recognition At Scale Samples](/assets/images/vit_teaser.png "ViT Samples")

##### 🎯 At a glance:

In this paper from late 2020 the authors propose a novel architecture that successfully applies transformers to the image classification task. The model is a transformer encoder that operates on flattened image patches. By pretraining on a very large image dataset the authors are able to show great results on a number of smaller datasets after finetuning the  classifier on top of the transformer model.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [Transformers](https://arxiv.org/abs/1706.03762)

***

##### 🔍 Main Ideas:

**1) Patch tokens:**  
The image is split into small patches (16x16), and each patch is completely flattened into a single vector of all of the pixel color values. The patches are projected with a linear layer into a smaller dimension (512) and concatenated with a learnable 1D positional encoding (turns out it implicitly learns to represent 2D coordinates), and an optional "classification" token embedding. This sequence of embeddings serves as the input for the transformer.

**2) Vision Transformer:**  
The transformer part is just the standard transformer model: alternating layers of multiheaded self-attention and feedforward blocks with layernorm applied before every block and residual connections after every block. The feedforward blocks have two layers with GELU non-linearity.

**3) Finetuning:**  
During inference it is possible to process images of higher resolution, since the transformer works with any sequence length. However, it is necessary to interpolate the positional encodings since they may lose their meaning at different resolutions than the one they have been trained for. It is also possible to finetune the pretrained model to any dataset by removing the pretrained classifier "head", and replacing it with a zero initialized feedforward layer that predicts probabilities for the classes in the new dataset.

##### 📈 Experiment insights / Key takeaways:
The main takeaway is that ViT requires A LOT of data to "git gud", but when it gets the data it outperforms SOTA classifiers on downstream tasks, and on larger datasets it pretrains faster than other baseline methods.

***

##### 🖼️ Paper Poster:

![An Image Is Worth 16X16 Words: Transformers For Image Recognition At Scale Paper Poster](/assets/images/vit.png "ViT Paper Poster")

***

##### ✏️My Notes:

- 3/5 for the name: very utilitarian, lacking some pizzazz.
- I guess the main question is about data availability for training ViT
- There is a point in the appendix about self-supervised pretraining: the authors corrup 50% of embeddings and ask the model to predict the average rgb value of the missing patch (predicting a 4x4 patch did not work as good).  It works good enough i suppose.

##### 🔗 Links:
[ViT arxiv](https://arxiv.org/abs/2010.11929) / [ViT Github](https://github.com/lucidrains/vit-pytorch)

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
