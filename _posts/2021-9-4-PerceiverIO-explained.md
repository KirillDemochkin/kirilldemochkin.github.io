---
layout: post
title:  "51: PerceiverIO"
meta_title: "PerceiverIO"
description:  "Perceiver IO: A General Architecture for Structured Inputs & Outputs" by Andrew Jaegle et al. explained in 5 minutes."
date: 2021-9-4 19:00:00 -0000
categories: cross-modal-fully-attentional-transformer
---

#### Perceiver IO: A General Architecture for Structured Inputs & Outputs" by Andrew Jaegle et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![PerceiverIO Samples](/assets/images/perceiverio_teaser.png "PerceiverIO teaser")

##### 🎯 At a glance:

Real-world applications often require models to handle combinations of data from different modalities: speech/text, text/image, video/3d. In the past specific encoders needed to be developed for every type of modality. Moreover, a third model was required to combine the outputs of several encoders, and another model - to transform the output in a task-specific way. Now thanks to the effort of the folks at DeepMind we now have a single model that utilizes a transformer-based latent model to handle pretty much any types and sizes of input and output data. As some would say: is attention all you need?
Let's find out!

##### ⌛️ Prerequisites:

**(Check these out if you have extra time):**  
1) [Transformer](https://arxiv.org/abs/1706.03762)

***

##### 🚀 Motivation:

There exists a transformer-based model called Perceiver that is able to handle various modalities of input data with minimal changes to the architecture. It, however, only works for simple output tasks like classification. The authors of PerceiverIO extend the fully-attentional network to handle various types of output tasks by adding a task-specific query to attend to the latent space representation, and decode it to a structured output such as an image, optical flow, language, etc. The key insight is that a task specific query based on the required shape and structure of the output data along with a learned task embedding allows PerceiverIO to perform most of the heavy processing in a compressed latent space agnostic to the types and sizes of input and output data.

***

##### 🔍 Main Ideas:

**1) Perceiver Architecture:**  
There are three main parts to the Perceiver architecture. First is the attention based encoder that uses input data to form keys and values, and a learned set of queries that have size independent from the dimensions of the input data to map the input data to a smaller latent space, which allows Perceivers to scale to much larger sizes of the input data since there is no quadratic memory dependence on input size. Second is the processing module, which processes the latent vectors using Transformer-style attention and MLP blocks. Finally, the output mapper uses hand-built or learned (or some combination of the two) query vectors that map the latent vectors to the desired output domain of specific size.

**2) Encoding, Processing, and Decoding:**  
The processing modules all apply a query-key-value attention followed by an index-independent MLP. Since the Transformer computes attention between all input elements the operation quickly becomes prohibitively memory-expensive as the size of the input grows. On the other hand, since Perceiver always maps the input to a latent set of vectors with fixed size, it side-steps this issue altogether, and works with inputs and outputs that have any shape or spatial layout, even if it is not the same for both.

**3) Decoding the latent representation with a query array:**  
The output query should reflect the downstream task, hence it is constructed by combining a set of vectors containing all of the information for the output at a given position. Queries can be built learned from scratch for simple tasks, built from positional encoding for spatially dependent tasks, created from task and modality specific learned queries for multi-modal/multi-task settings. For tasks like flow estimation, where the output should reflect the content of the input at the target location.
  
##### 📈 Experiment insights / Key takeaways:

- PerceiverIO outperforms BERT on SentecePiece, and scales much better than Transformer on longer sequences.
- PerceiverIO beats PWCNet and RAFT on Optical Flow prediction for Sintel and KITTI
- Perceiver reaches 45% top-q accuracy and 20.7 PSNR for multimodal autoencoding results on Kinetics 700 video classifier
- PerceiverIO does wonders on StarCraft II when used in place of the standard Transformer inside AlphaStar as it reaches the same level of performance as the original AlphaStar with latent size of just 32 vs 512
- PerceiverIO achieves the best results on ImageNet from any model without 2D architectural or feature information
- Possible improvements: auto-tune the latent space size, increase resistance to adversarial attacks and bias in the training data

***

##### 🖼️ Paper Poster:

![PerceiverIO paper poster](/assets/images/perceiverio.png "PerceiverIO Paper Poster")

***

##### ✏️My Notes:

- (4/5) - Not a meme, but still a great creative name!
- This seems to be a model that has many non-obvious applications
- Seems like this is an alternative approach to [GANsformer's duplex attention](https://t.me/casual_gan/14) for solving the quadratic memory requirement
- It is a shame that there is no training code in PyTorch 😕
- What do you think about PerceiverIO? Let's discuss in the CGP Chat!

##### 🔗 Links:
[PerceiverIO arxiv](https://arxiv.org/pdf/2107.14795.pdf) / [PerceiverIO github](https://github.com/deepmind/deepmind-research/tree/master/perceiver)

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
