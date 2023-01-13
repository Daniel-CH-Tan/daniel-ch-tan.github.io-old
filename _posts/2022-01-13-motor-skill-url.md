---
layout: distill
title: Motor Skill Discovery with Unsupervised RL
description: Learning general and re-usable skills for continuous-control tasks in robotics
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

In this blog post, I'll talk about my work so far on applying unsupervised RL methods to learning general and re-usable motor skills for robotic tasks. For now, I'm focusing mainly on locomotion tasks. 

### The need for diverse skills

Animals and humans exhibit a wide variety of agile and robust motor skills which they employ in a task-oriented way to successfully navigate their environment. For example, when a natural agent needs to traverse a large ditch, it must be able to select the appropriate motor skill, such as building up speed and jumping across if the gap is small enough, or climbing down and back up the other side otherwise. 

Put another way, every environment has a corresponding 'minimal skill set' that is required to fully traverse it. Simple environments, like flat and uniform ground in simulation, require only very simple skills; correspondingly, the complexity of the terrain and obstacles encountered in the real world is what requires natural agents to learn and use complex motor skills. 

In this sense, useful skills are necessarily defined by the environment; the required skillset for agents to navigate a forest may be very different from the required skillset in an urban setting. Nevertheless, we can aim to learn a superset of skills that will contain the appropriate minimal skillset for a variety of target environments. 

### Reinforcement learning

Reinforcement learning has been shown to be a powerful framework for solving difficult sequential decision problems. For example, AlphaGo, AlphaFold. 

One approach to enabling simulated agents to learn these motor skills, is to replicate the complexity of the real-world environment in simulation. This is the approach taken in <d-cite key="rudin2021walk"></d-cite>. A complex simulated environment with diverse terrain is set up, and a policy implicitly learns all the skills necessary to navigate such an environment. 

### Controllable skills

In the previous framework, skills are implicitly learned by the policy and used only in the appropriate scenarios. In other words, the choice of which skill to use depends only on the instantaneous perceptual input; it is purely a reactive decision process, without conscious planning.

However, we can also think of skills existing separately from their particular use cases; for example, a dog that can jump across a large gap, can execute the same jump on flat ground. This motivates the use of additional **conditioning variables** for specifying which skill should be executed. Various works have introduced such conditioning variables; <d-cite key="Peng_2021"></d-cite> uses reference motion frames as conditioning variables, whereas <d-cite key="sharma2019dynamics"></d-cite> uses an abstract latent variable as the conditioning variable.  

### Transfer Learning

Another benefit of a skills-based framework is that it enables transfer learning. By first learning a diverse set of low-level skills, these low-level skills can be composed together as building blocks for more difficult problems. 

### Conclusion

In this blog post, I discussed the motivating factors and benefits/disadvantages of the skills framework vis-a-vis reinforcement learning. I also reviewed some of the current literature in this field. 

In the next blog post, I'll talk more about my current progress in applying a skills-based framework to robotic control tasks. 