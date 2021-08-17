---
layout: post
title:  "46: Paint Transformer"
meta_title: "Paint Transformer"
description:  "Paint Transformer: Feed Forward Neural Painting with Stroke Prediction by Songhua Liu et al. explained in 5 minutes."
date: 2021-8-17 22:00:00 -0000
categories: image-to-painting-visual-transformer-stroke-prediction
---

#### Paint Transformer: Feed Forward Neural Painting with Stroke Prediction by Songhua Liu et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***
Paint            |  Transformer | Samples  
:-------------------------:|:-------------------------:|:-------------------------:
![Paint Transformer Samples Bird](/assets/images/painttransformer_teaser_1.gif "Paint Transformer teaser bird") | ![Paint Transformer Samples Boat](/assets/images/painttransformer_teaser_2.gif "Paint Transformer teaser boat") | ![Paint Transformer Samples Seagull](/assets/images/painttransformer_teaser_3.gif "Paint Transformer teaser seagull")

##### 🎯 At a glance:

After seeing Paint Transformer gifs for two weeks now all over Twitter, you know, I had to cover it. Anyways, Songhua Liu et al. present a cool new model that can "paint" any image, and boy, the results are PRETTY. The painting process is an iterative method that predicts parameters for paint strokes in a coarse-to-fine manner, progressively refining the synthesized image. The whole process is displayed as a dope painting time-lapse video with brush strokes gradually forming an image.

##### ⌛️ Prerequisites:

**(Check these out if you have extra time):**  
1)[Transformer](https://arxiv.org/abs/1706.03762)
1)[DETR](https://arxiv.org/abs/2005.12872)

***

##### 🚀 Motivation:

Authors suggest that the best neural paintings are obtained by generating a series of strokes to imitate how a human artist would create a painting, and generating stroke sequences turns out to be a challenging task. RL-based approaches tend to be unstable, and optimization methods require too much compute time and resources. Hence, the aim is to train a feed-forward stroke predictor. Paint Transformer is inspired by DETR, alas without any training data. The authors propose a self-training pipeline that only uses synthesized stroke images. As a result, Paint Transformer works on any natural mage at better quality and lower cost compared to existing methods.

***

##### 🔍 Main Ideas:

**1) Overall Framework:**  
The model consists of two blocks: a trainable Stroke Predictor (SP) and a parameter-free differentiable Stroke Renderer (SR). The two submodules work in tandem with SP generating a set of stroke parameters, an SR drawing the corresponding strokes on the canvas.  
During training, a set of random background (BGS) and foreground (FGS) strokes are sampled. Their composition becomes the Target Image, SP predicts FGS from BGS & Target Image, and SR generates the final prediction from BGS and the predicted FGS. This way the loss is computed on both stroke and pixel levels.

**2) Stroke Definition and Renderer:**  
A stroke is represented with a shape (center coordinate, width, height, rotation) and a color (r, g, b). The stroke renderer simply does alpha composes the strokes from a primitive brush according to the given parameters.

**3) Stroke Predictor:**  
SP is a Transformer model that takes the current canvas and the target image and for each patch of a predetermined size outputs stroke parameters along with a stroke confidence. The strokes are filtered based on confidence. This filtering is approximated with a sigmoid function for back propagation.

**4)  Loss Function:**  
The model is trained with an L1 pixel loss, L1 loss on predicted stroke parameters, Wasserstein distance between two strokes (a measure of the minimum cost needed to transform one stroke into the other), To compute the optimal permutation of strokes a bipartite matching is computed between the target and predicted strokes. The order of strokes is regulated at pixel-level.

**5) Inference:**  
At inference-time multiple coarse-to-fine steps are takes with progressively smaller patch sizes for SP. The resulting image from the previous step is used as the input for the next step. The strokes are rendered in parallel.
  
##### 📈 Experiment insights / Key takeaways:

- Paint Transformer (PT) is trained with patch-size 8, 3 conv blocks and two downscaling operations for feature extraction and 3 layers of size 256 for the Transformer. Random stroke parameters are sampled from a normal distribution during training. They are filtered to prevent too much overlap. Training is done in under 4 hours on a single 2080Ti
- PT generates images with clearer texture than optim and more vivid results than RL
- Ablations: no pixel loss -wrong colors, and dirty textures, no L1 loss - repeated stroke patterns, no wasserstein loss - no strokes of different scales, no confidence loss - too many small strokes
- It is possible to combine various brushes, and stylize paintings

***

##### 🖼️ Paper Poster:

![Paint Transformer paper poster](/assets/images/painttransformer.png "Paint Transformer Paper Poster")

***

##### ✏️My Notes:

- (4/5) Really good names lately!
- I love how a lot of the image manipulation models can be stacked like LEGO blocks to create cool pipelines
- The inference gifs are mesmerizing
- Have you tried Paint Transformer? Let's discuss!

##### 🔗 Links:
[Paint Transformer arxiv](https://arxiv.org/pdf/2108.03798.pdf) / [Paint Transformer github](https://github.com/huage001/painttransformer)

***

##### 👋 Thanks for reading!
<
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
