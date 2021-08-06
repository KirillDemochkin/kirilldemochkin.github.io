---
layout: post
title:  "43: 3D Inpainting Explained"
meta_title: "3D Inpainting Explained"
description:  "3D Photography using Context-aware Layered Depth Inpainting by Meng-Li Shih et al. explained in 5 minutes."
date: 2021-8-6 17:00:00 -0000
categories: single_view_layered_depth_3d_inpainting
---

#### 3D Photography using Context-aware Layered Depth Inpainting by Meng-Li Shih et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![3D Photography using Context-aware Layered Depth Inpainting Samples](/assets/images/3dinpainting_teaser.gif "3D Inpainting teaser")

##### 🎯 At a glance:

Is it possible to create 3d photos with convincing parallax effects from single RGB-D images? It is now! Check out a new 3D inpainting method proposed by Meng-Li Shih and colleagues. In short, the input image is transformed into a Layered Depth Image with explicit pixel connectivity, which is used to synthesize new local color-and-depth content into the occluded regions in a spatial context-aware manner. The resulting images can be rendered with a smooth parallax effect using standard graphics engines with fewer artifacts compared to current SOTA methods

##### ⌛️ Prerequisites:

**(Check these out if you have extra time):**  
1) [MPI](https://single-view-mpi.github.io/)  
2) [LDI](https://grail.cs.washington.edu/projects/ldi/)

***

##### 🚀 Motivation:
3D photos are more immersive than 2D, especially in VR. However, complex hardware setups are required to produce such images, and current methods that synthesize 3D photos from images captured with multi-lens smartphone cameras either produce gaps or distortions in the regions, occluded in the input image. Recent methods used Multi-Plane Image representation to address these issues, however they tend to produce artifacts on sloped surfaces. Instead of using rigid layers such as in Layered Depth Images (LDI), the authors explicitly store pixel connectivity and recursively apply CNN-based inpainting conditioned on spatially-adaptive context regions that are extracted from local connectivity in the LDI. The result is an algorithm for 3D photo generation without a predetermined number of depth layers.

***

##### 🔍 Main Ideas:

**1) Layered Depth Image:**  
An LDI is an image that can have any number of pixels in each position on the pixel grid. The LDI considered in the paper explicitly stores for each pixel up to one pointer to a neighboring pixel (the neighbors are not shared across layers). The proposed method takes an aligned color-and-depth image pair (RGB-D) and generated an LDI with inpainted color and depth in the regions that were occluded in the input.

**2) Image Preprocessing:**  
Before the main algorithm does its magic, the input image is used to initialize a 1-layer LDI with all pixels interconnected. Meanwhile, the depth map is sharpened to better identify depth discontinuities based on the disparity of neighboring pixels. Detected discontinuities are merged into  a collection of "linked depth edges" (basically the edges around large objects in the image), and small/disconnected discontinuities are discarded.

**3) Context and Synthesis Regions:**  
Next, the algorithm processes the computed depth edges one at a time in the following manner: first, pixel along an edge are disconnected in the LDI, then the context and the occluded synthesis regions are formed. The inpainting is local, hence the context region is bound by other depth edges (only pixels from the current layer are used for inpainting), and the synthesis region is needed for background only (relative to the layer along the depth edge that is in front). The synthesis region is initialized with a flood-fill algorithm from the context region. The synthesis region is dilated by 5 pixels near the edge to compensate for imperfect depth estimation.

**4) Context-Aware Color and Depth Inpainting:**  
The inpainting is done by 3 sub-networks: edge inpainting network that inpaints the edges in the synthesis region based on the context region, a color, and a depth networks that inpaint the color and depth respectively based on the context and the outputs from the depth edge inpainting network. This step is repeated for each edge until no edges remain. The entire model can be applied multiple times for better inpainting on complex images. The resulting inpainted depth and color can be integrated into the original LDI and represented as a 3D textured mesh.

##### 📈 Experiment insights / Key takeaways:

- Inpainting networks are UNet-based
- The model is trained on COCO with predicted depth values
- The model outperforms PB-MPI, StereoMag, LLFF, and Facebook 3D Photo
- It is not sensitive to the source of the depth maps
- SSIM is almost as good as StereoMag, PSNR, and LPIPS are better slightly for novel view synthesis

***

##### 🖼️ Paper Poster:

![3D Photography using Context-aware Layered Depth Inpainting paper poster](/assets/images/3dinpainting.png "3D Inpainting Paper Poster")

***

##### ✏️My Notes:

- No name to rate :(
- There seem to be a bunch of hard-coded/heuristic-based parts of the algorithm that I think can be replaced with MLPs or some other simple learnable modules for improved quality
- Add view dependent effects
- implicit continuous layers (NeRF-lite)?
- Add deferred neural rendering
- Have you tried 3D inpainting? Let me know in the comments!

##### 🔗 Links:
[3D Inpainting arxiv](https://arxiv.org/abs/2004.04727) / [3D Inpainting github](https://github.com/vt-vl-lab/3d-photo-inpainting.git)

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
