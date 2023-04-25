---
layout: page
title: Reinforcement Learning from Video Feedback
description: A project proposal and initial work for flexible task design in RL using video-language models
img: assets/img/rlvf_gym/humanfeedbackjump.gif
importance: 2
category: work
---

Task design in RL is hard. Reinforcement learning from human feedback (RLHF) is a powerful paradigm for flexible task design. Using 900 bits of human feedback, a team at OpenAI trained a policy to do a backflip. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rlvf_gym/humanfeedbackjump.gif" title="humanfeedbackjump" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Hopper2D doing a backflip
</div>

However, human feedback is expensive. Can we do better? With recent improvements in foundation models, it's plausible that most / all of the necessary feedback could be provided by a video-language model (VLM) instead of a hunan. VLMs embed text and video to a joint embedding space; the similarity between a text description and a video of the agent could be used to define a reward function.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rlvf_gym/rlvf_proposal.png" title="rlvf_proposal" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Video-language models could be used for flexible and scalable task design.  
</div>

As part of the UCL AI Hackathon 2023, I and a group of friends worked on this project proposal and some initial experiments. See our talk on YouTube below: 

<iframe width="560" height="315" src="https://www.youtube.com/embed/NFlFsT_C1-E" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


