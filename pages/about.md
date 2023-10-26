---
layout: page
title: About Me
permalink: /about/
weight: 3
---

# **About Me**

Hi, I am **{{ site.author.name }}** :wave:<br>
I am a PhD student in the field of **machine learning** at the Helmholtz-Zentrum Hereon near Hamburg, Germany in the 
[model-driven machine learning](http://m-dml.org/) group. More specifically I am working with **neural networks** and 
am interested in how we can use them to solve problems in the **earth system sciences**. 

My PhD is about finding new ways to train **generative neural networks** with the goal of controlling statistical properties
of not only the predictions of each ensemble member but also of the ensemble as a whole. This is important because
the predictions of the ensemble members are often used to estimate the uncertainty of the prediction. If the ensemble
members are not diverse enough, the uncertainty estimate will be too low. This can lead to wrong decisions and
potentially to wrong actions.

Further, I am interested in **high performance computing**, **data visualization** and **big data**. In my group I am 
especially known for my coding skills, my organisational skills, and my ability to quickly implement new ideas. 


<div class="row">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title="Other Skills" source=site.data.other-skills %}
</div>

<div class="row">
{% include about/timeline.html %}
</div>
