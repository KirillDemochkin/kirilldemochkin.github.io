---
layout: post
title: "54: CLIP"
meta_title: "CLIP"
description: "Learning Transferable Visual Models From Natural Language Supervision by Alec Radford, Jong Wook Kim et al. explained in 5 minutes."
date: 2021-9-14 16:00:00 -0000
categories: zero-shot-contrastive-loss-image-text-pretraining
---

#### Learning Transferable Visual Models From Natural Language Supervision by Alec Radford, Jong Wook Kim et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![CLIP teaser](/assets/images/clip_teaser.png "CLIP Teaser")

##### 🎯 At a glance:

I have mentioned CLIP so many times in my posts that you might think I am being paid to promote it. Unfortunately, I am not, but a lot of my favorite projects use CLIP, and it is time to finally get into the nitty-gritty of the powerhouse that is CLIP.
CLIP is model from 2020 that is inspired by ideas from Alec Radford, Jong Wook Kim and the good folks at OpenAI

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [SimCLR]({% post_url 2021-7-13-SimCLR %})  
2) [ViT]({% post_url 2021-4-30-ViT-explained %})
3) [Transformer](https://arxiv.org/abs/1706.03762)

***

##### 🚀 Motivation:

GPT-3 found huge success on a range of NLP tasks in a zero-shot settings by pretraining on a massive web text dataset. Could the same approach be used for vision tasks? It has been shown that Unfortunately, existing datasets were either too small or contained low-quality annotations (meaningless ids, camera settings, etc), hence the authors collected their own dataset of 400 million raw image-text pairs directly from the internet. Existing approaches worked as proof-of-concept, yet they were far from being competitive with SOTA task-specific methods. CLIP successfully scaled to the huge new dataset called Web Image Text (WIT) and obtains properties similar to the GPT models, where it can solve a multitude of downstream tasks without additional training on a competitive level with task-specific models.

***

##### 🔍 Main Ideas:

**1) Creating a Sufficiently Large Dataset:** 
The simple truth is that existing manually labeled datasets are just way too small (100k samples) for training a model on the scale of GPT, and larger semi-automatically labeled datasets (100m) contain so much noise and useless metadata that after cleaning they shrink to a measly 15m samples (about the size of ImageNet). The intuition was that the required dataset already exists on the web. This led to the construction of a set of 500k queries from words appearing in Wikipedia at least 100 times and subsequent collecting of up to 20k image-text pairs that contain each query.

**2) Selecting an Efficient Pre-Training Method:** 
After experimenting with class-label prediction the authors realized that the key to success was in predicting the pairings between images and corresponding text instead of trying to predict the exact text for each image. This discovery led to the use of multi-class N-pair loss such that the cosine similarity for each correct pair of embeddings is maximized, and for the rest of the N^2-N pairings – minimized. This is the contrastive pretraining part of CLIP. Only a crop transform is used for the images, and both encoders project their outputs to a shared embedding space. The temperature parameter of the softmax is directly optimized alongside the mode parameters.

**3) Choosing and Scaling a Model:**  
The image encoder is either a ViT (used in the final model) or a modified ResNet-D with attention pooling instead of global average pooling, where the query is conditioned on the global average pooling output for the whole image. The text encoder is a Transformer. Although masked attention was used to allow for the use of pretrained language models, in practice all models were trained from scratch. CLIP is trained with a batch size of 32768 for over 12 days on 256 V100.
   
##### 📈 Experiment insights / Key takeaways:

- To use CLIP as a classifier in a zero-shot manner embeddings are computed for the input image and the text prompts for all available classes, normalized into a probability distribution by softmax with temperature. This setup can be thought of as a vision backbone that computes a latent representation and a hypernetwork that generates the weights of a linear classifier based on the input text
- CLIP benefits from prompt engineering - using template sentences to provide context for ambiguous class labels. The default prompt is "A photo of {label}". Adding details to prompts further improves the accuracy
- Ensembling (averaging) results for multiple variations of the input prompt boosts the performance by up to 5%
- CLIP outperforms a logistic classifier on top of ResNet-50 on several datasets
- CLIP underperforms on highly specialized datasets such as satellite imagery, medical images, etc
- For more datasets CLIP does 10-15% worse than fully supervised models
- CLIP performs better when the underlying representations are of high quality
- Linear classifier trained on top of CLIP features is on par with the best supervised models (or better)
- CLIP features are more robust to distribution shift than models pretrained on ImageNet

***

##### 🖼️ Paper Poster:

![CLIP paper poster](/assets/images/clip.png "CLIP Paper Poster")

***

##### 🛠 Possible Improvements:

- Better use of Zero-Shot weights for Few-Shot transfer
- There is still a large gap between CLIP zero-shot and SOTA on most datasets
- More efficient way of scaling CLIP 1000x
- Better handling of abstract and fine-grained tasks (counting objects in an image, distinguishing between models of cars, measuring distances between objects in images)
- CLIP has a fialure mode where it does OCR instead of classification. If show a banana with the word apple written on it, CLIP will likely classify the image as an apple
- WIT dataset might have underlying biases since it is not curated nor cleaned 

##### ✏️My Notes:

- (4/5)  for the model name – not a meme, but CLIP sounds snappy, and there is no finagling with the acronym
- As of the time of writing this, CLIP is mostly used as a loss in other projects that pushes embeddings for some text query describing the target image/transformation and the output image closer in the CLIP space, in effect making the image better match the input text
- One of the best things about CLIP is that the pretrained model and all of its code is released under the MIT License that permits commercial use, which is huge, because StyleGAN2, for example, cannot technically be used out-of-the-box without rewriting the code, and training it from scratch on your own images
- The most widely known project is probably VQGAN + CLIP that is used to generate cool (if somewhat nightmare-inducing) images. Read my tutorial [here](https://www.patreon.com/posts/55645292)
- Have you used CLIP in any of your projects? Let’s discuss in the comments!

##### 🔗 Links:
[CLIP arxiv](https://arxiv.org/pdf/2103.00020.pdf) / [CLIP github](https://github.com/openai/CLIP)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [SOTA NLP model for everything - FLAN]({% post_url 2021-9-11-FLAN-explained %})
- [Deep Visual SLAM - DROID-SLAM]({% post_url 2021-8-27-DROID-SLAM-explained %})
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
