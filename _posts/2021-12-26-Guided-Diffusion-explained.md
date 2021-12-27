---
layout: post
title: "75: Guided Diffusion"
meta_title: "Guided Diffusion"
description: "Diffusion Models Beat GANs on Image Synthesis by Prafulla Dhariwal and Alex Nichol explained in 5 minutes"
date: 2021-12-26 16:00:00 -0000
categories: guided_diffusion_langevin_dynamics_classifier_guidance
---

#### Diffusion Models Beat GANs on Image Synthesis by Prafulla Dhariwal and Alex Nichol explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌕

***

![Diffusion](/assets/images/diffusion_teaser.png "Diffusion Teaser")

##### 🎯 At a glance:

GANs have dominated the conversation around image generation for the past couple of years. Now though, a new king might have arrived - diffusion models. Using several tactical upgrades the team at OpenAI managed to create a guided diffusion model that outperforms state-of-the-art GANs on unstructured datasets such as ImageNet at up to 512x512 resolution. Among these improvements is the ability to explicitly control the tradeoff between diversity and fidelity of generated samples with gradients from a pretrained classifier. This ability to guide the diffusion process with an auxiliary model is also why diffusion models have skyrocketed in popularity in the generative art community, particularly for CLIP-guided diffusion.

Does this sound too good to be true? You are not wrong, there are some caveats to this approach, which is why it is vital to grasp the intuition for how it works!

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [DDPM](https://arxiv.org/abs/2006.11239)  
2) [Langevin Dynamics](https://en.wikipedia.org/wiki/Langevin_dynamics)  
3) [DDIM](https://arxiv.org/abs/2010.02502)

***

##### 🚀 Motivation:

Lately, GANs have gotten really good at generating insanely realistic images, yet they still lack diversity in generated samples compared to ground truth data. One possible solution is to use likelihood-based models as they are easier to train, but they mostly suck compared to GANs in terms of sample quality. The authors hypothesize that two factors are holding likelihood-based models from reaching peak performance. First, GAN architectures are simply more refined thanks to the countless research hours spent exploring and optimizing every little thing about them. Second, GANs trade-off diversity for fidelity very efficiently to produce high-quality samples by covering only a subset of the true data distribution.

Diffusion models existed before this paper, although still in the shadow of their more appealing GAN alternatives for datasets beyond 64x64 CIFAR-10. Luckily, the authors propose more exciting ways to boost performance than simply throwing more compute at the model and hoping it works.

***

##### 🔍 Main Ideas:

_Note: You will not find explanations of formulas and their derivations here, as I honestly do not understand them well enough to explain in a coherent manner, instead this post is focused on the intuition behind the main ideas proposed in guided diffusion._

**1) What is diffusion?**
Diffusion is an iterative process that tries to reverse a gradual noising process. In other words, in diffusion, there exists a sequence of images with increasing amounts of noise, and during training, the model is given a timestep, an image with the corresponding noise level, and some noise. The goal then is to reconstruct the input image by mixing it with the noise and predicting the mixed noise from the slightly more corrupted resulting image. Surprisingly a simple MSE loss on the input and predicted noise is sufficient for high-quality results (in practice this loss is combined with estimated lower bound to reduce the number of required diffusion steps), given that the noise is modeled as a diagonal gaussian with its mean and std predicted by two MLPs.

At inference, the model starts from an image made entirely of noise and iteratively denoises it to arrive at a high-quality sample by predicting noise that is filtered out at each step, gradually adding more and more fine details. This process is similar to progressive generation seen in GANs, where the output of the first few layers determines the pose and other low-level features of the generated sample, while the latter layers add finer details. It has been shown that with enough steps it is possible to produce diverse high-quality samples with diffusion models.

**2) Architecture improvements:**  
The base model is a UNet with residual layers in the downsampling and upsampling branches with skip-connections linking the two branches. Additionally, it has global attention at the 16x16 resolution, and a projection of the timestep embedding in every residual block after the Group Norm operation, which the authors call Adaptive Group Normalization (AdaIN, but with a GroupNorm instead of Instance Norm).

Models with increased width reach the desired sample quality faster than models with increased depth.

Using BigGAN up/downsampling, increasing the number of heads, lowering the number of channels per head (64 channels per head are used in reported experiments), and using attention at all scales (32x32, 16x16, 8x8) instead of one all leads to improved FID.

**3) Classifier Guidance:**  
GANs have a nice mechanism to incorporate class labels into the generated samples in the form of class-conditioned normalization and classifier-like discriminators. Conditioning a diffusion model on a class is nontrivial to say the least (FYI there is a ton of math that I am skipping here to get to the big picture idea, see the source paper for derivations).

Assuming that we believe the math behind this intuition all checks out, we have a pretrained classifier model that we can use to predict the label of a noisy intermediate image, compare it to our target label and compute the gradient with respect to the model parameters. Turns out that the conditional backwards step for obtaining the “slightly less noisy” image by predicting the noise that produced the input image is almost the same as the unconditional variant: predict the parameters of a Normal distribution, offset the mean by the scaled gradient, and sample from the resulting distribution to obtain the noise that gets filtered out to bring us one step closer to a nice noise-free image.

**4) Scaling Classifier Gradients:**  
It is possible to control how much of an effect the class-conditioning has on the generated samples by scaling the classifier gradient offset up by a hyperparameter. This, in a fact, is an explicit way to control the diversity-quality tradeoff.

##### 📈 Experiment insights / Key takeaways:

- Datasets: LSUN bedrooms, horses, cats; ImageNet at 128, 256, 512 resolutions  
- Baselines: DCTransformer, DDPM, IDDPM, StyleGAN, VQ-VAE2, SR3, BigGAN-deep  
- In some cases even 25 diffusion steps is enough to outperform the best GANs while maintaining higher recall (diversity)  

- An awesome trick that does wonders for diffusion is to train 2 models: one in low resolution, and another in a high resolution. The way it works is that the low resolution model learns to generate samples via diffusion, and the high resolution model learns to upsample lowres images from the dataset starting from a bilinearly upsampled version of a lowres input image. At inference the two models are stacked: the lowres model produces a novel sample, and the highres model refines it, which greatly improves FID on ImageNet.  
- Only the lower resolution model is guided as dictated by the experimental data.  

***

##### 🖼️ Paper Poster:

![Diffusion poster](/assets/images/diffusion.jpg "Diffusion Poster")

***

##### 🛠 Possible Improvements:

- Speed this bad boy up, possibly approximate the entire process with a single feed-forward step  
- Apply CLIP to guide the diffusion process (already successfully done by the really smart people on Twitter)  

##### ✏️My Notes:

- (4/5) Diffusion is a dope word, and although the model name choice gives too many utilitarian vibes for my taste (ADM - Ablated Diffusion Model… really?), this is the perfect paper name for SEO  
- This paper is dense with formulas and equations, for someone who is not a math wiz, trying to understand everything was a struggle to be honest. Had to google a lot for a statistics refresher  

- I am glad to have finally covered diffusion on this blog. I believe that we will see a lot more papers building on this idea in 2022 and beyond, hence it is vital to grasp the intuition of the base model now to stay in the loop later  
- The authors somewhat understate the fact that diffusion is painfully slow  
- If statistics are fresh in your mind, and you love greek letters, I recommend checking out the appendix, as there are a couple more derivations tucked away in sections B and H  

- What do you think about guided diffusion? Share your thoughts in the [chat](https://t.me/casual_gans_chat)!

##### 🔗 Links:
[Diffusion arxiv](https://arxiv.org/pdf/2105.05233.pdf) / [Diffusion Github](https://github.com/openai/guided-diffusion)

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [100x Faster than NeRF - Plenoxels explained]({% post_url 2021-12-17-Plenoxels-explained %})  
- [NeRF + CLIP = Insanity - DreamFields explained]({% post_url 2021-12-7-DreamFields-explained %})  
- [SOTA StyleGAN inversion - HyperStyle explained]({% post_url 2021-12-3-HyperStyle-explained %})  

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
