---
layout: distill
title: Skill Learning
description: Thoughts on learning general and re-usable skills for continuous-control tasks in robotics
date: 2022-01-13

authors:
  - name: Daniel C.H Tan
    url: "daniel-ch-tan.github.io"
    affiliations:
      name: UCL CS, A*STAR

bibliography: 2022-01-13-motor-skill-url.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Introduction
    subsections:
    - name: Representation Learning
    - name: Options Theory
  - name: Learning Skills
  - name: Using Skills
    subsections:
    - name: Hierarchical Learning
    - name: Teacher-Student Learning
    # if a section has subsections, you can add them as follows:
    # subsections:
    #   - name: Example Child Subsection 1
    #   - name: Example Child Subsection 2
  # - name: Citations
  # - name: Footnotes
  # - name: Code Blocks
  # - name: Layouts
  # - name: Other Typography?

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }

---

## Introduction

As humans, we have learned certain patterns of whole-body actuation as high-level "skills" (walking, running, jumping, etc.) In planning our bodily actions, we do not consciously control each and every actuator (muscle) in our body. Rather,we largely constrain ourselves to these general and re-usable patterns, adapting and composing them on the fly to fit the situation. 

Why not do the same for robots? 

### Representation Learning

One timeless lesson from deep learning is that, in dealing with high-dimensional spaces, it is often useful to first compress it to a succint low-dimensional latent representation that can be used for downstream tasks. In recent times, there has been considerable interest in applying this technique to deal with high-dimensional observation spaces - for example, in robotic manipulation from camera feeds. Skill learning can be thought of as representation learning applied to action spaces. 

### Options Theory

One useful mathematical formulation for skill learning is the options framework introduced in <d-cite key="sutton1999options"></d-cite>. For a given Markov Decision Process (MDP), options are auxilliary policies that conditionally execute upon entering certain states and run until their termination condition is met. 

In the original formulation, each option exists as a separate policy. In practice, it has been popular <d-cite key="sharma2019dynamics"></d-cite> <d-cite key="Peng_2021"></d-cite> to implement skill learning using a latent-conditioned policy. Each instantiation of the latent variable corresponds to a single option, and the choice of distribution over the latents induces a family of options. Assuming sufficient regularity of the latent space, the latent-variable representations can be interpolated to sample novel skills. 

## Learning Skills

How do we obtain high-quality skills? To do that we must first define our notion of 'quality'. 

### Domain Knowledge

For a given control task and action space, domain knowledge may give us clues about how to design families of high-quality skills. For example, in legged locomotion, classical control methods have done well by using gait-parameterized actuation as a family of skills. Similarly, generalized hybrid-zero dynamics has been an effective heuristic for designing skills for bipedal locomotion. 

### Data-Driven

Another way to obtain a family of high-quality skills, is by imitating an expert that already possesses those skills. For the task of simulated locomotion, ASE <d-cite key="peng2022ase"></d-cite> and its predecessors used motion-capture data from known quadrupedal and bipedal experts (i.e. animals and humans). 

### Self-Supervised Discovery

Lastly, we can aim to discover skills simply from pure exploration. However, in the absence of extrinsic inductive biases, skill discovery requires intrinsic priors on high-quality skills, e.g diversity and consistency of the skill set <d-cite key="sharma2019dynamics"></d-cite>. By optimizing these intrinsic metrics, we hope that skills emerge autonomously from the learning process. 

Compared to the previous approaches, there is no guarantee of quality of individual skills in the learned set. However, by maximizing diversity, we can hope that the learned skill set contains sufficient high-quality skills that transfer well to downstream tasks. 


## Using Skills

Okay, so suppose that we have a collection of high-quality skills. How do we use them? 

### Hierarchical Learning

A suitable collection of skills (or options) can be used as action primitives for downstream control tasks. For example, a family of options can be treated as a low-level controller. A high-level planner can then use the option space as its action space, selecting and sequencing options to solve specific tasks. 

Doing this has several benefits. First and foremost, it enables transfer learning. The high-level policy does not need to learn skills from scratch, but can simply make use of them. Exploration in a highly informative, low-dimensional latent space is likely to be much easierr than exploration in a high-dimensional, noisy space. Similar to how pre-training on ImageNet, starting with a high-quality family of options can greatly improve sample efficiency on downstream tasks. 

### Teacher-Student Learning

Another perspective is to simply treat the learned skills as 'teachers' that generate off-policy state-action-reward histories, which are made available to student policies. This could be used by off-policy RL algorithms or even by imitation learning methods. 

Compared to the hierarchical approach, this has the benefit of fewer architectural constraints on the final policy. In turn, it likely trades off some of the gains in sample efficiency from directly re-using learned skills as action primitives. 

## Conclusion

In this blog post, I discussed the motivating factors and benefits/disadvantages of the skills framework vis-a-vis reinforcement learning. I also reviewed some of the current literature in this field. 

In the next blog post, I'll talk more about my current progress in applying a skills-based framework to robotic control tasks. 