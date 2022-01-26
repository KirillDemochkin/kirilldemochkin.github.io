---
layout: post
title: "78: Stitch it in Time"
meta_title: "Stitch it in Time"
description: "Stitch it in Time: GAN-Based Facial Editing of Real Videos by Rotem Tzaban et al. explained in 5 minutes"
date: 2022-1-26 16:00:00 -0000
categories: hiqh_quality_video_editing_stylegan_inversion
---

#### Stitch it in Time: GAN-Based Facial Editing of Real Videos by Rotem Tzaban et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![Stitch it in Time](/assets/images/stitch_teaser.gif "Stitch it in Time Teaser")

##### 🎯 At a glance:

What do you do after mastering image editing? One possible answer is to move on to video editing, a significantly more challenging task due to the inherent lack of temporal coherency in existing inversion and editing methods. Nevertheless, Rotem Tzaban and the team at The Blavatnik School of Computer Science and Tel Aviv University show that a StyleGAN is all you need. Well, a StyleGAN and several insightful tweaks to the to the frame-by-frame inversion and editing pipeline to obtain a method that produces temporally consistent high quality edited videos, and yes, that includes CLIP-guided editing. With the overview part out of the way, let’s dive into the details.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [e4e]({% post_url 2021-4-6-NeRF-explained %})
2) [PTI]({% post_url 2021-6-29-PTI %})

***

##### 🚀 Motivation:

Over the last year (2021) tremendous progress was made in the GAN-based image editing deparment, in particular, leveraging the highly disentangled and semantically meaningful latent space of pretrained StyleGAN2 generators. Yet, applying the same techniques to video editing proved to be far from trivial, mostly due to to the additional constraint of maintaining temporal coherence, meaning the edited frames should have the same edited face and smooth transitions without any flickering between frames. While the recently proposed StyleGAN-V is able to generate coherent videos at a high framerate, it struggles with high resolution synthesis and requires a specialized dataset.  

Here the authors state that video editing only requires maintaining temporal consistency, not creating it since the original video always exists, and share two key insights that led them to the impressive results shown off in the paper. First, generators tend to learn low frequency features, which is typically brought up as a weakness, but is surprisingly used here as an advantage to maintain the consistency of frames edited with slightly varying latent codes. Second, finetuned generators can be finetuned without breaking their latent space, which helps to produce smoothly changing editing offsets for a set of smoothly changing latent codes.  

Vanilla PTI fails to produce smoothly changing inversion latent codes for each frame, hence the flickering, whereas an encoder-based inversion is subject to the low-frequency bias, hence it produces a more stable set of latent codes for the input video frames.  

***

##### 🔍 Main Ideas:

**1) Alignment:**  
A gaussian filter is applied to detected landmarks before cropping and aligning faces from video frames to reduce sensitivity to the exact locations of facial landmarks and improve temporal consistency down the line.  

**2) Inversion:**  
The cropped and aligned face are inverted with PTI, with the exception that the optimization-based inversion in PTI is replaced with a e4e encoder. This is required to fix discrepancies between the “pivots” that fall too far from one another since the e4e encoder is more focused on the low frequency features that do not change as much between different frames and provide a smoother transition for the style codes of adjacent frames. Additionally, PTI is applied to all latent codes for each frame simultaneously.  

**3) Editing:**  
Luckily, off-the-shelf linear editing techniques work in a drop-in manner, and do not require any changes to produce temporally consistent edits for smoothly changing inverted style codes.  

**4) Stitching Tuning:**  
Simply pasting the edited face back into the original frame leads to undesirable artifacts such as ghostly outlines around the border, hence a second tuning is performed. The process is as follows: obtain a segmentation mask for the edited face using a pretrained segmentation network, dilate the mask to create the boundary region, paste the edited face into the original frame and finetune the generator for each frame to make the boundary region more similar to the original image without changing the edited face.  


##### 📈 Experiment insights / Key takeaways:

- Stitch it in time works on out-of-domain videos and challenging scenes  
- Stitch it in time achieves higher TL-ID and TG-ID (both metrics measure temporal consistency in a video in terms of local jitter and identity shifts between frames) than PTI and Latent Transformer  
- Editing is done with InterFaceGAN and StyleCLIP  
- PTI performs considerably worse in terms of the global score compared to the local score  

- Ablations (verbatim from the paper): Without an encoder, the edited frames become inconsistent when the face undergoes considerable movement or changes in expression. Without PTI, frames are less faithful to the original video, stitching performance suffers, and identity changes over longer time periods. Without stitching, artifacts appear around the hair and borders of the segmentation mask (i.e. the edge of the face)  

***

##### 🖼️ Paper Poster:

![Stitch it in Time poster](/assets/images/stitch.jpg "Stitch it in Time Poster")

***

##### 🛠 Possible Improvements:

- The biggest one is speed. Unfortunately, the method does not work in realtime, which would be perfect for real world applications. Alas, a single 5 sec video takes 1.5 hours per edit  
- Just remixing papers from the authors, but I assume HyperStyle + Stitch it in Time is an obvious next step  
- Hair sometimes gets cropped incorrectly and ends up outside the edited region  
- Fix texture sticking  

##### ✏️My Notes:

- (4/5) A fun paper name, that could have totally used an acronym to refer to the proposed method instead of the canonical ‘ours’  

- Another great paper by the brains behind the recent HyperStyle and PTI  
- Really wish this worked in real time, this sounds like the perfect thing to use during zoom calls  
- Shoutout to whoever got to pick the office gifs to show off the results, you have a fine taste in comedy!  
- I am very intrigued, what the next couple of papers from this lab will be (beside the obvious “fast video editing follow up”), something in 3D maybe?  

- What do you think about Stitch it in Time? Leave a comment, and let’s discuss!  

##### 🔗 Links:

[Stitch it in Time arxiv](https://arxiv.org/abs/2201.08361) / [Stitch it in Time Github](https://github.com/rotemtzaban/STIT)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Train a NeRF in Seconds - Instant Neural Graphics explained]({% post_url 2022-1-18-Instant-Neural-Graphics-Primitives-explained %})
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
