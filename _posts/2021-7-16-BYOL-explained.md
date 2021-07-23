---
layout: post
title:  "37: BYOL Explained"
meta_title:  "BYOL Explained"
description: "Bootstrap Your Own Latent: A New Approach to Self-Supervised Learning by Jean-Bastien Grill et al. explained in 5 minutes or less"
date:   2021-7-16 19:00:00 -0000
categories: self-supervised-contrastive-representation-learning
redirect_from: /self-supervised/representation-learning/online-target-networks/2021/07/13/BYOL.html
---
  
#### Bootstrap Your Own Latent: A New Approach to Self-Supervised Learning by Jean-Bastien Grill et al. explained in 5 minutes or less. 

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑 

***

![BYOL: Bootstrap Your Own Latent samples](/assets/images/byol_teaser.jpg "BYOL Paper teaser")

##### 🎯 At a glance:

Is it possible to learn good image representations for many downstream tasks at once without negative pairs? A well known approach is to use self-supervised pretraining such as state-of-the art contrastive methods that are trained to reduce the distance between representation of augmented views of the same image (positive pairs) and increasing the distance between representations of augmented views of different images. These methods need careful treatment of negative pairs, whereas BYOL achieves higher performance than SOTA contrastive methods without using negative pairs at all. Instead, it uses two networks that learn from each other to iteratively bootstrap the representations by forcing one network to use an augmented view of an image to predict the output of the other network for a different augmented view of the same image.  Sounds crazy, I know... but it actually works! Keep reading to learn the ideas that let BYOL beat out other self-supervised approaches.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core contributions of this paper):*
1) [SimCLR]({% post_url 2021-7-13-SimCLR %})

***

##### 🔍 Main Ideas:
**1) Target network base-case:**
One way to prevent collapse to a trivial solution (predicting the same code for every image) without comparing negative pairs is to use a fixed randomly initialized network to produce the targets for the predictions. It does not result in good representations but it still much better than using random representations. This observation is the key to BYOL's success: from a given "target" representation we can build a new better "online" representation by predicting the target representation. Hence, we can build a sequence of representations that are iteratively refined by using the online network as the target.

**2) BYOL structure:**
The most straightforward way to build a sequence of target networks is to checkpoint the online network at set intervals to use as the new target. In practice though, an exponentially moving average of the online network is used as the target network. The networks consist of an encoder (ResNet50) and a projector (Dense layer with a ReLU), the online network additionally has a predictor module (same as the projector). Both networks share the architectures but not the weights. The output of the encoder module is used as the final representation in the downstream tasks.  
A single training iteration consists of generating two augmented views of the same image, passing them through the two networks to obtain the projections, using the prediction module in the online network to obtain the predicted projection of the target network, and calculating the MSE between the normalized predictions and target projections. The online network is updated using gradient descent, and the target network is updated using EMA.

**3) BYOL intuition:**
Interestingly enough BYOL relies on the so-called "neural optimism" meaning that since there is no a priori reason for BYOL representations to collapse, they won't. This intuition is proven empirically in that the collapsed representations are unstable.

##### 📈 Experiment insights / Key takeaways:
- BYOL almost matches the best supervised baseline on top-1 accuracy on ImageNet and beasts out the self-supervised baselines.
- BYOL can be successfully used for other vision tasks such as detection
- BYOL is not affected by batch size dynamics as much as SimCLR
- BYOL does not rely on the color jitter augmentation unlike SimCLR. The intuition here is that in SimCLR with just random crops the color histograms of the augmented views are enough to differentiate the input images. In BYOL however the network is encouraged to keep as much of the information as possible to improve the online predictions.

***

##### 🖼️ Paper Poster:

![BYOL: Bootstrap Your Own Latent explained](/assets/images/BYOL.png "BYOL Paper Poster")

***

##### ✏️My Notes:
- It would be interesting to see the progression of experiments that led to authors gaining the insight that a fixed random network can prevent representation collapse. It sort of sounds like they stumbled onto the insight and then built the narrative around it.
- I don't know about you guys, but I found it extremely surprising that predicting the output of the average network from past iterations lets you learn really good representations 🤯. Is this approach used in other areas of deep learning? (I know that reinforcement learning is mentioned in the paper)
- I found their insight about the need for color jitter in SimCLR very helpful for understanding why self-supervised learning works at all.

##### 🔗 Links:
[BYOL arxiv](https://arxiv.org/pdf/2006.07733.pdf) / [BYOL github](https://github.com/deepmind/deepmind-research/tree/master/byol)

***

##### 👋 Thanks for reading!
*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*  

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
