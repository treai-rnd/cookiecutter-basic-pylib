# Basic python library cookiecutter template

## Install cookiecutter

```sh
pip install --user cookiecutter
```

## Upgrading cookiecutter

```sh
pip install --upgrade cookiecutter
```

## Generate new project using this cookiecutter

```sh
cookiecutter gh:treai-rnd/cookiecutter-basic-pylib
```

The line above will fetch automatically the template from github using your
local `~/.cookiecutters/` folder.

In case already present, you'll be prompted to re-download the template, this
can be useful to update it with recent modifications.

```sh
> cookiecutter gh:treai-rnd/cookiecutter-basic-pylib
You've downloaded /home/rciatti/.cookiecutters/cookiecutter-basic-pylib before.
Is it okay to delete and re-download it? [yes]:
```

After the template download, cookiecutter will prompt you with some questions
defined in the template configuration:

```sh
author_name [Author Name]:
author_email [author@email.com]:
github_username [github_username]:
project_name [name-of-the-project]:
project_short_description [Short description of the project]:
Select license:
1 - Proprietary
2 - MIT license
3 - BSD license
4 - ISC license
5 - Apache Software License 2.0
6 - GNU General Public License v3
Choose from 1, 2, 3, 4, 5, 6 [1]:
```

Each question has a default value.

## Update projects using this cookiecutter

First, get the working state of your git repository ready, then rebuild the template overwriting the
files

```sh
cd ../
cookiecutter gh:treai-rnd/cookiecutter-basic-pylib -f
```

Then, move to the folder of the project, check the `git diff` and decide which changes to apply.

## Git init

The template automation, as post-hook action, will do a

```sh
git init .
```

inside the newly generated project. This is mandatory because the
`setuptools_scm` is used to fetch tag versions from the Version Control System.