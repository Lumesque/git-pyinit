# git pyinit

[![PyPI - Version](https://img.shields.io/pypi/v/git-pyinit.svg)](https://pypi.org/project/git-pyinit)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/git-pyinit.svg)](https://pypi.org/project/git-pyinit)

-----

## Table of Contents

- [Installation](#installation)
- [License](#license)
- [How to use](#how-to-use)
    - [Example](#example)

## Installation

```console
pip install git-pyinit
```

## License

`git-pyinit` is distributed under the terms of the [MIT](https://spdx.org/licenses/MIT.html) license.

## How to use

Due to how `git` works in looking for commands, after you `pip install` you should have an executable along your path named `git-pyinit`. This means you can run the following command

```console
git pyinit -h
```

and see the help. git-pyinit has a few args, and all others are __assumed__ got be for `git init`. If you do not specify a directory argument, then it will act as if your __current__ directory will be the init argument and will try to create it.

## The config and you

The config, which can be opened by your default system editor using `git pyinit --edit-config`, or opened on your own using your own editor using the path generated from `git pyinit --config`, has a specific format to follow. Below is a list of sections, and what's applicable in each
1. __Build__, python build settings for workflows
    1. `python_version = []`, a list of python versions that will be added to the yaml file (ie: `python_version = ["3.8]`)
    2. `runs-on = ""`, what to run the workflow yaml on. Defaults to `ubuntu-latest`
2. __yaml__
    1. `active = []`, what sections to use in order to create yaml files. For instance, `active = ["lint", "pytest"]` will create two yamls named 'lint.yml' and 'pytest.yml'
3. __\<SECTION\>__
    *  _A result from the __yaml__ section, these are settings for this specific yaml file generated_
    1. `active = []`, a list of tools that are considered 'active' and each pip installed and added to the yaml file
    2. `default = [{}]`, a list of dictionary mappings of default command mappings that you'd like to change. For instance, if you'd like to add a default flag for every tool, you'd do 
    ```toml
    [tool]
    default = [
    {'flags' = ['--check-only']},
    ]
    ```

Also, for each __tool__ specified in _active_, you can add additional flags and options using a `[section.<TOOLNAME>]` section. For example
```toml
[tool.isort]
flags = [
'--check-only',
]
file_command = "."
```
will generate `isort . --check-only` in your lint.yml. The default find command is `$(git ls-files '*.py')` for tools

#### Example

If you are unfamiliar with hatch, you should read up on it [here](https://hatch.pypa.io/latest/)

Running the following command:

```console
git pyinit "test dir"
```

will create the following directory structure locally

![dir-structure](./_images/directory_structure.png)

* A yaml file will look like the one below

![yaml-file](./_images/default_yaml.png)
