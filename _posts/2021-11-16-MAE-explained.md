---
layout: post
title: "69: MAE"
meta_title: "MAE"
description: "Masked Autoencoders Are Scalable Vision Learners by Kaiming He et al. explained in 5 minutes"
date: 2021-11-16 19:00:00 -0000
categories: self-supervised-large-scale-pretraining-vision-transformers
---

#### Masked Autoencoders Are Scalable Vision Learners by Kaiming He et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![MAE teaser](/assets/images/mae_teaser.png "MAE Teaser")

##### 🎯 At a glance:

The simplest solutions are often the most elegant and cleverly designed. This is certainly the case with a new model from Facebook AI Research called Masked Autoencoders (MAE) that uses such smart yet simple ideas that you can’t stop asking yourself “how did nobody think to try this before?” Using an asymmetric encoder/decoder architecture coupled with a data-efficient self-supervised training pipeline MAE-pretrained models outperform strong supervised baselines by learning to reconstruct input images from heavily masked image patches (75% blank patches).

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [ViT]({% post_url 2021-4-30-ViT-explained %})

***

##### 🚀 Motivation:

Nowadays, models have gotten so massive that overfitting on millions of images is becoming increasingly common, and finding public labeled datasets large enough - increasingly hard. Inspired by the success of masked autoencoding models in NLP and the advances in self-supervised learning the team at FAIR identifies and addresses 3 main issues that have prevented masked autoencoding models from doing that well in computer vision tasks.

First, convolutional models did not have an easy way to process mask tokens or positional embeddings. Luckily, this issue was solved with the recent introduction of ViT.

Second, a much higher level of knowledge is required to fill in missing words in a language task, and completing masked image pieces is way too easy, by comparison, hence a very high portion of the image is masked to force the decoder to understand high-level semantic concepts and objects instead of relying on neighboring patches.

The third and final point is that a language decoder predicts whole words, while an image decoder outputs low-level information in the form of raw pixels. This means that a simple MLP is sufficient for a language decoder, while much more care must be taken when designing an image decoder.

***

##### 🔍 Main Ideas:

**1) Masking:**  
The input image is split into non-overlapping patches with the majority of them (75%) randomly masked out. This approach makes the reconstruction task non-trivial, as it is no longer possible to recover the missing patches by simply extrapolating their neighbors.

**2) MAE encoder & decoder:**  
Interestingly, the model is asymmetrically designed so that the encoder and decoder are different. The encoder is a very large ViT model that only processes the non-masked input tokens, leveraging their low number to save a ton of computing and memory. The decoder, on the other hand, processes the entire image, mask tokens and all. For each mask token, a shared learning embedding vector is used along with positional encoding to indicate the location of the masked patch. The decoder is also a series of Transformer blocks with a linear projection layer that converts the output to pixel space, however, it is extremely lightweight, using only 10% of operations per token compared to the encoder.

**3) Reconstruction target:**  
MAE is trained with an MSE loss to reconstruct the pixel values of only the masked patches. Additionally, normalizing the pixels inside each masked patch with their mean and standard deviation improves the representation quality of the encoder.

##### 📈 Experiment insights / Key takeaways:

- Models are pretrained on ImageNet-1K
- ViT large trained from scratch is used as a baseline

- The optimal masking ratio is around 75%
- A single transformer block is enough to get 84.8% accuracy after fine-tuning
- For finetuning decoder width is more important than depth
- Default MAE decoder has 9% of FLOPs per token compared to ViT-Large

- Encoder performs worse if it uses the MASK token, cause without it the encoder always sees only the real patches, and works 3.5x-4.1x faster
- High-frequency components are useful in an MAE setting
- Predicting tokens from a pretrained DALL-E dVAE does not improve results over predicting patch-normalized pixel values
- Random masking heavily reduces the need for augmentations
- Simple random masking works better than block strategy or grid-wise sampling

- MAE outperforms BEiT and self-supervised ViT-L
- MAE representations are not well linearly-separable but are better non-linear features
- MAE works just as good or better as the considered baselines on segmentation and object detection tasks

***

##### 🖼️ Paper Poster:

![MAE poster](/assets/images/mae.jpg "MAE Poster")

***

##### 🛠 Possible Improvements:

- It seems that the authors did not reach the limit for scaling up their models, so if you have the necessary resources, it might be worth experimenting with even larger models
- Another idea from me - try this approach in a generative setting to improve existing ViTGAN models

##### ✏️My Notes:

- (3/5) for the name. Can’t give more than 3 for a name without even an attempt at a joke. If you created a flying car. would you name it “Flying Car”?!

- This paper is so succinctly written, the authors practically did my job for me :)
- Now, the big question is: can this approach beat VQGAN on unstructured image generation? Doesn’t seem likely out of the box, since the reconstructed images are rather blurry, but I can’t help to wonder…
- Stay with me, but could a loss in the CLIP’s shared image-language latent space help here? I kinda think it might be due to its innate ability to capture object concepts that the authors of MAE strive for.

- What do you think about MAE? Share your thoughts in the comments!

##### 🔗 Links:
[MAE arxiv](https://arxiv.org/pdf/2111.06377v1.pdf) / MAE github - ? / MAE Demo - ?

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [Essence transfer with TargetCLIP explained]({% post_url 2021-10-26-TargetCLIP-explained %})  
- [AdaConv explained - the best artistic style transfer model]({% post_url 2021-11-3-AdaConv-explained %})  
- [How to train GANs really fast - ProjectedGAN explained]({% post_url 2021-11-8-ProjectedGAN-explained %})  

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
