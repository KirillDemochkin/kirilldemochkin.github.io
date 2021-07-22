---
layout: post
title:  "34: CVPR 2021 Best Paper - GIRAFFE Explained"
description: "Representing Scenes as Compositional Generative Neural Feature Fields by Michael Niemeyer et al. explained in 5 minutes."
date:   2021-7-8 19:00:00 -0000
categories: implicit-3d-aware-NeRF-GAN
---
  
#### Representing Scenes as Compositional Generative Neural Feature Fields by Michael Niemeyer et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![GIRAFFE: Representing Scenes as Compositional Generative Neural Feature Fields samples multi-object generation](/assets/images/add_clevr6.gif "GIRAFFE: multi-object generation")
![GIRAFFE: Representing Scenes as Compositional Generative Neural Feature Fields samples controlled translation](/assets/images/tr_d_cars.gif "GIRAFFE: controlled translation")
![GIRAFFE: Representing Scenes as Compositional Generative Neural Feature Fields samples controlled rotation](/assets/images/rotation_celebahq.gif "GIRAFFE: controlled rotation")

##### 🎯 At a glance:

If you thought GRAF did a good job at 3d-aware image synthesis just wait until you see the samples from this model by Michael Niemeyer and colleagues at the Max Planck Institute. While generating 256x256 resolution images does not sound that impressive in 2021, leveraging knowledge about the 3D nature of real world scenes to explicitly control the position, shape, and appearance of objects on the generated images certainly is exciting. So, did GIRAFFE deservedly win the best paper award at the recent CVPR 2021? Keep reading to find out!

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core contributions of this paper):*
1. [NeRF](https://t.me/casual_gan/22)
2. [GRAF]({% post_url 2021-7-2-GRAF %})

***

##### 🔍 Main Ideas:
**1) Object & Scene Representation:**
unlike GRAF and NeRF which represent the entire scene as a single radiance field GIRAFFE exploits the compositional nature of scenes, and models every object and the background as separate feature fields (a fancy term for a radiance field with more than 3 channels) conditioned on a shape and appearance code. Additionally each object has an affine transformation assotiated with it. The transformation is comprised of three parameters: scale, translation, and an SO(3) rotation matrix. These transforms are used to transition between object space and scene space.

**2) 3D Volume Rendering:**
All objects are composited via density-weighted mean into a single scene, where the background has a fixed scale and translation parameters. The neural rendering pipeline is the same as GRAF except that it renders a low resolution feature map instead of the high-res RGB image.

**3) 2D Image Rendering:**
The low resolution feature map from step 2 is processed by a convolutional network with upsampling and skip connections to produce the final image. The training procedure is similar to GRAF, and the object level transformations are sampled at each iteration (gravity is taken into account). All MLPs share their weights for all objects.

##### 📈 Experiment insights / Key takeaways:
- Several datasets from various domains are considered at 256 resolution
- Platonic-GAN, BlockGAN, HoloGAN, and GRAF are evaluated against (spoiler: GIRAFFE beats them all)
- Without any supervision strong scene disentanglement emerges. Objects are independent of the background and of one another.
- Shapes of object, their appearance, and position/rotation in scenes can be explicitly controlled
- Scenes generalize beyond training data: increased translation ranges, number of object, etc
- Dataset biases affect disentanglement

***

##### 🖼️ Paper Poster:

![CVPR 2021 best paper - GIRAFFE explained](/assets/images/GIRAFFE.png "GIRAFFE Paper Poster")

***

##### ✏️My Notes:
- (5/5) for the name of the model! Oh how long have I waited for a paper name like this 😍
- Sure, 256x256 images are nothing to drool over in 2021 but I think this paper's main contribution is the proof of concept for leveraging implicit 3d knowledge in 2d generative tasks
- I said it before, and I will say it again: I am mindblown that coherent 3d-ish scenes are possible to learn from a set of unposed 2d images
- I wonder what kept the authors from going to 512 or 1024 resolution. I assume it is computational complexity but it is not specifically mentioned in the paper

##### 🔗 Links:
[GIRAFFE arxiv](http://www.cvlibs.net/publications/Niemeyer2021CVPR.pdf) / [GIRAFFE github](https://github.com/autonomousvision/giraffe)

***

##### 👋 Thanks for reading!
*If you found this paper digest useful, subscribe and share the post with your friends and colleagues to support Casual GAN Papers!*

*Join [the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
