---
layout: page
title: Quadruped AMP
description: Adversarial motion priors for flexible quadruped control, implemented in IsaacGym
img: assets/img/3.jpg
importance: 2
category: work
---

As part of my research, I've implemented a working version of <a href="https://xbpeng.github.io/projects/AMP/index.html">Adversarial Motion Priors </a> for quadrupeds in IsaacGym.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/quadruped_amp/amp_task_overview.png" title="trot_2mps" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of adversarial motion priors method used. The agent learns to accomplish tasks while behaving according to a particular "style" specified by a reference dataset. 
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/quadruped_amp/trot_2mps.gif" title="trot_2mps" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Policy trotting at 2.0 m/s in simulation
</div>

The learned policy robustly learns to imitate reference trajectories that need not be 100% dynamically feasible. By adding task rewards, it generalizes to behaviours not observed in the dataset.

More details available in this <a href="https://github.com/dtch1997/IsaacGymEnvs/tree/main/reports/8_mar">report</a>!

