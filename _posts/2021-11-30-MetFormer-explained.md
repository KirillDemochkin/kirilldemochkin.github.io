---
layout: post
title: "71: MetaFormer"
meta_title: "MetaFormer"
description: "MetaFormer is Actually What You Need for Vision by Weihao Yu et al. explained in 5 minutes"
date: 2021-11-30 19:00:00 -0000
categories: vision-transformer-meta-architecture-sota-imagenet-pretraining
---

#### MetaFormer is Actually What You Need for Vision by Weihao Yu et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![MetaFormer](/assets/images/metaformer_teaser.png "MetaFormer Teaser")

##### 🎯 At a glance:

Unless you have been living under a rock for the past year you know about the hype beast that is vision Transformers. Well, according to new research from the team at the Sea AI Lab and the National University of Singapore this hype might be somewhat misattributed. You see, most vision Transformer papers tend to focus on fancy new token mixer architectures, whether self-attention or MLP-based, however, Weihao Yu et al. show that a simple pooling layer is enough to match and outperform many of the more complex approaches in terms of model size, compute, and accuracy on downstream tasks. Perhaps surprisingly, the source of Transformers’ magic might lie in its meta-architecture, whereas the choice of the specific token mixer is not nearly as impactful!

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [ViT]({% post_url 2021-4-30-ViT-explained %})  

***

##### 🚀 Motivation:

In a short span of time Transformers have already proved to be a powerhouse for large scale pretraining in vision tasks. Yet the question remains, what exactly gives them this edge over other approaches? So far the common belief was that the various token mixers that propagate information between tokens were the key to success. Over the last year, a multitude of such mixers was developed, starting with the classic self-attention and onto spatial MLP modules. Upon closer inspection, it became apparent that whatever token mixer the model used internally, as long as the overall architecture was Transformer-like it was successful. The authors of this paper ask the burning question of how much this MetaTransformer can be abstracted from the choice of its specific token mixer without getting kicked in the teeth on downstream tasks. What a revelation it must have been that even an “embarrassingly simple”, as the team calls it, pooling layer matches and in many cases surpass many of the much more complicated token mixers. Apparently, this whole time the Vision Transformers research was focused on the wrong part of the pipeline. Could it really be this bad?  

Let’s go over the main ideas of the authors’ approach and their definition of MetaFormer

***

##### 🔍 Main Ideas:

**1) MetaFormer:**  
The proposed architecture of the MetaFormer consists of an input embedding that projects the input tokens into the “embedding” space. The embedding tokens are passed through a series of repeated MetaFormer blocks that consist of two subblocks with residual connections - a token mixer with normalization and an MLP with a nonlinear activation. The purpose of the token mixer is, again, to communicate information across tokens, although some designs allow information to pass between channels as well. Token mixers are typically implemented as attention or as spatial MLPs.  

**2) PoolFormer:**  
Next, let’s look at a super simple MetaFormer implementation, where the token mixing is implemented as a pooling operation that replaces each token with the average value of its neighbors inside the pooling window (basically average pooling with the input subtracted to account for the residual connections). Unlike self-attention and spatial MLPs that are quadratic in complexity and heavy on memory use, pooling has no learnable parameters and works in linear time respective to the number of input tokens. The PoolFormer has a varying number of blocks at different scales with intermittent patch embeddings to reduce the spatial size of the input for a total of 5 variants from smallest to largest.

##### 📈 Experiment insights / Key takeaways:

- Baselines: DeiT, ResMLP, ViT, PVT, Swin-Mixer (what an awesome name, btw)  
- For numerical results see the attached visual summary  

- PoolFormer has about 30% fewer MACs (number of basic operations like addition and multiplication) than the next best attention model (DeiT-S) and 67% fewer MACs and 43% less parameters than the next best MLP model (ResMLP-S24)  
- Even at 300 epochs improved ResNet still can’t surpass PoolFormer  
- PoolFormer outperforms the baselines in object detection and semantic segmentation tasks as well  

- Ablations: pool size - 3 has same performance as 5 and 7 and slightly better than 9; Group Norm performs better than Layer Norm an Batch Norm; GELU > ReLU or SiLU  
- It is possible to use pooling in the first blocks of the MetaTransformer and a more complex token mixer for the later blocks for a hybrid approach that does even better at the cost of a slight increase in parameter size and MACs  

***

##### 🖼️ Paper Poster:

![MetaFormer poster](/assets/images/metaformer.jpg "MetaFormer Poster")

***

##### 🛠 Possible Improvements:

- It sucks that the authors didn’t speculate on potential improvements to the MetaFormer architecture, nor did they clearly identify unsolved problems with this architecture.

##### ✏️My Notes:

- (4/5) MetaFormer is a pretty good name, and it actually does exactly what the name says it does.

- I actually laughed a bit while reading MetaFormer since it nailed how researchers often get carried away with hype without double-checking the true reason for their success. Please note that there is no definitive proof that the mixer architecture actually contributes as little as this paper claims, but their experimental data sorta-kinda suggests that it does. Regardless, it asks some seriously important questions and I am looking forward to seeing more work done on this topic.
- I was hoping to see PoolFormer variations that explore the importance of the two-layer MLP block in the ablations, but, alas maybe next time.
- You know what I haven't mentioned yet in this whole digest? CLIP! And for good reason, since I can't really think of a way MetaFormer would benefit from it.

- What do you think about MetaFormer? Write your opinion in the comments!

##### 🔗 Links:
[MetaFormer arxiv](ttps://arxiv.org/pdf/2111.11418.pdf) / [MetaFormer github](https://github.com/sail-sg/poolformer) / MetaFormer Demo - ?

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [SOTA pretraining with Masked Auto-Encoders - MAE explained]({% post_url 2021-11-16-MAE-explained %})  
- [GANs + Transformers = ? - GANsformer-2 explained]({% post_url 2021-11-24-GANsformer2-explained %})  
- [How to invert images with StyleGAN2 - The intuition explained]({% post_url 2021-11-19-AI-assisted-Image-Editing-Part1 %})  

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
