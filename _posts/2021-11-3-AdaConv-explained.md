---
layout: post
title: "67: AdaConv"
meta_title: "AdaConv"
description: "Adaptive Convolutions for Structure-Aware Style Transfer by Prashanth Chandran et al. explained in 5 minutes"
date: 2021-11-3 19:00:00 -0000
categories: style-conditioned-image-to-image-style-transfer
---

#### Adaptive Convolutions for Structure-Aware Style Transfer by Prashanth Chandran et al. explained in 5 minutes

##### ⭐️Paper difficulty: 🌕🌑🌑🌑🌑

***

![AdaConv teaser](/assets/images/adaconv_teaser.png "AdaConv Teaser")

##### 🎯 At a glance:

Classical style transfer is based on Adaptive Instance Normalization, which is limited to transferring statistical attributes such as color distribution and textures while ignoring local geometric structures in the image. But that is the stuff of the past, let me introduce to you Adaptive Convolutions, a drop-in replacement, for AdaIN, proposed by Prashanth Chandran and the team at Disney research. AdaConv is able to transfer the structural styles along with colors and textures in real-time.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [AdaIN](https://arxiv.org/abs/1703.06868v2)

***

##### 🚀 Motivation:

One of the main AdaIN drawbacks is that the image statistics are computed globally without taking local spatial structure into account. For example, transferring the style from an image of black and white circles and squares with AdaIN will transfer the colors of the style image but ignore the geometric shapes altogether. Similar to Weight Demodulation from StyleGAN2 AdaConv predicts full convolution kernels, although AdaConv is sensitive to the spatial semantics of the, while Weight Demod is not. While SPADE does have per-pixel transformations it is not able to preserve the spatial layout of the content image.

***

##### 🔍 Main Ideas:

**1) Overview:**  
AdaConv modifies AdaIn in two ways. First, it replaces the scale term with a predicted 2d convolutional kernel that modulates the feature channel in a spatially-varying way. Second, it turns that convolution into a depthwise separable convolution with another predicted kernel which helps it better model the correlation between channels. Interestingly, the number of kernels of both types (pointwise and depthwise) can be arbitrarily large.

**2) Style Transfer with AdaConv:**  
The network is an encoder-decoder-style model where the content and style images are encoded with a pretrained VGG-19. The style features are further processed with a style encoder to obtain the global style descriptor that is used by the kernel prediction networks to output the depthwise separable convolutional kernels with per-channel biases. There are 4 in total and the predicted kernels are injected into all layers of the decoder right before the regular convolutions at different resolutions. The model is trained with the VGG-19 feature space content and style losses.

**3) Style Encoder:**  
The output of the pretrained VGG network is (512, 32, 32), hence a small convnet with a fully-connected layer is trained on top of the VGG-19 to convert its output to a style vector. In effect, the style image is constrained to a fixed 256x256 size, while the content image can be of any dimension.

**4) Predicting Depthwise-Separable Convolutions**  
Each kernel predictor module consists of three small convnets that output the depthwise spatial kernel, the pointwise 1x1 kernel, and the per-channel biases. While all three submodules receive a reshaped global style tensor of the same size, the latter two use average pooling and 1x1 Conv to predict their parameters. The kernel predictor layers at higher resolutions use less channels in the depthwise kernel to account for the gradually reducing depth and increasing spatial size of the decoded image.

##### 📈 Experiment insights / Key takeaways:

- Baselines: a bunch of different AdaIN-based style transfer methods
- The results are pretty much all qualitative, and they look dope!
- AdaConv is WAY better at transferring the local structure of the style image along with texture and colors than AdaIN
- AdaConv actually changes the style of the transferred image based on the rotation of the input style image
- AdaConv not only interpolates the color and texture between styles but their structure as well
- AdaConv even works decently on videos
- AdaConv can be used as a drop-in replacement for Weight Demodulation in StyleGAN-2 models to generate good looking results (FID not provided)

***

##### 🖼️ Paper Poster:

![AdaConv poster](/assets/images/adaconv.jpg "AdaConv Paper Poster")

***

##### 🛠 Possible Improvements:

- I feel like VGG-19 as the feature encoder is a bit outdated, and could possibly be replaced by a self-supervised ResNet or CLIP

##### ✏️My Notes:

- (2/5) for the model name. Sorry, but it's kinda boring...
- Someone please make a huggingface space for this paper
- IMHO this is by far the best artistic style transfer model that I have seen so far
- I wonder whether layer swapping can be used with AdaConv to mix styles from different images at different scales
- Yes, I am a sucker for CLIP guidance, and I feel like text-conditioning could be used here to specify which parts of the style image should be transferred.
- I assume AdaConv style transfers can be distilled into a lightweight Pix2Pix model to be used in neural filters on phones, which gets me excited
- This concludes the new style transfer papers that I wanted to cover for now!

##### 🔗 Links:
[AdaConv arxiv](https://studios.disneyresearch.com/app/uploads/2021/04/Adaptive-Convolutions-for-Structure-Aware-Style-Transfer.pdf) / [AdaConv github (unofficial)](https://github.com/RElbers/ada-conv-pytorch) / AdaConv Demo - ?

***

##### 🔥 Check Out These Popular AI Paper Summaries:  
- [Essence transfer with TargetCLIP explained]({% post_url 2021-10-26-TargetCLIP-explained %})  
- [Sensorium = disentangled content + style image-to-image translation]({% post_url 2021-10-19-Sensorium-explained %})  
- [CIPS-3D]({% post_url 2021-10-22-CIPS-3D-explained %})  

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
