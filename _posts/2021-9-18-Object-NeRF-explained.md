---
layout: post
title: "55: Object-NeRF"
meta_title: "Object-NeRF"
description: "Learning Object-Compositional Neural Radiance Field for Editable Scene Rendering by Bangbang Yang et al. explained in 5 minutes."
date: 2021-9-18 16:00:00 -0000
categories: мulti-оbject-NeRF-3D-scene-editing
---

#### Learning Object-Compositional Neural Radiance Field for Editable Scene Rendering by Bangbang Yang et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![Object-NeRF teaser](/assets/images/objectnerf_teaser.gif "Object-NeRF Teaser")

##### 🎯 At a glance:

NeRF models have come a long way since the initial “explosion” last year. Yet one of the things they still can’t quite handle is scene compositionality, meaning that the model is not aware of the distinct objects that make up the scene. Object NeRF aims to tackle this issue using a dual-branch model that separately encodes the global context of the scene and each object in it. This approach not only reaches competitive levels of quality with current SOTA methods on static scenes, it also enables object-level editing. For example, adding or moving furniture in a real world scene.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [NeRF]({% post_url 2021-4-6-NeRF-explained %})

***

##### 🚀 Motivation:

Existing models either use a single model per scene (NeRF, SRN) or a volumetric representation that is agnostic to object identity, which is a severe limitation in settings, where scene editing is desired (e.g. virtual tours). Unlike OFS that does not learn object placement from real world images, Object-NeRF uses a novel conditional neural rendering architecture that is able to render each object standalone with everything else removed, which can be further rendered from a novel viewpoint, at a new location, or replicated. Additionally, a clever training scheme using 2d instance segmentation masks lets Object-NeRF learn to render complex scenes from a collection of real world posed 2d images.

***

##### 🔍 Main Ideas:

**1) Overview:**  
Object NeRF consists of two submodules: the scene branch that renders the background and provides global context for the occlusion regions and the object branch that encodes each object in the 2D instance mask conditioned on several learnable latent codes. The network is able to simultaneouslt earn to encode multiple objects by assigning shuffled object activation codes to the training rays.

**2) Framework of Object-Compositional NeRF:**  
For each sampled point positional encoding is applied to the voxel feature interpolated from the 8 closest vertices as well as the spatial coordinate. This along with a viewing direction is fed into the scene branch MLP that outputs the opacity and color at x. The object branch additionally receives a shared embedded object voxel feature and a unique per-object activation code as a condition and renders only the object, while leaving everything else blank.

**3) Object-Compositional Learning:**  
The goal of the training procedure is to produce object radiance fields that are completely empty everywhere except for the location of the object. To enforce this authors assign a code from the object activation code library to each ray in a training batch and compute a loss between instance segmentation mask for that object and the sum of the object opacity along the rays. Additional losses include L2 between the GT and rendered colors for each object. Ambiguos occlusions when objects in the foreground are blocking other objects behind them are handled by leveraging information from the scene branch to perform scene guidance (biased ray sampling).

This is not enough in more advanced cases, hence a 3D guard mask is constructed by pushing scene depth rendered by the scene branch slightly forward along the camera viewing direction and blocking out the rays in the areas “behind” the objects.

You can think of this as a paper diorama with all objects as paper cutouts in front of one another. When you shun a light at the diorama, the objects will cast shadows on parts of other objects behind them. The shadow parts are the ones blocked by the 3D guard mask in Object-NeRF.

The intuition behind this idea is that gradient should be blocked in occluded regions, and the object branch will render just the object and nothing else.

**4) Editable Scene Rendering:**  
To edit Object-NeRF scenes simply render the scene branch without the rays in the edited region to remove whatever objects might already be there. Then pick whichever object code you want from the learned library, render it, and manipulate the color and opacity in whatever way you want.

##### 📈 Experiment insights / Key takeaways:

- All considered models are evaluated on a custom ToyDesk set of scenes and on the ScanNet dataset
- In terms of SSIM, PSNR and LPIPS Object-NeRF does slightly better or almost the same as NeRF, NSVF, and NPCR on both datasets.
- Qualitatively Object-NeRF renders scenes with finer granularity and better texture when voxels are missing
- Object-NeRF can cleanly render separate objects without the rest of the scene unlike other baselines
- Scene editing is miles ahead for Object-NeRF compared to the rest of the models
- Results from ablations check out
- Rendered objects have smooth edges despite rough input segmentation masks


***

##### 🖼️ Paper Poster:

![Object-NeRF paper poster](/assets/images/objectnerf.png "Object-NeRF Paper Poster")

***

##### 🛠 Possible Improvements:

- Adding learnable parameters in 3D space can also broaden the network ability for compositional rendering
- It is promising to integrate the scene lighting model into the framework for more realistic editing
- Adding scene completion models can help render unseen textures under objects

##### ✏️My Notes:

- Name rating - (2/5) What a shame, this paper missed such a good opportunity to coin the DONeRF (Decoupled Object NeRF) model name and win the 2021 CGP meme name awards. Instead, it is Object NeRF ☹️.
- For some reason this paper was a bit of a tough read. I found myself reading sections over and over feeling like I am missing some key information only to realize that the authors name-dropped a variable that is defined in a later section.
- I think it is time to add a new section here: Can {X} be CLIP-ed? For object NeRF the obvious way to add CLIP is to use it for text-based scene edits instead of manually manipulating each object.
- I very much like the trend of using multiple smaller NeRFs to compose a scene instead of a single big NeRF. Reminds me of the movie “Aliens” (sequel to the 1979 classic “Alien”)
- Interesting how this does against GRAF since the ideas are very similar
- Is there such a thing as NeRF inpainting?
- Overall I love this project, this is definitely moving AR/VR in the right direction. I want to see now how this model can be adapted to handle objects added from a different scene.
- Have you tried Object-NeRF? Let’s discuss in the comments!

##### 🔗 Links:
[Object-NeRF arxiv](http://www.cad.zju.edu.cn/home/gfzhang/papers/object_nerf/object_nerf.pdf) / [Object-NeRF github](https://github.com/zju3dv/object_nerf)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [SOTA NLP model for everything - FLAN]({% post_url 2021-9-11-FLAN-explained %})
- [The second half of VQGAN + CLIP for text-image similarity - CLIP]({% post_url 2021-9-14-CLIP-explained %})
- [SOTA - Robust Video Matting]({% post_url 2021-9-7-Robust-Video-Matting-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
