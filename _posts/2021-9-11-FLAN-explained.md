---
layout: post
title:  "53: FLAN"
meta_title: "FLAN"
description:  "Finetuned Language Models Are Zero-Shot Learners by Jason Wei et al. explained in 5 minutes."
date: 2021-9-11 16:00:00 -0000
categories: prompt-engineering-fine-tune-zero-shot-language-model
---

#### Finetuned Language Models Are Zero-Shot Learners by Jason Wei et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![FLAN teaser](/assets/images/flan_teaser.png "FLAN Teaser")

##### 🎯 At a glance:

These ginormous language models seem like with enough hacks and tricks they can handle whatever task is thrown at them, even in a zero-shot manner! This begs the question: is there a simpler way to generalize a language model to all kinds of unseen tasks by training on a subset of them? The folks at Google might have an answer in their new FLAN model, which is a decoder-only transformer model fine-tuned on over 60 NLP tasks in the form of natural language instruction templates. During inference FLAN outperforms the base model and zero-shot GPT-3 on most unseen tasks as well as few-shot GPT-3 on some.

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1) [GPT-3](https://arxiv.org/abs/2005.14165)

***

##### 🚀 Motivation:

FLAN aims to improve zero-shot performance of large language models. The key intuition is that NLP tasks can be described via natural language instructions such as "Is the sentiment of this mvie review positive or negative?" or "Translate 'how are you' into Chinese". Authors are able to combine the pretrain-finetune approach with prompt engineering by taking a 137B parameter language model (vs 175B parameters for GPT-3) and fine-tuning it on several types of NLP tasks clustered by type (sentiment, reading comprehension paraphrase, etc). This approach allows FLAN to do extremely well on held-out cluster of tasks despite a smaller model size.

***

##### 🔍 Main Ideas:

**1) Tasks and Templates:**  
Authors aggregate 62 text datasets from Tensorflow Datasets into 12 task clusters. For each task (unique input-output given by a dataset) 10 task templates are formed by hand. The templates are mostly paraphrases of the original task, but up to 3 templates ask for the inverse task i.e. generating a negative movie review for sentiment classification. The model is instruction-tuned on a mix of all of the templates with values plugged-in from the training data. Unseen tasks for inference come from held-out clusters during training. For tasks with predefined answer options an OPTIONS token is passed along with the list of output classes for that task.

**2) Training Details:**  
The 137B decoder-only transformer language model (which, I think, is just a fancy way of saying GPT) is pretrained on a collection of web-documents that is not as clean as the trainset of GPT-3, hence the expected zero and few-shot performance of the base model is expected to be slightly lower.  
Tasks for fine-tuning FLAN are limited to 30k examples, and all reported experiments ran for 30k gradient updates with a batch-size of 8192 using Adafactor.
  
##### 📈 Experiment insights / Key takeaways:

- FLAN > GPT-3 on 19/25 tasks
- GPT-3 wins on TriviaQA (Open-Domain QA), HellaSwag, PiQA, ReCoRD, WSC273, Winogrande (Commonsense reasoning & coreference resolution)
- Intuition for the results is that for the tasks above instructions are not crucial for describing the task
- More clusters & larger model -> better results

***

##### 🖼️ Paper Poster:

![FLAN paper poster](/assets/images/flan.png "FLAN Paper Poster")

***

##### 🛠 Possible Improvements:

- Remove subjectivivity from task clustering  
- Use longer instructions  
- Use more tasks  

##### ✏️My Notes:

- (4/5) - Yummy! But come on, LAnguage Net is a bit of a stretch, especially since it is literally called a Language MODEL everywhere else in the paper.
- NLP is not my strong suite, hence I am not even sure how good these results are, but they look impressive enough to an onlooker such as myself.
- I firmly believe that with CLIP on the rise we are not far from using a similar instruction-based task model for vision tasks. Image just putting in something like "highlight all of the cats on this image". I know DALL-E sorta kinda does it, but I think there is room for improvement.
- I was originally going to do CodeNeRF, but after reading it I did not feel inspired enough to write a post about it since it kinda looks like GIRAFFE did it better anyways
- What do you think about the results of FLAN? Let's discuss in the comments!

##### 🔗 Links:
[Robust Video Matting arxiv](https://arxiv.org/pdf/2109.01652v1.pdf) / [Robust Video Matting github](https://github.com/google-research/flan)

***

##### 🔥 Check Out These Popular AI Paper Summaries:
- [Cross-modal Transformer - PerceiverIO]({% post_url 2021-9-4-PerceiverIO-explained %})
- [Deep Visual SLAM - DROID-SLAM]({% post_url 2021-8-27-DROID-SLAM-explained %})
- [SOTA - Robust Video Matting]({% post_url 2021-9-7-Robust-Videо-Matting-explained %})

##### 👋 Thanks for reading!
<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
