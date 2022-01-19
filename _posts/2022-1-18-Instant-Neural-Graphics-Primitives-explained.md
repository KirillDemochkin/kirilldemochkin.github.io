---
layout: post
title: "77: Instant Neural Graphics Primitives"
meta_title: "Instant Neural Graphics Primitives"
description: "Instant Neural Graphics Primitives with a Multiresolution Hash Encoding by Thomas Müller et al. explained in 5 minutes"
date: 2022-1-18 16:00:00 -0000
categories: fastest_nerf_3d_neural_rendering
---

#### Instant Neural Graphics Primitives with a Multiresolution Hash Encoding by Thomas Müller et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![Instant Neural Graphics Primitives](/assets/images/ingp_teaser.gif "Instant Neural Graphics Primitives Teaser")

##### 🎯 At a glance:

If you liked the 100x NeRF speed up from a month ago, you definitely will love this fresh new way to train NeRF 1000x faster proposed in a paper by Thomas Müller and the team at Nvidia that utilizes a custom data structure for input encoding that is implemented as CUDA kernels highly optimized for the modern GPUs. Specifically, the authors propose to learn a multiresolution hashtable that maps the query coordinates to feature vectors. The encoded input feature vectors are passed through a small MLP to predict the color and density of a point in the scene, NeRF-style.  

How does this help the model to fit entire scenes in seconds? Let’s learn!  

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [NeRF]({% post_url 2021-4-6-NeRF-explained %})

***

##### 🚀 Motivation:

For a long time, NeRF was expensive to train, hence much research was focused on reducing the necessary GPU time required to fit a scene. Despite iterative improvements, the issue remained largely unsolved, until the recent Plenoxels paper that showed how a simple grid structure with learned spherical harmonics coefficients can converge orders of magnitudes faster than the fastest NeRF alternative without quality degradation.  

This paper builds on the idea of using a grid-like structure to store learned embeddings that can be interpolated to obtain precise values for any coordinate. In the case of Instant Neural Graphics Primitives, this structure is a multiscale grid structure that looks up embeddings based on the query coordinates, which is done in O(1) time (read: instantly). Using these learned input embeddings allows the authors to shrink the MLP and make it more efficient, which was a bottleneck in other papers.  

The key insight of this whole paper, IMO, is that the entire pipeline is implemented in low-level CUDA code that is highly optimized for the modern GPUs, which is precisely what allows this method to work as fast as it does. The hashtable can be queried in parallel on all resolution levels for all pixels, and does not require any sort of control flow. Moreover, the cascading nature of the multiresolution grid enables the use of a coarse 1-to-1 mapping between entries and spatial locations, and a fine “hash-table-like” structure with a spatial function that performs collision resolution via …wait for it… gradient descent by averaging the colliding gradients, making the whole grid sparse by design. It is crazy how well all of the pieces of this approach fit together.  

***

##### 🔍 Main Ideas:

**1) Multi-resolution Hash Encoding:**  
The encoding structure consists of multiple hash tables of various resolutions arranged in geometric progression from coarse to fine. At each resolution level, we find the corners of the voxel that contains the query coordinate, retrieve their encodings, and interpolate them to obtain the set of feature vectors for the specified coordinate that is concatenated into a single input embedding. For the coarse levels, there is a unique coordinate for each vertex, whereas for fine levels, the number of embeddings is smaller than the number of vertices, and a spatial hash function is used to retrieve the necessary feature vector. It is possible to trade off speed vs quality by increasing the sizes of the hashtables at later resolutions.  

**2) Implicit Hash Collision Resolution:**  
Since the number of embeddings is smaller than the number of vertices some collisions are bound to happen. What can be done about that? Turns out, nothing, really. The collisions are automagically resolved via the multiscale approach since it is unlikely that collisions will happen across all levels simultaneously. And for the occurring collisions, the gradients get averaged, effectively, making the grid favor the dense parts of the scene since for points lying in visible areas of the scene the gradients will be larger than for the points in empty space.  

**3) Online Adaptivity:**  
The implicit collision resolution is a gift that keeps on giving. If the distribution of points changes to a concentrated area somewhere in the scene, the collisions will become rarer, and a better hashing function can be learned, effectively automatically adapting to the changing data on the fly without any task-specific structure changes. An example of this use-case is a dynamic scene with moving objects.  

**4) Implementation:**  
There is a number of engineering tricks used for optimization such as evaluating the resolution levels one by one, skipping gradient updates, where it is equal exactly to 0, using one-blob encoding or spherical harmonics in case a camera direction should be passed along with the query coordinates.  

##### 📈 Experiment insights / Key takeaways:

_Gigapixel Image Approximation:_  
- INGP achieves the same result as ACORN with a smaller model size and in 2.5 minutes instead of 36 hours.  
- Biggest advantage over ACORN is the simplicity of the encoding. Yeah right, and not the 730x speedup.  

_SDF:_  
- INGP approaches the fidelity of NGLOD without a dedicated data structure  

_Neural Radiance Caching:_  
- INGP runs at 133 fps in full-HD while training online  

_NeRF:_  
- Consists of two stacked MLPs, for density and view-dependent color  
- The density MLP has a single hidden layer, and the color MLP has 2  
- Scenes can be trained in seconds and rendered in 60fps without caching  
- Information from the coarse levels is used to make the ray marching more efficient by selecting the densest areas for querying  
- The MLP is just 15% more expensive than a single linear layer but captures specular effects better  


***

##### 🖼️ Paper Poster:

![Instant Neural Graphics Primitives poster](/assets/images/ingp.jpg "Instant Neural Graphics Primitives Poster")

***

##### 🛠 Possible Improvements:

- Experiment with reduction vs concatenation for the set of embeddings from different resolutions  
- Reduce the amount of the grainy “microstructure” that is the result of hash collisions  
- Make the hash function learnable  
- Make it generative!  

##### ✏️My Notes:

- (2/5) Such a missed opportunity on the name. Should have rearranged the words in the title to have a 0-PING NeRF. Props for the low-key flex in the title, it really nearly instant compared to everything else in the field  

- I mean, wow, this is just the kind of breakthrough that we needed to get 2022 started  
- Can’t wait to see more papers using this method in the near future  
- You are either already writing your own CUDA code or you just wait until this gets ported to PyTorch for easy of use  
- I need this in VR  

- What do you think about Instant Neural Graphics Primitives? Leave a comment, and let’s discuss!  

##### 🔗 Links:

[Instant Neural Graphics Primitives arxiv](https://nvlabs.github.io/instant-ngp/assets/mueller2022instant.pdf) / [Instant Neural Graphics Primitives Github](https://nvlabs.github.io/instant-ngp)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [100x Faster than NeRF - Plenoxels explained]({% post_url 2021-12-17-Plenoxels-explained %})
- [Infinite Video Generation - StyleGAN-V explained]({% post_url 2022-1-11-StyleGAN-V-explained %})
- [Guided Diffusion explained]({% post_url 2021-12-26-Guided-Diffusion-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
