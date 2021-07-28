---
layout: post
title:  "20: DINO Explained"
meta_title: "DINO Explained"
description:  "Emerging Properties in Self-Supervised Vision Transformers by Caron et al. explained in 5 minutes."
date:   2021-5-18 19:00:00 -0000
categories: self-supervised-segmentation-vision-transformers
---

#### Emerging Properties in Self-Supervised Vision Transformers by Caron et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![Emerging Properties in Self-Supervised Vision Transformers teaser](/assets/images/dino_teaser_2.webp "DINO teaser")
![Emerging Properties in Self-Supervised Vision Transformers teaser](/assets/images/dino_teaser_1.gif "DINO teaser")

##### 🎯 At a glance:

In this paper from Facebook AI Research the authors propose a novel pipeline to train a [ViT](https://t.me/casual_gan/33) model in a self-supervised setup. Perhaps the most interesting consequence of this setup is that the learned features are good enough to achieve 80.1% top-1 score on ImageNet. At the core of their pipeline is a pair of networks that learn to predict the outputs of one another. The trick is that while the student network is trained via gradient descent over the cross-entropy loss functions, the teacher network is updated with an exponentially moving average of the student network weights. Several tricks such as centering and sharpening are employed to combat mode collapse. As a fortunate side-effect the learned self-attention maps of the final layer automatically learns class-specific features leading to unsupervised object segmentations.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [ViT](https://t.me/casual_gan/33)  
2) [BYOL]({% post_url 2021-7-16-BYOL-explained %})

***

##### 🔍 Main Ideas:

**1) Self-supervised learning with knowledge distillation:**  
In DINO the student network tries to match the output of the teacher network that is a vector of probability distribution over its dimensions. Both output vectors are normalized by softmax with a temperature parameter that controls the sharpness of the distribution. Higher values for smoother, lower for sharper. The teacher network is fixed, and the student network is trained with a cross-entropy loss between the two output probability distribution vectors. Both networks share the same architecture but with different sets of parameters

**2) Local to global correspondence:**  
The two networks actually see different transformed views of the same input image. The teacher network sees only 2 high resolution "global" views, while the student network additionally sees several smaller (less than 50% of full image) "local" views.

**3) Teacher Network:**  
Since DINO is self supervised there is no pretrained teacher network, hence it is built directly during training. Empirically the best results were obtained with a momentum encoder. Meaning that the weights for the teacher network for an iteration are equal to the exponentially moving average of the student weights from previous iterations.

**4) Architecture:**  
The backbone of DINO is [ViT](https://t.me/casual_gan/33), followed by a projection head made of 3 fully connected layers of size 2048, L2 normalization and a weight-normalized fully connected layer. 8x8 patches are used as input to [Vit](https://t.me/casual_gan/33)5)

**5) Avoiding Mode Collapse:**  
Two opposite operations are used: centering, which adds a bias term to the teacher outputs to stop one dimension from becoming too dominant, and sharpening that uses a low temperature in the teacher softmax to prevent collapse to a uniform distribution, which is a side effect of centering. The "center" is updated with an EMA, which works well for varying batch sizes.

##### 📈 Experiment insights / Key takeaways:

- DINO outperforms other baselines with a simple k-NN classifier on ImageNet
- DINO outperforms supervised baselines on ImageNet image retrieval benchmark
- DINO works well for copy detection tasks
- DINO's self-attention maps are competitive for video instance segmentation
- DINO's features transfer better to downstream tasks than comparable supervised methods

***

##### 🖼️ Paper Poster:

![Emerging Properties in Self-Supervised Vision Transformers paper poster](/assets/images/dino.png "DINO Paper Poster")

***

##### ✏️My Notes:

- (4/5) DINO is a dope name but it is just random parts of words from the full title
- The authors decrease the spatial sizes of patches (compared to [ViT](https://t.me/casual_gan/33)), which obviously boosts accuracy, but harms memory and performance. Interesting to see what could be done to alleviate this problem.
- With the number of Vision Transformer models I am covering lately, this channel might as well be renamed to "Casual Transformers"
- How long do you think until the vision transformers have their "StyleGAN/ImageNet" moment and it becomes obvious if and why they are superior to convnets for CV tasks? I think we will know by the end of the year for sure. Comment bellow, and let's discuss!

##### 🔗 Links:
[DINO arxiv](https://arxiv.org/pdf/2104.14294v1.pdf) / [DINO github](https://github.com/facebookresearch/dino)

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
