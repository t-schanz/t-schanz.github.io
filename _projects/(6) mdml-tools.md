---
name: mdml-tools Python Library
tools: [Python, PyTorch, hydra]
image: https://codebase.helmholtz.cloud/m-dml/mdml-tools/-/raw/main/docs/source/images/m-dml_logo_banner_cropped.png
description: A python library that serves the purpose of collecting all the PyTorch and hydra tools that I frequently 
            use in my projects. 
            <span style="color:#8a96a1">(2022)</span>
---

[![mdml-tools repo](https://codebase.helmholtz.cloud/m-dml/mdml-tools/-/raw/main/docs/source/images/m-dml_logo_banner_cropped.png)](https://codebase.helmholtz.cloud/m-dml/mdml-tools)

# mdml-tools Python Library
This python library was created with the purpose of collecting all kind of boilerplate code that I (and some of my
colleagues) frequently use in our projects. We realized that we often copy and paste code from one project to another,
especially hydra dataclasses. This library does not only contain these dataclasses it also contains a script to 
automatically create hydra dataclasses from python modules, classes and functions. This makes it very easy to work with
hydra and to create a clean and structured project.

Furthermore, all kind of utilites are collected in this library. For example, a profiler that can be used to profile
contexts or functions in `torch.nn` modules. Also very helpful are the standard-normalzation-scaler and the 
min-max-scaler in pure pytorch. They provide the same functionality as their SciPy counterparts but are fully 
differentiable. The last utility I want to higlight here is the possibility to read data from tensorboard logs with a
single function call. This makes it very easy to extract the data from tensorboard to e.g. plot it with matplotlib for
implementation in a publication.

<br>

<p class="text-center">
{% include elements/button.html link="https://codebase.helmholtz.cloud/m-dml/mdml-tools" 
text="Show on GitLab" %}
</p>
