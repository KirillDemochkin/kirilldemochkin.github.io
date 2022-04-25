---
layout: post
title: "85: GLIDE"
meta_title: "GLIDE"
description: "GLIDE: Towards Photorealistic Image Generation and Editing with Text-Guided Diffusion Models by Alex Nichol et al. explained in 5 minutes"
date: 2022-4-25 16:00:00 -0000
categories: faster-diffusion-models-text-to-image-classifier-free-guidance
---

#### GLIDE: Towards Photorealistic Image Generation and Editing with Text-Guided Diffusion Models by Alex Nichol et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![GLIDE Model](/assets/images/GLIDE_preview.png "GLIDE Teaser")

##### 🎯 At a glance:

“Diffusion models beat GANs”. While true, the statement comes with several ifs and buts, not to say that the math behind diffusion models is not for the faint of heart. Alas, GLIDE, an OpenAI paper from last December took a big step towards making it true in every sense. Specifically, it introduced a new guidance method for diffusion models that produces higher quality images than even DALL-E, which uses expensive CLIP reranking. And if that wasn’t impressive enough, GLIDE models can be fine-tuned for various downstream tasks such a inpainting and and text-based editing.

As for the details, let’s dive in, shall we?

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Tip Casual GAN Papers', '#2b2f3c', 'V7V7BXBHV');kofiwidget2.draw();</script> 

***

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1. [Diffusion Models - ADM]({% post_url 2021-12-26-Guided-Diffusion-explained %})  
2. [CLIP]({% post_url 2021-9-14-CLIP-explained %})

##### 🚀 Motivation:

It used to be with diffusion models that you could boost quality at the cost of some diversity with the classifier guidance technique. However, vanilla classifier guidance requires a pertained classifier that outputs class labels, which is not very suitable for text. Recently though, a new classifier-free guidance approach was introduced. It came with two advantages: the model uses its own knowledge for guidance instead of relying on an external classifier, and it greatly simplifies guidance, when it isn’t possible to directly predict a label, which is should sound familiar for fans of text-to-image models.

***

##### 🔍 Main Ideas:

**1. Model:**  
The authors take the ADM (the standard diffusion model) architecture and extend it with text conditioning information. This is done by passing the caption through a transformer model and using the encoded vector in place of the class embedding. Additionally, the last layer of token embeddings is projected to the dimensionality of every attention layer of the ADM model, and concatenated to the attention context of that layer.

**2. Classifier-free guidance:**  
After the initial training routine, the model is fine-tuned for classifier-free guidance with 20% of the captions replaced with empty sequences. This enables the model to synthesize both conditional and unconditional samples. Hence, during inference two outputs are sampled from the model in parallel. One is conditioned on the text prompt, while the other is unconditional and gets extrapolated towards the conditional sample at each diffusion step with a predetermined magnitude.

**3. Image Inpainting:**  
The simplest way to do inpainting with diffusion models is to replace the known region of the image with a noised version of the input after each sampling step. However this approach is prone to edge artifacts, since the model cannot see the entire context during the sampling process. To combat this effect, the authors of GLIDE fine-tune the model by erasing random regions of the input images and concatenating the remaining regions with a mask channel.

**4. Noised CLIP:**  
The authors of GLIDE noticed that CLIP-guided diffusion can not handle intermediary samples very well since they are noised and most likely fall out of the distribution of the pertained, publicly available CLIP model. As a pretty simple fix they train a new CLIP model on noised images to make the diffusion-guiding process more robust.

##### 📈 Experiment insights / Key takeaways:

- *Baselines:* DALL-E, LAFITE, XMC-GAN (second best), DF-GAN, DM-GAN, AttnGAN  
- *Datasets:* MS-COCO  
- *Metrics:* Human perception, CLIP score, FID, Precision-Recall  
- *Qualitative:* Classifier-free guided samples look visually more appealing than CLIP-guided images. GLIDE has compositional and object-centric properties.  
- *Quantitative:* Classifier-free guidance is nearly Paretto optimal in terms of FID vs IS, Precision vs Recall, and CLIP score vs FID. The takeaway is that CLIP-guidance finds adversarial samples for CLIP instead of the most realistic ones.  

***

##### 🖼️ Paper Poster:

![GLIDE poster](/assets/images/GLIDE.jpg "GLIDE Poster")

***

##### 🛠 Possible Improvements:

- From the model card: “Despite the dataset filtering applied before training, GLIDE (filtered) continues to exhibit biases that extend beyond those found in images of people.”  
- Unrealistic and out-of-distribution prompts are not handled well, meaning that GLIDE samples are limited by the concepts present in the training data.

##### ✏️My Notes:

- *(Naming: 4/5)* Memorable but not a meme.  
- *(Reader Experience - RX: 3/5)* While the samples are presented in a very clean and consistent manner (except for figure 5 that does not fit on the screen, which is an issue because the models are arranged row-wise and you will need to scroll back and forth to compare samples across models), the strange order and naming of the paper section and lack of an architecture overview figure threw me for a loop. Moreover, the structure of the paper is quite unorthodox as most of the information about the proposed method is actually hidden in the background section, not in the typical “The Proposed Method” section, which is simply called “Training” here, and contains configuration details I would expect to see in the beginning of the “Experiments” section.  

- Classifier-free guidance reminds me of the good ol’ truncation trick from StyleGAN  
- Props to the authors for citing Katherine Crowson  
- TBH I wonder, how the heck does 64x64 CLIP even work? I don’t think I could compare images to captions at that resolution with my eyes not to even mention a model  
- Not sure how I feel about the whole “this model is not safe, hence we won’t release it” narrative that OpenAI is trying to spin since they clearly intend to monetize these huge AI models.  

##### 🔗 Links:

[Make-A-Scene arxiv](https://arxiv.org/abs/2112.10741) / [Make-A-Scene GitHub](https://github.com/openai/glide-text2im)

##### 💸 If you enjoy Casual GAN Papers, consider tipping on KoFi:  

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Tip Casual GAN Papers', '#e02863', 'V7V7BXBHV');kofiwidget2.draw();</script> 

***

##### 🔥 Read More Popular AI Paper Summaries:
- [Mask-conditioned text-to-image - Make-A-Scene Explained]({% post_url 2022-4-12-Make-A-Scene-explained %})
- [How to generate videos from static images - FILM Explained]({% post_url 2022-2-26-FILM-explained %})
- [How to do text-to-image without training on texts - CLIP-GEN Explained ]({% post_url 2022-3-9-CLIP-GEN-explained %})

##### 👋 Thanks for reading!
*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
