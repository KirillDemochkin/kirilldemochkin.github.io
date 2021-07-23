---
layout: post
title:  "39: MoCO Explained"
meta_title: "MoCo Explained"
description:  "Momentum Contrast for Unsupervised Visual Representation Learning by Kwonjoon Lee et al. explained in 5 minutes."
date:   2021-7-23 19:00:00 -0000
categories: self-supervised-representation-learning-encoder
---

#### Momentum Contrast for Unsupervised Visual Representation Learning by Kwonjoon Lee et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![Momentum Contrast for Unsupervised Visual Representation Learning teaser](/assets/images/moco_teaser.png "MoCo teaser")

##### 🎯 At a glance:

The core motivation of self-supervised learning (SSL) is to use pretraining on unlabeled data to obtain robust embeddings useful for many downstream tasks. Yet, one of the recurring problems in SSL is managing a large number of negative pairs necessary for stable training. In MoCo, a ResNet-based general purpose encoder, a constantly updated queue of recent batch encodings is used in place of a very large batch of negative pairs during training. The considered approach coupled with a momentum-based update scheme for one of the encoders outperforms its supervised pre-training counterpart in 7 detection/segmentation tasks.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [SimCLR]({% post_url 2021-7-13-SimCLR %})
2) [BYOL]({% post_url 2021-7-16-BYOL-explained %})

***

##### 🔍 Main Ideas:

**1) Momentum Contrast Dictionary as a Queue:**
Authors view contrastive learning as building a discrete dictionary of feature vectors for high-dimensional inputs (images). Their hypothesis is that good features are learned when the dictionary covers a rich set of negative samples and dictionary keys a kept as consistent as possible despite the key encoder updating during training. In practice MoCo uses two encoders with the same architecture: a query encoder for dealing with the incoming online data, and a key encoder for maintaining a queue-based dictionary of recent batch embeddings. This queue is constantly cycling new batches in and discarding old batches. The idea is very clever as it enables using a normal sized batch for training on positive pairs, and using the entire queue of past embeddings as a pseudo-batch of negative pairs.

**2) Momentum update:**
Unfortunately, using a queue makes it impossible to use gradient descent to update the key encoder. One simple option is to copy the query encoder at each iteration but this approach fails since the rapidly changing encoder makes the keys inconsistent between iterations. Instead, the authors use a momentum update to smoothly change the key encoder without compromising the keys. This is essentially an exponentially moving average of the query encoder parameters with a large (0.999) momentum value.

**3) Encoder Pipeline:**
The already familiar training pipeline is used. There are two encoders that see different augmented views of the same input samples, and the goal for the query encoder is to minimize the InfoNCE loss by minimizing he distance between the query embeddings and key embeddings for positive pairs and maximizing it for negative pairs, with dot product between the embeddings as the similarity score.

**4) MoCoV2:**
Since this paper is 3 pages long, I will just put the improvements here.
Following SimCLR, the authors add a 2-layer MLP head with ReLU after the ResNet embeddings to compute the loss in "contrastive loss space". Additionally, the blur augmention is used to improve the scores on downstream tasks. Interestingly, stronger color augmentations actually hurt the scores.

##### 📈 Experiment insights / Key takeaways:

- MoCo is pretrained on ImageNet and tested on Pascal VOC and COCO
- The features are normalized to match the downstream task hyperparameters
- The transferring accuracy depends on the detector backbone
- MoCo outperforms the considered baselines: RelPos, Jigsaw, LocalAgg, and Multi-task
- MoCo can outperform its ImageNet supervised pre-training counterpart in 7 detection or segmentation tasks
- MoCo is on par on Cityscapes instance segmentation, and lags behind on VOC semantic segmentation
- MoCoV2 beats [SimCLR]({% post_url 2021-7-13-SimCLR %}) with the same (and fewer) number of iterations

***

##### 🖼️ Paper Poster:

![Momentum Contrast for Unsupervised Visual Representation Learning paper poster](/assets/images/MoCo.png "MoCo Paper Poster")

***

##### ✏️My Notes:

- (4/5) for the name. MoCo sounds very anime.
- Probably should have covered this before [BYOL]({% post_url 2021-7-16-BYOL-explained %}), as it is almost the same  thing (just less intuitive, imho) but without a couple of MLP blocks, and still uses the negative pairs for training
- It is really interesting how many variations of the same idea people came up with to solve the negative pairs problem
- While it is good to check this paper out for historic reasons, just use one of the newer approaches

##### 🔗 Links:
[MoCo arxiv](https://openreview.net/pdf?id=sSjqmfsk95O) / [MoCo github](https://github.com/zsyzzsoft/co-mod-gan)

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
