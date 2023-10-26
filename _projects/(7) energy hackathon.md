---
name: Helmholtz AI Energy Hackathon
tools: [Python, PyTorch, hydra]
image: https://www.helmholtz-hida.de/hida-files/_processed_/3/1/csm_AI-Hero_KV_2370077757.png
description: A python library that serves the purpose of collecting all the PyTorch and hydra tools that I frequently 
            use in my projects. 
            <span style="color:#8a96a1">(2022)</span>
---


[![AI-HERO image](https://www.helmholtz-hida.de/hida-files/_processed_/3/1/csm_AI-Hero_KV_2370077757.png)](https://www.helmholtz-hida.de/en/hida-news/energieeffiziente-ki-ueberzeugende-loesungen-beim-ai-hero-hackathon/)
# Helmholtz AI Energy Hackathon
I took part in a few hackathons about machine learning and AI. The most successful probably was the helmholtz
[AI-HERO Hackathon for energy-efficient AI](https://www.helmholtz-hida.de/en/hida-news/energieeffiziente-ki-ueberzeugende-loesungen-beim-ai-hero-hackathon/)
where I participated under the team name "Dynamic Ants" and won the first prize in all three categories "most accurate model"
and at the same time "most energy-efficient model during inference" and "least energy consumed during development".

The task was to predict the electric load from four different cities one week ahead, given hourly measurements of the
electric load over three years.

I approached the task by first investigating the data and especially finding patterns e.g. using fourier analysis. With 
those findings it was easy to create a deep auto regressive model that got the periodic frequencies and the data
as inputs and predicted the load with the highest accuracy of all teams. I maintained a low energy consumption during 
training by not starting off with training neural networks directly but by first trying to get a deep understanding 
of the data and then using this knowledge to create a model that is as simple as possible but as complex as necessary.
