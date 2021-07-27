---
layout: post
title:  "40: SimSiam Explained"
meta_title: "SimSiam Explained"
description:  "Exploring Simple Siamese Representation Learning by Xinlei Chen et al.  explained in 5 minutes."
date: 2021-7-27 19:00:00 -0000
categories: self-supervised-contrastive-learning-siamese-networks
---

#### Exploring Simple Siamese Representation Learning by Xinlei Chen et al.  explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![Exploring Simple Siamese Representation Learning teaser](/assets/images/simsiam_teaser.png "SimSiam teaser")

##### 🎯 At a glance:

We have seen all sorts of tricks to make self-supervised learning work: negative sample pairs, large batches, momentum encoders, and so on. Now, the authors of SimSiam claim that none of these are necessary, and their approach achieves competitive results on ImageNet and downstream tasks without using any of the above! The proposed method uses simple Siamese networks with stop-gradient.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [SimCLR]({% post_url 2021-7-13-SimCLR %})  
2) [MoCo]({% post_url 2021-7-23-MoCo-explained %})  
3) [BYOL]({% post_url 2021-7-16-BYOL-explained %})

***

##### 🔍 Main Ideas:

**1) Overview & Loss:**  
A standard contrastive learning setup is followed: two networks sharing weights are shown two different augmented views of the same image, and an MLP projection head transforms one view to match the other. Interestingly, the contrastive loss is symmetric: the output of one of the networks is treated as constant (using stop-gradient), and the output of the second network is projected to match it. Then the two networks switch roles, and the output of the first network is projected and matched to the "constant" output of the second network. The two losses are averaged, and each of the networks receives gradient for the loss term, where stop-gradient is not applied to its output.

**2) The Effect of Stop-Gradient:**  
Without the stop-gradient operation the network representations completely collapse (See point n. 5 for details).

**3) The Effect of the predictor MLP:**  
The model does not work if the MLP is a fixed random network or if it is removed completely. A constant LR for the MLP works well since it helps the projection better adapt to the latest representations. In the case of the symmetric loss, removing the MLP causes the gradient of the loss function to be in the same direction as without using the stop-gradient operation in the loss function, which was shown to make the representations collapse to the trivial solution.

**4) The Effect of Batch Size and BatchNorm:**  
SimSiam does not require a large batch size to work unlike [SimCLR]({% post_url 2021-7-13-SimCLR %}), and works well without a specialized LARS optimizer. BN is helpful for optimization when used everywhere, except the output of the MLP projection head. Yet, BN alone does not cause or prevent representation collapse.

**5) Why SimSiam is an Expectation-Maximization like algorithm:**  
The authors suppose that SimSiam is essentially optimizing a loss with respect to two sets of variables: using one set of variables (encoder weights) as cluster centers, and the other as assignment vectors (representations). SimSiam can be approximated by using two alternating steps: First, sampling an augmentation, and using it to update the representations, and, second, treating them as constant, updating the encoder weights. The MLP predictor is used to compensate for approximating the true expectation over all images and augmentations that is unrealistic to compute. The symmetric setup of SimSiam helps sample images and augmentations more densely.

**6) Why isn't collapsing?**  
Based on empirical observations, the authors conclude that it is unlikely that the output of a randomly initialized network would be constant, and gradients are not computed jointly for all x, hence starting from this optimization it will be hard for the optimizer to approach a constant representation, instead seeking a trajectory, where the representations are scattered.

##### 📈 Experiment insights / Key takeaways:

- SimSiam beats [SimCLR]({% post_url 2021-7-13-SimCLR %}) in all cases, and all methods under 100-epoch pretraining
- Siamese structure is a core factor for the general success of the considered baselines
- SimSiam is [SimCLR]({% post_url 2021-7-13-SimCLR %}) without negative pairs, neither the stop-gradient nor the extra predictor is necessary or helpful for [SimCLR]({% post_url 2021-7-13-SimCLR %})
- SimSiam is SwAV without online clustering, adding the predictor does not help either. Removing stop-gradient (so the model is trained end-to-end) leads to divergence
- SimSiam is [BYOL]({% post_url 2021-7-16-BYOL-explained %}) without the momentum encoder

***

##### 🖼️ Paper Poster:

![Exploring Simple Siamese Representation Learning paper poster](/assets/images/simsiam.png "SimSiam Paper Poster")

***

##### ✏️My Notes:

- (4/5) SimSiam rolls of the tongue and is a nice callback 👌
- I think this was conceptually the hardest of the SSL papers I have covered so far because of all the theorizing, and hypothesizing, although the method itself is very straightforward
- Just one paper left to cover in this planned SSL series
- Have you tried SimSiam? Let me know in the comments!

##### 🔗 Links:
[SimSiam arxiv](https://arxiv.org/abs/2011.10566) / [SimSiam github](https://github.com/facebookresearch/simsiam)

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
