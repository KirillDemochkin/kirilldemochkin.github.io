---
layout: post
title: "82: FILM"
meta_title: "FILM"
description: "FILM: Frame Interpolation for Large Motion by Fitsum Reda et al. et al. explained in 5 minutes"
date: 2022-2-18 16:00:00 -0000
categories: motion-interpolation-image-animation-implicit-model
---

#### FILM: Frame Interpolation for Large Motion by Fitsum Reda et al. et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![FILM Model](/assets/images/film_preview.gif "FILM Teaser")

##### 🎯 At a glance:

Motion interpolation between two images surely sounds like an exciting task to solve. Not in the least, because it has many real-world applications, from framerate upsampling in TVs and gaming to image animations derived from near-duplicate images from the user’s gallery. Specifically, Fitsum Reda and the team at Google Research propose a model that can handle large scene motion, which is a common point of failure for existing methods. Additionally, existing methods often rely on multiple networks for depth or motion estimations, for which the training data is often hard to come by. FILM, on the contrary, learns directly from frames with a single multi-scale model. Last but not least, the results produced by FILM look quite a bit sharper and more visually appealing than those of existing alternatives.

As for the details, let’s dive in, shall we?

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
Relax! 

***

##### 🚀 Motivation:

The authors identify three significant vectors of improvement over existing baselines: using a single unified model that learns in an end-to-end fashion, handling large motion between frames, improving image quality for the interpolated frames. More specifically, FILM does not rely on any additional priors, such as depth estimation for occlusion detection or motion estimation from an external model. Hence, FILM consists of just a single module that achieves state-of-the-art results. Moreover, existing models struggle to generalize to novel data when the range of motion differs from the training data, and the authors use a multi-scale approach to learn both large and small motion. Finally, instead of using a more obvious adversarial (GAN) loss, the team picked the Gram matrix loss, which is an evolution of everyone’s favorite perceptual loss.

***

##### 🔍 Main Ideas:

The model consists of three distinct sequential modules.  

**1) The Feature Extractor:**  
is a convolutional encoder that processes a 7-scale input image pyramid. The crux of the method is in the way the weights are shared between scales, and how the extracted features are stacked across scales in a grid-like manner. The same encoder is applied to each scale, and the resulting features are concatenated with the other feature maps of the same spatial size from all scales in a clever twist on the sliding window idea. Interestingly, the resulting stacks have a different number of feature maps in them. For example, the first stack will have just the output of the first layer of the encoder applied to the largest scale image, the second stack will have the outputs of the second layer of the encoder applied to the largest scale, and the feature map from the first layer of the encoder applied to the second-largest scale image, and so on. This part is applied to both input frames, to obtain two feature pyramids.  

**2)The Flow Estimator(this part is awesome but slightly tricky, so pay attention!):**  
the driving idea of this module is that in order for the large motion to work in frame interpolation the motion flow should be similar across scales. How can we ensure this? The authors take a recursive approach by computing the flow of each pyramid level as the sum of the upscaled flow from the next smaller (coarser) level a predicted residual vector. This vector is computed with a small convolution model that takes in the input feature map for one frame and the feature map extracted from the second frame that is backward warped using the same upsampled estimate of the flow from the next smaller scale. (and no, this is not a leaked synopsis of the tenet sequel). Note that this process is repeated for the other input frame as well.  

**3) The Fused Decoder:**  
finally, a UNet like decoder is used to upsample and concatenate warped features of matching spatial dimensions from both frames to predict the intermediate frame. Note that this process can be done recursively, to insert more frames between the input and generated frames.  

**4) Loss Functions:**  
One, two, three, no GANs to see. Anyways, the authors use a simple combination of L1, perceptual, and Gram matrix losses. With the later computed as the L2 norm of the autocorrelation matrix of VGG-19 features from various layers. Think of it as the more snobbish perceptual loss. Apparently, this loss helps significantly with inpainting disocluded areas of the image.  

##### 📈 Experiment insights / Key takeaways:

- Baselines: DAIN, AdaCoF, BMBC, SoftSplat, ABME  
- Datasets: Vimeo-90k, UCF101, Middlebury, Xiph  

- Kind of a cheat here, the reported results say that quantitatively ABME outperforms FILM by a small margin (which is still an impressive result, considering all of the additional data and external modules the other models have), but! FILM is better when trained with perceptual quality in mind.  
- FILM puts all other models to shame (qualitatively) on the large motion dataset Xiph  
- FILM produces sharper images, without ghosting, and with better-preserved structure  
- Ablations look good  

***

##### 🖼️ Paper Poster:

![FILM poster](/assets/images/film.jpg "FILM Poster")

***

##### 🛠 Possible Improvements:

- Reduce the number of artifacts in images (Lame)  

##### ✏️My Notes:

- (Naming: 5/5) An excellent example of a mode/paper name, catchy, easy to pronounce, thematically relevant, it has it all! The name is honestly on the level of CLIP in terms of catchiness.  
- (Paper UX: 4/5) The math is surprisingly easy to follow, the big diagram with the model architecture is a great visual aid, and I especially love the color-coding to show, which parts of the model share weights. Additionally, the diagram has this almost skeuomorphic feel to it, very stylish! The samples all have zoom-in right next to the big image, which is important, since it is not always obvious, what part of the image to pay attention to when studying the qualitative results. One improvement I would suggest is to give a “zoomed-in” view of the flow estimation module, because it was not immediately clear to me how the math for backward flow estimation connected with the big diagram.  
- All of the figure and table descriptions are self-sufficient, which is essential for the first glance-through read of the paper. That is it for the visual aspect of the paper, and as for the text, I appreciate the clearly defined contributions at the end of section one, and how section two elaborates on each of the main contributions. This is vital to give the reader the necessary context to understand, the goal of the paper, and how each architecture design decision helps to achieve this goal. Now for some criticism on the text: the future work section is total BS (which is a shame, as it is one of the best ways to get traction for your paper, IMO). Moreover, I think the ablations section is pretty lazy. Yes, the authors check every part of their pipeline, but what is the point of taking up the valuable page space if all you are going to say is that each part of the model significantly contributes to the final result. Obviously, the parts that didn’t contribute got cut out of the final model. At least provide some conjectures as to why the contribution is significant, or just leave a table with the results and summarize them in a single paragraph.  

- I wonder, how this would work with a GANsformer, where it would generate the layouts for frames, and then FILM would be used to animate them  
- Honestly, I could not think of a way to incorporate CLIP into FILM, write your suggestions in the comments!  
- I wonder if it is somehow possible to use two frames from different videos so that the interpolated frame is the first frame, driven by the second frame.  

##### 🔗 Links:

[FILM arxiv](https://arxiv.org/pdf/2202.04901.pdf) / [FILM Github](https://github.com/google-research/frame-interpolation)

##### 💸 If you enjoy my posts, consider donating ETH here to support the website:  
0x7c2a650437f58664BED719D680F2BE120bE5623b

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Improved VQGAN - MaskGIT explained]({% post_url 2022-2-18-MaskGIT-explained %})
- [Is StyleGAN Good? - Third Time is the Chart explained]({% post_url 2022-2-2-Third-Time-Is-The-Charm-explained %})
- [Best Facial Animation Tool - First Order Motion Model]({% post_url 2022-2-10-First-Order-Motion-Model-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
