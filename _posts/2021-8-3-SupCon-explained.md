---
layout: post
title:  "42: SupCon Explained"
meta_title: "SupCon Explained"
description:  "Supervised Contrastive Learning by Prannay Khosla et al. explained in 5 minutes."
date: 2021-8-3 19:00:00 -0000
categories: contrastive-loss-supervised-classification
---

#### Supervised Contrastive Learning by Prannay Khosla et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![Supervised Contrastive Learning Data Samples](/assets/images/supcon_teaser.png "SupCon teaser")

##### 🎯 At a glance:

Self-supervised learning is all about that contrastive loss, but is it possible to apply contrastive losses in a supervised setting? Prannay Khosla and his colleagues from a bunch of different organizations introduce Supervised Contrastive Learning - an approach that leverages the information available in the known class labels to augment the contrastive loss with many positive pairs in addition to many negative pairs. The proposed loss is simple in concept, yet it beats current state-of-the-art approaches on top-1 accuracy for ImageNet. This is the first contrastive loss to outperform models trained with the cross-entropy loss.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [SimCLR]({% post_url 2021-7-13-SimCLR %})

***

##### 🚀 Motivation:
Recent self-supervised learning papers use contrastive losses to pull together an "anchor" and a "positive" sample (a randomly augmented view of the anchor) in the embedding space, and push apart the "anchor" from many "negative" samples (other samples in a batch). In the supervised setting additional information is available in the form of class labels, hence it makes sense to generalize the loss to use multiple positive samples from the same class as the anchor. The use of many positive and negative samples per anchor allows SupCon models to achieve SOTA accuracy without hard-negative mining.

***

##### 🔍 Main Ideas:
**1) Training Pipeline:**  
SupCon is essentially supervised SimCLR:  
Augmentation module that produces two views of each training batch,  
Encoder network that maps each image to a size-2048 embedding vector (this is the final embedding that is used at inference),  
Projection network that reduces the size of the embedding vector to 128 for the SupCon loss to be computed (it is discarded at the end of training).

**2) SupCon Loss:**  
There are two ways to generalize the contrastive loss to an arbitrary number of positives: with the summation over them inside of the log of the contrastive loss, and outside of it. Both variants encourage  the encoder to give closely aligned representations to all samples from the same class, have the implicit hard positive/negative mining property in their gradients that removes the need for explicit hard mining, and retain the increase in contrastive power with an increased number of negative samples. However, the second variant (summation outside) achieves significantly higher performance (possibly due to more optimal gradient structure)

##### 📈 Experiment insights / Key takeaways:

- AutoAugment performed the best out of the considered data augmentation strategies
- ResNet is used as the encoder
- SupCon achieves a new state of the art accuracy of 78.7% on ResNet-50 with AutoAugment (top-1 ImageNet)
- The SupCon models have lower mCE values across different corruptions, and less accuracy lost with increased corruption
- SupCon has high hyperparameter stability
- SupCon is on par with cross-entropy and self-supervised contrastive loss on transfer learning performance

***

##### 🖼️ Paper Poster:

![Supervised Contrastive Learning paper poster](/assets/images/supcon.png "SupCon Paper Poster")

***

##### ✏️My Notes:

- (2/5) SupCon doesn't quite flow when said out loud. I need a **DeceptiCon** paper 😂
- I was planning for this to be the end-cap to the SSL learning series that I just finished, but turns out this paper is not bout SSL at all 💁‍♂️
- For some reason it is a bit quiet on the recent releases front, so I might cover a couple more papers from last year. Any suggestions on what I missed?
- As always, if you have tried using SupCon let me know in the comments!

##### 🔗 Links:
[SupCon arxiv](https://arxiv.org/pdf/2004.11362.pdf) / [SupCon github](https://github.com/HobbitLong/SupContrast)

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
