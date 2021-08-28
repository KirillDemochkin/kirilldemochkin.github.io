---
layout: post
title:  "49: DROID-SLAM"
meta_title: "DROID-SLAM"
description:  "DROID-SLAM: Deep Visual SLAM for Monocular, Stereo, and RGB-D Cameras by Zachary Teed and Jia Deng explained in 5 minutes."
date: 2021-8-24 19:00:00 -0000
categories: 3D-Visual-SLAM-Structure-from-Motion
---

#### DROID-SLAM: Deep Visual SLAM for Monocular, Stereo, and RGB-D Cameras by Zachary Teed and Jia Deng explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![DROID-SLAM Samples](/assets/images/droidslam_teaser.png "DROID-SLAM teaser")

##### 🎯 At a glance:

How to model dynamic controllable faces for portrait video synthesis? It seems that the answer lies in combining two popular approaches - NeRF and 3D Morphable Face Model (3DMM) as presented in a new paper by ShahRukh Athar and his colleagues from Stony Brook University and Adobe Research. The authors propose using the expression space of 3DMM to condition a NeRF function and disentangle scene appearance from facial actions for controllable face videos. The only requirement for the model to work is a short video of the subject captured by a mobile device.

##### ⌛️ Prerequisites:

**(Check these out if you have extra time):**  
1) [SLAM](https://github.com/liulinbo/slam/blob/master/Probabilistic%20Robotics%20_Sebastian%20Thrun%20et%20al..pdf) (page 309)  
2) [RAFT](https://arxiv.org/pdf/2003.12039.pdf)

***

##### 🚀 Motivation:

Simultaneous Localization And Mapping (SLAM) aims to simultaneously (duh) build a map of the environment and localize the agent on that map. SLAM is vital for robotics and autonomous driving, and modern SLAM systems still lack the robustness demanded for many real-world applications. Since oftentimes the input to SLAM is a video stream of sequential frames from a camera moving through an environment, deep neural networks seem like an obvious candidate to save the day. Yet newer DL-based SLAM methods fall short of the accuracy of classic algorithms. DROID-SLAM key advantages are iterative updates of camera poses and depth on an arbitrary number of frames and "differentiable Dense Bundle Adjustment Layer which computes Gauss-Newton update to camera poses and dense per-pixel depth so as to maximize their compatibility with the current estimate of optical flow". These enable high accuracy, robustness and generalization.

***

##### 🔍 Main Ideas:

**1) Representation:**  
The input to the model is an ordered collection of images. Additionally, for each frame a camer pose (SE3)  and inverse depth are iteratively updated. A dynamically built and updated frame-graph is used to represent co-visibility (overlapping fields of view) between frames. Loop closures (long range graph connections) are performed if the camera returns to an already visited region.

**2) Feature Extraction and correlation:**  
A feature and context networks are used to process input images. Feature network is used to build correlation volumes, and the context network - to predict features injected into the network during each update. 4D correlation volume of two frames is computed by taking an inner product of every pair of their feature vectors, averaging the last two dimensions at 4 different scales, bilinearly sampling from each scale with a correspondence field (next section), and concatenating the results.

**3) Update Operator:**  
This is a 3x3 convolutional GRU that iteratively predicts pose and depth updates. At the start of each iteration a correspondence field is computed for every edge in the frame graph by mapping the estimated inverse depth map and a coordinate grid on one frame to a 3D point cloud and back to another frame using the estimated camera poses. This correspondence field is used to sample the correlation volumes and optical flow between frames. Combined they help to align visually similar regions. At this point the context, correlation, and flow features are all injected into the GRU that outputs low res updates for correspondence fields (flow revisions) and confidence scores.

**4) Dense Bundle Adjustment Layer:**  
DBA maps the set of flow revisions to pose and pixelwise depth updates by solvinga linearized cost function defined as the sum of Mahalanobis distances for each edge in the frame graph weighted by the confidence scores predicted in the previous step with a Gauss-Newton solver.

**5) Training:**  
Redundant degrees of freedom in the camera poses (6-dof gauge freedom) are removed by fixing the first two camera positions to ground-truth poses. Training examples are 7-frame sequences selected such that the average flow between adjacent frames is between 8 and 96 px. An L2 loss between predicted and GT flow fields along with an L2 loss on poses is used.

**6) Inference:**  
Frames are collected in real time, keepring frames with a large enough flow. Once 12 frames are collected the frame graph is initialized and 10 iterations of the update operator are performed. For each new frame its features are extracted, it is added to the frame graph, and its pose is initialized with a linear motion model. Redundant or old frames are removed at each
   
##### 📈 Experiment insights / Key takeaways:

- DROID-SLAM leaves all other competition in the dust on TartanAir, EuRoC, TIM-RGBD, and ETH3D-SLAM datasets
- That is all, there isn't really a point in comparing them further

***

##### 🖼️ Paper Poster:

![DROID-SLAM paper poster](/assets/images/droidslam.png "DROID-SLAM Paper Poster")

***

##### ✏️My Notes:

- (5/5) - Awesome name: average BattleBots fan vs average DROID-SLAM enjoyer.
- Reading this paper gave such nostalgia for a perception in robotics course I took a couple of years ago [see the chat for gifs](https://t.me/casual_gans_chat)
- On the other hand, reading this paper was a bit more difficult than I expected since I had to google most of the terminology to remind myself, what it meant
- I wonder if a Transformer could be used to process the camera trajectory instead of a GRU

##### 🔗 Links:
[DROID-SLAM arxiv](https://arxiv.org/pdf/2108.10869.pdf) / [DROID-SLAM github](https://github.com/princeton-vl/DROID-SLAM)

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
