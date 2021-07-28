---
layout: post
title:  "19: Gaussianized Latent Spaces Explained"
meta_title: "Gaussianized Latent Spaces Explained"
description:  "Improving Inversion and Generation Diversity in StyleGAN using a Gaussianized Latent Space" by Wulff et al. explained in 5 minutes."
date:   2021-5-14 19:00:00 -0000
categories: gaussian_prior_latent_space_encoding
---

#### Improving Inversion and Generation Diversity in StyleGAN using a Gaussianized Latent Space" by Wulff et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![Improving Inversion and Generation Diversity in StyleGAN using a Gaussianized Latent Space teaser](/assets/images/gaussianized_teaser.jpg "Gaussianized Latent Spaces teaser")

##### 🎯 At a glance:

In this paper about improving latent space inversion for a pretrained StyleGAN2 generator the authors propose to model the output of the mapping network as a Gaussian, which can be expressed as a mean and a covariance matrix. This prior is used to regularize images that are projected into latent space  via optimization, which makes the inverted images lie in well conditioned regions of the generator's latent space, and allows for smoother interpolations and better editing.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [e4e](https://t.me/casual_gan/25)  


***

##### 🔍 Main Ideas:

**1) Gaussian latent space:**  
It is possible to embed any image into the latent space of a generator. For example, embedding an image of a car into a face generator, however these images would lie in ill-defined regions of the latent space, and it is nearly impossible to interpolate between these images. To make sure the inverted images stay within the learned distribution the authors propose to use a gaussian prior when inverting images.
It turns out that the distribution of latent codes in W is highly irregular, and hard to describe analytically, however undoing the last leaky ReLU with slope = -0.2 with another leaky ReLU with slope = -5 the distribution of latent vectors sampled from the mapping network becomes a high-dimensional Gaussian. Hence it is possible to empirically compute the mean and the covariance matrix of this Gaussian by sampling a set of vectors from the mapping network.

**2) Improving inversion:**  
To improve the inversion the authors add a regularizer to the standard inversion optimization that finds a latent vector that minimizes the distance between the input image and the image that is generated from the optimized vector. The regularizer is the empirical covariance matrix-multiplied on the left and right by the difference between the "gaussianized" (by Leaky ReLU with a slope of -5.0) optimized latent vector and the empirical mean of the gaussianized latent space.
It is possible to extend this approach to W+ by summing the regularization terms for each of the optimized vectors in W+

**3) Removing artifacts using the Gaussian prior:**  
The go-to method for increasing the sample quality in generative models is the truncation trick that pushes samples closer to the average vector, which leads to a decrease in sample diversity since all images start to look similar to the average image. The authors suggest an alternative approach to combat generation artifacts.  They perform PCA on the Gaussian latent space to find the main directions of variation, and project the latent vectors onto the computed primary components. As it turns out, images with artifacts have very large values in certain components, especially lower dimensions. The final step is to apply logarithmic compression to the large components, and the resulting images become virtually artifact-free, while retaining generated content diversity.

##### 📈 Experiment insights / Key takeaways:

- Truncation changes the identity of the person on the image, while PCA compression does not
- User study concluded that the interpolation quality for the proposed method is miles ahead of the baseline (2-8 times higher)
- The reconstruction errors on images are 3-4 times lower compared to the baseline

***

##### 🖼️ Paper Poster:

![Improving Inversion and Generation Diversity in StyleGAN using a Gaussianized Latent Space poster](/assets/images/gaussianized.png "Gaussianized Latent Spaces Paper Poster")

***

##### ✏️My Notes:

- No name to rate here
- Kind of a counterintuitive idea since the whole motivation for the mapping network was to let the generator learn a complex latent space and move away from the simple gaussian prior since it was deemed not expressive enough.
- Cool that it is a trick that does not require to train anything

##### 🔗 Links:
[Gaussianized Latent Spaces arxiv](https://arxiv.org/pdf/2009.06529.pdf) / Gaussianized Latent Spaces github <- not available at this time

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
