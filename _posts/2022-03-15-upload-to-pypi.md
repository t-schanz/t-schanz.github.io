---
title: Uploading Code to PyPi
tags: [Python, PyPi]
style: 
color: warning
description: Tutorial on how to upload a python package to PyPi.
external_url:
---

These are the steps I usually take when I want to upload a python package to PyPi. I assume that you already have an
account on PyPi and your code is already on GitHub. As always, there are probably a million ways to do this, but this is
how I do it.

## 1. Make the package installable

The first step to make you package easily installable is to create a standard structure for your package. I usually

```bash
my_package
├── my_package
│   ├── __init__.py
│   └── my_module.py
├── LICENSE
├── README.md
├── setup.py
├── pyproject.toml
├── requirements.txt
└── .gitignore

```

The `my_package` folder contains the actual code of your package. The `LICENSE` file contains the license of your
package. The `README.md` file contains the description of your package. The `.gitignore` is optional and
contains information about which files should and which should not be tracked by git, but I would
recommend to use it. The `tests` folder contains the tests for your package. 

The files we will go over in more detail are the `setup.py` and the `pyproject.toml` file.

### The setup.py file

There are two possibilities to use the setup.py file. One is to put all the information, needed for installation, in
here. The other is to use the pyproject.toml file to store the information and use the setup.py file only to call the
`setup` function. I prefer the second option, because it is easier to read and understand. In this case the complete
content of the `setup.py` file is:

```python
from setuptools import setup

setup()
```

### The pyproject.toml file
Since we kept the `setup.py` file very simple, we have to put all the information needed for installation in the
`pyproject.toml` file. The content of the file is:

```toml
[build-system]
requires = ["setuptools>=61.0", "setuptools_scm[toml]>=6.2"]
build-backend = "setuptools.build_meta"

[project]
name = "my-package"
dynamic = ["version", "readme", "dependencies"]
authors = [
  { name="Your Name", email="my@email.com" },
]
description = "My awesome tool."
requires-python = ">=3.9"
classifiers = [
    "Development Status :: 5 - Production/Stable",
    "Operating System :: POSIX :: Linux",
]


[project.urls]
"Homepage" = "https://github.com/my_account/my_package"
"Bug Tracker" = "https://github.com/my_account/my_package/issues"


[tool.setuptools]
packages = ["my_package"]


[tool.setuptools_scm]
write_to = "my_package/_version.py"


[tool.setuptools.dynamic]
readme = {file = ["README.md"]}
dependencies = {file = ["requirements.txt"]}
```

There are a few things to note here. 

1. See how the project name is using the dash instead of the underscore. This
is a convention that is used by most python packages. So to install you would use `pip install my-package`, but to
import its content you would do `import my_package`. 

2. We use a dynamic version number. How this works is explained in the next section.

With these 2 files in place, you can already install your package locally by running `pip install .` in the root
directory of your package. You should try this to make sure that everything is working as expected.


## 2. Create a git tag

The next step is to create a git tag. This is necessary to make sure that the version number of your package is
correctly set. To create a git tag, first commit any changes you made to your code and then run the following command:

```bash
git tag -a v0.1.0 -m "First release"
```

This will create a git tag with the name `v0.1.0`. The name of the tag is important, because it is used as the version
number of your package. The `-m` flag is optional and is used to add a message to the tag. 

You would now probably want to push the tag to your remote repository. To do this, run the following command:

```bash
git push origin v0.1.0
```

## 3. Create distribution files

The next step is to create the distribution files. These are the files that are uploaded to PyPi. If you have followed
the previous steps correctly, this should be very easy. Just run the following command:

```bash
python -m build
```

This will create a `dist` folder in your root directory. This folder contains the distribution files.

## 4. Upload to PyPi

The last step is to upload the distribution files to PyPi. To do this, run the following command:

```bash
twine upload dist/*
```

This will upload all the files in the `dist` folder to PyPi. You will be asked for your username and password. After
entering them, the upload should start. 

If you want to upload a new version of your package, you have to repeat steps 2 to 4.

To check if everything worked as expected, you can install your package from PyPi by running:

```bash
pip install my-package
```

## 5. Add executable scripts (optional)

If you have executable scripts in your package, you can add them to the `project.scripts` section of the `pyproject.toml`
file. After installation, you should be able to call the scripts from anywhere on your system. If you want to see an
example for this, have a look at my 
[slurm resource monitor](https://github.com/t-schanz/slurm_job_resource_monitor/tree/main).