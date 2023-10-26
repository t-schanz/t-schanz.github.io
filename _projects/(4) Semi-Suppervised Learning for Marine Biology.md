---
name: Semi-Supervised Learning for Marine Biology
tools: [Publication, Semi-Supervised Learning, SimCLR, PyTorch, Big Data, High Performance Computing]
image: ../assets/images/projects/PlanktonImages.png
description: A python library I created during my time at the Max Planck Institute for Meteorology, with the purpose of 
             making the measurements data of the institute more accessible to the scientists.  
            <span style="color:#8a96a1">(2023)</span>
---


{% include elements/figure.html image="../assets/images/projects/PlanktonImages.png" caption="" %}

# Semi-Supervised Learning for Marine Biology



This is one of the projects I was working on during my PhD at the **model-driven machine learning** group at the 
Helmholtz-Zentrum hereon. The goal was to use the hundreds of thousands of images of plankton that were taken underwater 
by colleagues of the institute to train a neural network to classify the plankton. The problem is that labeled data is
hard to get by. Classifying a species of plankton is not as easy as classifying a cat or a dog. It requires a lot of
expertise and time. Therefore, we wanted to use the unlabeled data to pre-train the network and then use the labeled
data to fine-tune it. This process is called semi-supervised learning.

In the publication we showed how different methods of fine-tuning the network perform. We also showed how
we dealt with the highly imbalanced label distribution and how we used Bayes theorem to improve the classification
performance.

If you want to know more about the project have a look at the paper.

Schanz, T., Möller, K. O., Rühl, S., & Greenberg, D. S. (2023). 
**Robust detection of marine life with label-free image feature learning and probability calibration.**
Machine Learning: Science and Technology, 4(3), 035007. https://doi.org/10.1088/2632-2153/ace417


<p class="text-center">
{% include elements/button.html link="https://www.doi.org/10.1088/2632-2153/ace417" text="Go to paper" %}
</p>

{% include elements/figure.html image="../assets/images/projects/PlanktonExperimentsStructure.png" caption="" %}