---
layout: post
title: "56: IC-GAN"
meta_title: "IC-GAN"
description: "Instance-Conditioned GAN by Arantxa Casanova et al. explained in 5 minutes."
date: 2021-9-24 16:00:00 -0000
categories: unaligned-image-class-conditional-generation
---

#### Instance-Conditioned GAN by Arantxa Casanova et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![IC-GAN teaser](/assets/images/icgan_teaser.png "IC-GAN Teaser")

##### 🎯 At a glance:

Aren’t you tired of only seeing generated FFHQ-like faces? I bet you are, and if you know just how atrocious the samples from StyleGAN-2 trained on other datasets such as ImageNet really look you should be wildly excited to see Instance Conditioned GAN (IC-GAN) by Arantxa Casanova and the team at Facebook AI Research! IC-GAN flips the script and uses unaligned images to condition the generator to synthesize samples similar to the input data points. This approach can be thought of as learning overlapping local distributions around the input images, which lets it train on diverse unaligned images while maintaining the latent space density needed for high-quality image synthesis.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [KDE](https://en.wikipedia.org/wiki/Kernel_density_estimation)

***

##### 🚀 Motivation:

Existing approaches that use unsupervised data partitioning fall short of SOTA class-conditional GANs mostly due to using coarse and non-overlapping data partitions. On the other hand, using finer partitions causes sparse image clusters with few input data points. Intuitively IC-GAN learns to model local distributions of the input images as a mixture of the densities of their neighbors. This is such an elegant way to increase the number of clusters to the maximum (number of clusters equals the number of data points) without reducing the number of points in each cluster. Another way to think of IC-GAN is as an adversarial KDE in which the local density is modeled implicitly with a neural network.

***

##### 🔍 Main Ideas:

Perhaps surprisingly, IC-GAN is pretty straightforward. A ResNet50 pretrained on either a classification task or in a self-supervised manner with SwAV for the labeled and unlabeled settings respectively is used as an encoder to obtain embeddings for all input images.

An iteration of training then consists of picking an input image, and its 5 or 50 (depending on the dataset) nearest neighbors according to the cosine similarities of their embeddings. The Generator is passed the input image, based on which it synthesizes a batch of images that should be indistinguishable from its real nearest neighbors.

The Discriminator is shown the batch of real and fake neighbor samples along with the input image that is referred to as the instance condition (roll credits) and predicts, which images are fake, and which are real.

And that is it. Almost. Just one small thing left: for class-conditional generation, both networks also receive the class label as a condition.

##### 📈 Experiment insights / Key takeaways:

- IC-GAN is evaluated on ImageNet and COCO-Stuff, and it beats all previous approaches as well as diffusion-based models in terms of FID and IS almost everywhere on 128 and 256 resolutions even for specialized models with twice the parameter count for unlabeled generation, and is on par with BigGAN or better for class-conditional generation
- Off-the-shelf domain transfer works surprisingly well, and it is not due to dataset similarity
- It is better to compute the cluster centers with k-means and select neighbors closest to the centroid than randomly sampling from the cluster
- storing more than 1000 points in a cluster does not improve the result
- Increasing the number of stored instances, allows to better recover the data density at the expense of slightly degraded image quality in lower density regions of the manifold.

***

##### 🖼️ Paper Poster:

![GSN paper poster](/assets/images/icgan.png "GSN Paper Poster")

***

##### 🛠 Possible Improvements:

- Avoid storing a lot of training instances
- Train the encoder jointly with the generator for better embeddings
- Improve transfer quality to datasets very different from ImageNet

##### ✏️My Notes:

- (3/5) for the name. IC-GAN is good if a bit generic name. I just feel like there is room for some wordplay around IC (pronounced like “I See”) GAN that is left unexplored.
- My favorite section makes a triumphant return: Can This be CLIPed? Oh, wait the Authors actually provide a colab with built-in CLIP support!!! Mad respect 🤝
- Looks like I might have to pull myself together and cover diffusion models in the near future ... but not before I cover a couple more cool 3D papers ;)
- Do you have any questions left about IC-GAN? Let’s discuss in the comments!

##### 🔗 Links:
[IC-GAN arxiv](https://arxiv.org/pdf/2109.05070v1.pdf) / [IC-GAN github](https://github.com/facebookresearch/ic_gan)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Editable 3D Scenes - Object-NeRF]({% post_url 2021-9-18-Object-NeRF-explained %})
- [The second half of VQGAN + CLIP for text-image similarity - CLIP]({% post_url 2021-9-14-CLIP-explained %})
- [GSN - Generative Scene Networks (VR NeRF)]({% post_url 2021-9-21-GSN-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
