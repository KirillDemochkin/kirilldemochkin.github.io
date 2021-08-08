---
layout: post
title: "14: EigenGAN Explained"
meta_title: "EigenGAN Explained"
description: "EigenGAN: Layer-Wise Eigen-Learning for GANs by Zhenliang He et al. explained in 5 minutes."
date: 2021-4-27 19:00:00 -0000
categories: discovering-semantic-directions-latent-editing-interpretable-generation
---

#### EigenGAN: Layer-Wise Eigen-Learning for GANs by Zhenliang He et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![EigenGAN: Layer-Wise Eigen-Learning for GANs Scale Samples](/assets/images/3-2_pose.gif "EigenGAN Samples")
![EigenGAN: Layer-Wise Eigen-Learning for GANs Scale Samples](/assets/images/celeba_4-5_smiling.gif "EigenGAN Samples")

##### 🎯 At a glance:

The authors propose a novel generator architecture that can intrinsically learn interpretable directions in the latent space in an unsupervised manner. Moreover each direction can be controlled in a straightforward way with a strength coefficient to directly influence the attributes such as gender, smile, pose, etc on the generated images.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [Transformers](https://arxiv.org/abs/1706.03762)

***

##### 🔍 Main Ideas:

**1) Generator with layer-wise subspaces:**  
The generator architecture is simply a stack of transposed convolution layers that start from a latent noise vector processed by a fully-connected layer and increase the resolution of the image twofold. Before each transposed conv2d layer an orthogonal linear subspace is injected.

**2) Linear subspace structure:**  
Each linear subspace is comprised of 3 elements (all of which are learned during training): a set of orthonormal vectors U that correspond to interpretable directions, a diagonal matrix L that controls the strength of each direction, and an offset vector μ that denotes the origin of the subspace. The orthogonality of U is achieved via regularization

**3) Injection mechanism:**  
To inject this subspace a random noise vector z (coordinate) is sampled and projected into the subspace by left-multiplying it with the U and L matrices, and adding the offset μ. The resulting vector is either plainly added to the feature map from the previous layer or, alternatively, processed by a 1x1 convolution. This process is repeated for each layer with a new sampled coordinate.

**4) Details:**  
For a single linear subspace the authors prove that the columns of U are the results of PCA of the space. When the subspaces are injected into a hierarchical nonlinear generator it can be seen as progressively adding new "straight" dimentions that bend and curve after each nonlinear transposed convolution layer. The authors limit each subspace to just 6 basis vectors, and observe that the subspaces injected into the earlier layers correspond to more love level attributes such as pose and hue, while the later subspaces learn directions for more abstract attributes such as facial hair, hair side, and background texture orientation.

##### 📈 Experiment insights / Key takeaways:
The main takeaway is that EigenGAN has better disentanglement than directions discovered with SeFa in StyleGAN, additionally the EigenGAN directions are more similar to a PCA decomposition of the latent space than directions from other baselines.

***

##### 🖼️ Paper Poster:

![EigenGAN: Layer-Wise Eigen-Learning for GANs Paper Poster](/assets/images/eigengan.png "EigenGAN Paper Poster")

***

##### ✏️My Notes:

- 4/5 for the name - it's cool and straight to the point  
- Important to note that higher level attributes are not very well disentangled, although the authors claim that they are still interpretable  
- Interesting whether real images can be projected well into the latent space of the proposed generator  
- I would like to see, whether this method can be applied to other domains besides faces  
- Do learned directions depend very much on the initialization? Or are they close every time? (Authors note that some more rare attributes are not discovered everytime)  
- Is it possible to swap the learned eigen-dimensions between two generators trained on different datasets?  

##### 🔗 Links:
[EigenGAN arxiv](https://arxiv.org/pdf/2104.12476.pdf) / [EigenGAN Github](https://github.com/LynnHo/EigenGAN-Tensorflow)

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
