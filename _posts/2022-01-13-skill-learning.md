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
  - name: Related Work
  - name: Learning Skills
  - name: Using Skills
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

As humans, we have learned certain patterns of whole-body actuation as high-level "skills" (walking, running, jumping, etc.) In planning our bodily actions, we do not consciously control each and every actuator (muscle) in our body. Rather,we largely constrain ourselves to these general and re-usable patterns, adapting and composing them on the fly to fit the situation. Why not do the same for robots? 

In this blog post, I will describe the motivations and approaches for skill learning in control problems, i.e. to **learn** and **discover** a **diverse** and **high-quality** set of **action primitives** that **transfer well** to downstream tasks. 

## Motivation

### The Cereal-Making Robot

An average ten-year-old could probably walk into the kitchen and make themselves a bowl of cereal unsupervised. Yet the same capability remains out of reach of modern robots. (Or at the least, it has yet to be demonstrated!) Why is that? (Example credit: Chelsea Finn, [podcast episode](https://thegradientpub.substack.com/p/chelsea-finn-on-meta-learning-and#details) hosted by [The Gradient](https://thegradientpub.substack.com/s/podcast)). 

<div class="row mt-3" style="position: relative">
    <div class="col-sm mt-3 mt-md-0" style="width: 50%">
        {% include figure.html path="assets/img/skill_learning/cereal.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Making cereal is so easy, even kids can do it!
</div>

More generally, we would like to develop control frameworks that enable robots to accomplish **complex tasks** in the **real world**. 
<!--- There are many angles through which this could be tackled, and as yet there is no consensus on what the best one is. For example, even the method of **task specification** admits multiple perspectives: cost functions a la optimization, reward functions a la RL, or even natural language. Some work focuses on imbuing robots with **common-sense understanding** of real-world objects and physics.  -->

### Skills as Building Blocks

One method of enabling complex task-oriented behaviour, is to adopt an incremental approach and try to learn to solve simpler subproblems first, then compose those solutions into a full solution. As programmers, this sounds inherently appealing. Going back to the cereal analogy, we might come up with a list of lower-level skills that need to be learned first, such as **finding** the necessary ingredients, **picking** up the cereal box, **pouring** the milk into a bowl, and **carrying** it out of the kitchen while maintaining good **balance**. 

If we had access to high-quality action primitives that were able to solve each of these subproblems, the overall problem reduces to a simple task of finding the right skills and sequencing them in the right order. Conversely, needing to learn to solve all of these subproblems from scratch makes the overall problem exponentially more difficult. 

The core problem of skill learning is therefore to discover and learn a diverse set of high-quality action primitives that transfer well to downstream tasks. 

### Skills for Dexterity

It has been said that dexterity is the "last open frontier in robotics". 

One reason for this is that learning-based approaches struggle is the difficulty of exploration in high-dimensional control space. For a humanoid morphology, even maintaining balance is already a rather challenging task, let alone learning to move in an agile, safe, and efficient way or exercise fine motor skills in handling everyday objects. 

## Related work

### Representation Learning

Skill learning can be thought of as representation learning. However, while most representation learning methods aim to construct informative representations of the observations space, we are instead focusing on the action space. The space of all valid action primitives scales as $$\dim(A^T)$$; yet the space of useful action primitives could be hypothesised to be restricted to a low-dimensional manifold within this combinatorially large space. Reducing the dimension of the action space has obvious benefits for improving interpretability as well as the efficiency of exploration. 

### Options Theory

One useful mathematical formulation for skill learning is the options framework introduced in <d-cite key="sutton1999options"></d-cite>. For a given Markov Decision Process (MDP), options are auxilliary policies that conditionally execute upon entering certain states and run until their termination condition is met. 

### Latent-Variable Policies

In practice, it has been popular <d-cite key="sharma2019dynamics"></d-cite> <d-cite key="Peng_2021"></d-cite> to implement skill learning using a latent-conditioned policy. Each instantiation of the latent variable corresponds to a single option, and the choice of distribution over the latents induces a family of options. Assuming sufficient regularity of the latent space, the latent-variable representations can be interpolated to sample novel skills. 

### Planning with subgoals

A similar line of reasoning is employed in **subgoal**-conditioned learning and planning; however, while subgoals are akin to static milestones that need to be achieved in order to reach the main goal, skills are akin to the known routes that take us safely and quickly between subgoals. 

## Learning Skills

Learning a skill refers to learning how to execute the skill, given that we already know what it is. Depending on choice of representation, this may be very easy; for example, if we have a skill represented as a state-action trajectory, then simple behaviour cloning would enable the skill to be learned. For the task of simulated locomotion, ASE <d-cite key="peng2022ase"></d-cite> and its predecessors used motion-capture data from known quadrupedal and bipedal experts (i.e. animals and humans). 

For a given control task and action space, domain knowledge may also give us clues about how to design families of high-quality skills. For example, in legged locomotion, classical control methods have done well by using gait-parameterized actuation as a family of skills. Similarly, generalized hybrid-zero dynamics has been an effective heuristic for designing skills for bipedal locomotion. 

However, in practice, such data and domain knowledge may not be readily available, which leads us to...

## Discovering Skills

Instead of relying on prior knowledge, we can aim to discover skills simply from pure exploration. However, in the absence of extrinsic inductive biases, skill discovery requires intrinsic priors on high-quality skills, e.g diversity and consistency of the skill set <d-cite key="sharma2019dynamics"></d-cite>. By optimizing these intrinsic metrics, we hope that skills emerge autonomously from the learning process. 

Compared to the previous approaches, there is no guarantee of quality of individual skills in the learned set. However, by maximizing diversity, we can hope that the learned skill set contains sufficient high-quality skills that transfer well to downstream tasks. 

## Transferring Skills to Tasks

Okay, so suppose that we have a collection of high-quality skills. How do we use them in a task-oriented way? 

### Hierarchical Learning

A suitable collection of skills can be directly used as a low-level controller. A high-level planner can then use the skill space as its action space, selecting and sequencing options to solve specific tasks. 

Doing this has several benefits. First and foremost, the high-level policy does not need to learn skills, but can simply make use of them in a zero-shot manner. Furthermore, exploration in a highly informative, low-dimensional latent space is likely to be much easier than exploration in a high-dimensional, noisy space, greatly improving sample efficiency on downstream tasks. 

To understand this, consider that simple exploration strategies in the skill space can translate to highly structured and efficient exploration in the action space. 
<!---(TODO: add easy-to-digest example) -->

### Teacher-Student Learning

Another perspective is to simply treat the learned skills as 'teachers' that generate off-policy state-action-reward histories, which are made available to student policies. This could be used by off-policy RL algorithms or even by imitation learning methods. 

Compared to the hierarchical approach, this has the benefit of fewer architectural constraints on the final policy. In turn, it likely trades off some of the gains in sample efficiency from directly re-using learned skills as action primitives. 

## Conclusion

In this blog post, I discussed the motivating factors and benefits/disadvantages of the skills framework vis-a-vis reinforcement learning. I also reviewed some of the current literature in this field. 

In the next blog post, I'll talk more about my current progress in applying a skills-based framework to robotic control tasks. 