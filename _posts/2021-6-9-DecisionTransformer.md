---
layout: post
title:  "26: Decision Transformer Explained"
meta_title:  "Decision Transformer Explained"
description:  "Decision Transformer: Reinforcement Learning via Sequence Modeling by Lili Chen et al. explained in 5 minutes."
date:   2021-6-9 19:00:00 -0000
categories: sequence-modeling-reinforcement-learning-transformers
---

#### Decision Transformer: Reinforcement Learning via Sequence Modeling by Lili Chen et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![Decision Transformer: Reinforcement Learning via Sequence Modeling teaser](/assets/images/decision_transformer_teaser.png "Decision Transformer teaser")

##### 🎯 At a glance:

Transformers are everywhere. Why not add them to reinforcement learning (RL) as well? Yeah, that's right, the researchers at UC Berkley just did that. They approach RL as a sequence modeling problem and use an autoregressive transformer to predict the next optimal action given the previous states, actions, and rewards so that it maximizes some reward function. Perhaps surprisingly, this simple Decision Transformer approach achieves state-of-the-art performance on Atari, OpenAI Gym, Key-to-Door tasks.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*
1. [Transformer](https://arxiv.org/abs/1706.03762)

***

##### 🔍 Main Ideas:

**1) Motivation & Offline RL:**
Why are transformers even a good fit for RL tasks?  Transformers have a proven ability to model long sequences, can perform credit assignments directly via self-attention (useful when working with sparse rewards), and finally, empirical evidence suggests that transformers have better generalization ability than other considered baselines. The goal in offline RL is to use a limited number of trajectories (rollouts) of arbitrary (often suboptimal) policies. In other words, there is no environment to interact with, only several prerecorded sequences of actions, states, and rewards.

**2) Trajectory representation:**
Since the transformer should predict actions that lead to the states with the most possible future rewards instead of using past rewards the authors use rewards-to-go (the sum of the rewards for all future states starting with the current step).  At test time a starting state and a target reward are provided as conditioning information. The target reward is decremented after each taken action by the achieved reward.

**3) Architecture & Training:**
The last `K` number of steps is fed into the Decision Transformer. Each step is a triplet of return-to-go, state, action. An embedding with a layer norm is learned for each mode of input to obtain the input tokens. For visual inputs, a convolutional encoder is used. Additionally, an embedding is learned for each timestep (per 3 tokens). The tokens are processed by a GPT model, which predicts future tokens autoregressively.  
Minibatches of sequence length `K` are sampled from the dataset at each iteration. The model is trained with cross-entropy or MSE loss for discrete and continuous actions respectively. Only the next action is predicted as predicting other tokens empirically did not improve performance.

##### 📈 Experiment insights / Key takeaways:
- Decision transformer (DT) outperforms (mostly) or matches state-of-the-art TD learning algorithms for offline RL (CQL, BEAR, BRAC, AWR) on all tasks in the OpenAI Gym environment
- DT can finetune to a specific strategy (imitate a specific subset of action sequences) after learning on the whole dataset. Moreover, it outperforms Percentile Behavior Cloning (%BC) in settings with low amounts of data (using 1% of the best rollouts in Atari) by generalizing on all available trajectories even if they are not successful.
- DT almost perfectly predicts the return-to-go for its trajectories making it a good critic model as well
- The higher the K value used the better DT performs
- Delayed rewards minimally affect DT
- DT does not require policy regularization unlike TD-learning algorithms
- DT can (possibly) be used in online RL as well

***

##### 🖼️ Paper Poster:

![Decision Transformer: Reinforcement Learning via Sequence Modeling paper poster](/assets/images/DecisionTransformer.png "Decision Transformer Paper Poster")

***

##### ✏️My Notes:
- (3/5) for the name, too straightforward, no meta-humor :(
- Not many thoughts here, since I am not very well versed in RL, but the paper seemed interesting!
- I was really hoping to see videos of simulated trajectories comparing DT to other TD-learning algorithms

##### 🔗 Links:
[Decision Transformer arxiv](https://arxiv.org/pdf/2106.01345.pdf) / [Decision Transformer github](https://github.com/kzl/decision-transformer)

***

##### 👋 Thanks for reading!

<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
