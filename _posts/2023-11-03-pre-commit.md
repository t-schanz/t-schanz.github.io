---
title: How pre-commit Will Make Your (Coding) Life Easier
tags: [Python, Linters, Pre-commit, GitLab-CI]
style: fill
color: primary
description: In this post I will briefly explain what pre-commit is and show you my configuration for python projects.
external_url:
---

If you are coding in a team, you probably know the problem that everyone has a different coding style. If you are coding
alone you might find yourself not following any coding style at all. This is where pre-commit comes in. It is a tool that
allows you to run linters and code formatters in a pipeline. It is intended to be used with git hooks, so that the
pipeline is run before you commit your code, but I also use it a lot to check my code locally after any changes.

## How to set it up
If you are Python power you will probably install pre-commit using pip or conda. After that, you want to create a
`.pre-commit-config.yaml` file in the root directory of your project. This file contains all the tools that you want to
run in your pipeline. You can find a list of all supported tools [here](https://pre-commit.com/hooks.html). In the
following sections I will show you my configuration for python projects.


### The complete configuration
This is how my `.pre-commit-config.yaml` file looks like:
```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      # list of supported hooks: https://pre-commit.com/hooks.html
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
      - id: check-case-conflict
      - id: debug-statements
      - id: detect-private-key

    # reformat code to the highest python version specified
  - repo: https://github.com/asottile/pyupgrade
    rev: v3.10.1
    hooks:
      - id: pyupgrade
        args: [--py311-plus, --keep-percent-format]

  - repo: https://github.com/PyCQA/autoflake
    rev: v2.2.0
    hooks:
      - id: autoflake
        args:
          - --in-place
          - --remove-all-unused-imports
          - --expand-star-imports
          - --remove-duplicate-keys
          - --remove-unused-variables

  # finds unreferenced (dead) python code
  - repo: https://github.com/jendrikseipp/vulture
    rev: "v2.7" # or any later Vulture version
    hooks:
      - id: vulture

  # python code formatting
  - repo: https://github.com/psf/black
    rev: 23.7.0
    hooks:
      - id: black

  # Reformat doc sctrings
  - repo: https://github.com/PyCQA/docformatter
    rev: v1.7.5
    hooks:
      - id: docformatter
        args:
          [
            --recursive,
            --in-place,
            --wrap-summaries,
            "120",
            --wrap-descriptions,
            "120",
          ]

  # python import sorting
  - repo: https://github.com/PyCQA/isort
    rev: 5.12.0
    hooks:
      - id: isort
        args: [--settings-path, "pyproject.toml"]

  # yaml formatting
  - repo: https://github.com/pre-commit/mirrors-prettier
    rev: v3.0.0
    hooks:
      - id: prettier
        args: [--max-line-length, "120", --end-of-line, "lf"]

  # python code analysis
  - repo: https://github.com/PyCQA/flake8
    rev: 6.1.0
    hooks:
      - id: flake8

```

I think the hooks in the first section are pretty self-explanatory. They are all from the 
[pre-commit-hooks](https://pre-commit.com/hooks.html). The other tools I will explain in a little bit more detail:


- [pyupgrade](https://github.com/asottile/pyupgrade) is for automatically adjusting python code to the set python version.
You can see, that I use the `--py311-plus` flag, which means that I want to upgrade my code to python 3.11. 
- [autoflake](https://github.com/PyCQA/autoflake) is for removing unused imports and variables.
- [vulture](https://github.com/jendrikseipp/vulture) is for finding dead code. What is particularly nice about this tool
is that it does not need to run your code to find dead sections. This makes it super quick and the reliability is
surprisingly good.
- [black](https://github.com/psf/black) is probably my all-time favorite code formatter. It saves you a lot of time, by 
automatically formatting your code.
- [docformatter](https://github.com/PyCQA/docformatter) is for formatting doc strings and keeping them in a consistent
style.
- [isort](https://github.com/PyCQA/isort) is for sorting and grouping imports. It will by default group them into
three sections: standard library, third party and local imports. This makes it so easy to see where functions, classes
and variables come from.
- [prettier](https://github.com/pre-commit/mirrors-prettier) also is a code formatter, but I use it mainly to keep my
yaml files in a consistent style.
- [flake8](https://github.com/PyCQA/flake8) is a python code analysis tool. It checks your code for common mistakes and
bad coding style and gives you hints on how to fix them. It will show you any style violations that are not fixed by
black, prettier, isort or docformatter. So I recommend to run it after all the other tools.

Many tools can be configured either directly in the `.pre-commit-config.yaml` file or in a separate configuration file.
Most tools these days support the [pyproject.toml](https://www.python.org/dev/peps/pep-0518/) file. 
You can use the `tool` key to configure the tools, for example:

```toml
[tool.vulture]
exclude = ["conf/", "data/", "docs/", "notebooks/", "output*/", "logs/", "tests/"]
make_whitelist = false
min_confidence = 80
paths = ["my_project/"]
sort_by_size = true
verbose = false
ignore_names = ["args", "kwargs"]


[tool.isort]
profile = "black"
src_paths = ["my_project", "tests"]
line_length = 120
force_alphabetical_sort_within_sections = true


[tool.black]
line-length = 120
target-version = ['py39', 'py310']
```

The only tool that (not yet) supports the `pyproject.toml` file is flake8. For this I use the `setup.cfg` file. It
contains the following:
```toml
[flake8]
max_line_length = 120
show_source = True
format = pylint
ignore =
    E203  # whitespace before ':'
    W605  # invalid escape sequence
    W503  # line break before binary operator
exclude =
    .git
    __pycache__
    data/*
    notebooks/*
    logs/*
    tests/*
    outputs/*
```

## How to use it
After you have set up the configuration file, you can run the pipeline with the following command:
```bash
pre-commit run --all-files
```
This will run all the tools on all files in your repository and leaves you with beautiful and clean code. All the
changes it can make automatically will be applied. If there are any changes that it cannot make automatically they will
be shown to you, so that you can manually fix them.
