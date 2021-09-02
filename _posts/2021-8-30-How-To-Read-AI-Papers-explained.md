---
layout: post
title:  "50: How To Read AI Papers"
meta_title: "How To Read AI Papers"
description:  "Casual GAN Papers: What I learned after writing summaries for 50 AI papers over the last 6 months! by Kirill Demochkin explained in 5 minutes."
date: 2021-8-30 19:00:00 -0000
categories: how-to-learn-to-read-ai-papers-quickly
---

#### Casual GAN Papers: What I learned after writing summaries for 50 AI papers over the last 6 months! by Kirill Demochkin explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

_Hey everyone!_

_As this is the 50th post for Casual GAN Papers, I wanted to celebrate by doing something a bit special for you to enjoy!_

_Cheers,_  
_-Kirill_

***

##### 🎯 At a glance:

Since I have been writing two summaries per week for some time now, I wanted to share some tips that I learned while doing it! First of all, It usually takes me around 2.5 hours from start to finish to read a paper, write the summary, compile the graphics into a single image, and post it to the channel and the blog. I mostly pick the papers that seem appealing to me, and ones that I see generating a lot of buzz on twitter. I got to admit, I do tend to favor more "fun" papers with crazy cool-looking image and gif results as well as papers with eye-catching titles. Once I have decided on a paper to summarize, I check to see if it has any "required" reading associated with it, then try to understand, what problem the new approach attempts to solve, and why existing methods are not enough. Then I skim the "Method" section, look at the figures, actually read the "Method" section, and finally make sure that the experiments section makes sense in terms of the numbers, metrics, baselines, and ablations as a sort of sanity check. Let's now dive deeper into each of these steps.

##### ⌛️ Prerequisites:

**(Check these out if you have extra time):**  
1) [Casual GAN Papers](https://www.casualganpapers.com/)

***

##### 🚀 Motivation:

Quickly reading papers is an essential skill for any researcher, especially in AI since our field develops at such a quick pace. Moreover, being able to summarize the ideas you read allows you to commit them better to your memory, and efficiently communicate with your colleagues during brainstorming sessions. Perhaps, not surprisingly, this is just another skill that can be honed with enough practice. Read on to find out about the Casual GAN Papers pipeline that I use for all of the posts you find in this channel.

***

##### 🔍 Main Ideas:

**1) Discovery:**  
I find most of the papers from paperswithcode, reddit, and a feedly list that a colleague shared with me. Unfortunately, I do not have much of a presence on twitter at the moment, and as I am learning more about community building, I am planning to start posting more consistently there so follow me [@KirillDemochkin](https://twitter.com/KirillDemochkin) for quick thoughts, news reactions, and short updates. Look for papers that:
- seem like they are doing something different - Paint Transformer, StyleMap-GAN, etc,
- apply a recently developed approach in a new setting - anything with ViT, NeRF, or CLIP from the last year.
- have great looking results (especially true for generative and img2img tasks) - StyleGAN-Nada, FLAME-In-NeRF, Generative Face Prior, etc

**2) Understanding the context:**  
Papers are not written in vacuum. Pretty much any paper (even breakthroughs like NeRF) has a history of prior works behind it. Understanding them greatly helps see the potential applications of the proposed approach, its strengths and weaknesses. Unfortunately, we can't usually spend a week reading all of the cited papers to build this context, hence a good strategy is to approximate it by explicitly writing out the specific task that a paper is attempting to solve, the existing alternative solutions for it, the issues with these solutions, and why the authors believe their method circumvents these issues.

**3) Grasping the main ideas:**  
Start by reading the section and subsection titles and connecting them with the parts of figures that depict the proposed architecture/pipeline. This way you will have the big picture mapped out in your head before reading the details, and getting confused as to what point is referencing what part of the model.

**4) Doing a sanity check of the experiments:**  
Look, the goal of any paper is to present the hard work done by the authors in one pretty package optimized for acceptance rate. This is why sometimes a bit of details are left out here and there. This is fine most of the time (it isn't very interesting to read about a 100 variations of the model that did not work), every now and then, however, you can gain additional insights based on WHAT was left out, and WHY. To understand this:
- Check the baselines - are there any recent methods that are left out of comparisons? b) Check the ablations - is every part of the pipeline tested for its overall contribution? Sometimes these things can get misattributed.
- Check the data, are there any known datasets not mentioned in experimental results? Perhaps, there is a common trait they share that points out some weakness in the proposed method

**5) Enjoy the cool results, and let the new ideas marinate a bit in your head to help you come up with exciting ways to apply them to other projects.** 

##### 📈 Experiment insights / Key takeaways:

- Try to determine right away, what papers are prerequisite reading to understand the new approach. You might be completely confused by what is going on in the paper only to realize that the details omitted are covered in detail in another paper.
- Try to understand clearly, what is the core problem that the new paper is attempting to solve ASAP. Having context is important to know which points to focus on
- Connect the text to the figures in the paper, visualizing the complicated architectures based on text alone is pretty tough
- Don't take the authors' words at face value. Do a sanity check in the experiments section to see if you spot any inconsistencies or glaring omissions that might hint at omitted data that does not fit the narrative of the paper
- Cross reference experiments with the future work ideas in the "Conclusion" section for quick project ideas

##### ✏️My Notes:

- (4/5) - Casual GAN Papers was a callback to the "Casually Explained" YouTube channel, and a record of the GAN papers I read, and as the channel grew I covered so much more than just GANs but I still think the name is a good inside joke 😉
- Thank you for your continued support everyone, this has been an amazing journey so far, and stay tuned for more Casual GAN Papers, I have big things planned for when the Patreon gets off the ground!
- Let me know in the [chat](https://t.me/casual_gans_chat), what type of papers you want to see more of in the coming months as we get closer to ICLR, CVPR, and the rest.

##### 🔗 Links:
[Casual GAN Papers - Community Chat](https://t.me/casual_gans_chat) / [Casual GAN Papers - Patreon](https://www.patreon.com/bePatron?u=53448948&redirect_uri=https%3A%2F%2Fwww.casualganpapers.com%2F&utm_medium=widget)

***

#### Check Out These Popular AI Paper Summaries:
- [SOTA SuperResolution - Real-ESRGAN]({% post_url 2021-7-30-Real-ESRGAN-explained %})  
- [Sketch-based GAN domain adaptation - Sketch-Your-Own-GAN]({% post_url 2021-8-10-Sketch-Your-Own-GAN-explained %})  
- [CVPR 2021 Best Paper - GIRAFFE]({% post_url 2021-7-8-GIRAFFE %})  

##### 👋 Thanks for reading!

<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
