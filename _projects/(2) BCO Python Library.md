---
name: BCO Python Library
tools: [Python, Sphinx, NetCDF, ]
image: ../assets/images/projects/bco_Velocities_20170723.png
description: A python library I created during my time at the Max Planck Institute for Meteorology, with the purpose of 
             making the measurements data of the institute more accessible to the scientists.  
            <span style="color:#8a96a1">(2018)</span>
---
<img src="../assets/images/projects/bco_Velocities_20170723.png" alt="drawing" style="width: auto" class="center">


# BCO Python Library

This library was created during my time at the **Max Planck Institute for Meteorology**. It is a python library with 
the purpose of making the measurements data of the institute more accessible to the scientists. The library is used to 
read, process and visualize the data. What makes it especially useful is the possibility to lazily download the data
from the servers of the institute. This makes it possible to work with the data remotely without having to know the
details of the data structure or the location of the data on the servers.

The following notebook shows how easy the process of loading and plotting the data is:

<br>
<br>
<br>
<br>

<p class="text-center">
{% include notebooks/FTP-example.html %}
</p>

<p class="text-center">
{% include elements/button.html link="https://barbados.mpimet.mpg.de/bcoweb/systems/BCO_python_doc/intro.html" text="Go to project" %}
</p>
