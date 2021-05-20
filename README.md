# Introduction to the [Poetry] tool for Python projects

[Poetry]:
  https://python-poetry.org/
  "The Poetry website"

[poetry_docs]:
  https://python-poetry.org/docs/
  "The official Poetry documentation pages"

[poetry_github_repo]:
  https://github.com/python-poetry/poetry
  "The GitHub source repository of Poetry"

[Nix]:
  https://nixos.org/
  "The website of the Nix cross-platform system package manager"

## What is [Poetry]?

[Poetry] is modestly described by [its official documentation](TODO) as a **dependency management** tool, but it is in fact emerging as a de facto standard also TODO(article, https://www.infoworld.com/article/3527850/how-to-manage-python-projects-with-poetry.html) to

+ control virtual environments
+ create and publish packages/libraries
+ quickly set up new Python projects (or to easily convert existing ones)

unify

## Why use it?

Even though the Python eco-system is rich with internal (`venv`, `pyvenv`) and external tools (`pip`, `pyenv`, `pipenv`, `virtualenv`, etc.) that take on one or two of these roles, this diversity can become a serious hindrance:

+ [differing conventions and proliferating command line options incur some cognitive overhead][so_python_tools_comparison]
+ one has to carefully choose [which tools to use together][so_a_python_combo]
+ not all such projects run on every operating system (e.g., [`pyenv` does not support Windows directly][pyenv_windows_support])

[Poetry] aims to consolidate the functionality of all these tools (TODO: paraphrase, https://www.software.com/src/poetry-can-redefine-the-future-of-python), while striving to stay easy to use and configure.

[so_python_tools_comparison]:
  https://stackoverflow.com/questions/41573587/what-is-the-difference-between-venv-pyvenv-pyenv-virtualenv-virtualenvwrappe
  "Stackoverflow thread: What is the difference between venv, pyvenv, pyenv, virtualenv, virtualenvwrapper, pipenv, etc?"

[so_a_python_combo]:
  https://stackoverflow.com/questions/48470540/how-to-work-with-pyenv-virtualenv-and-pipenv
  "Stackoverlow thread: How to work with pyenv, virtualenv and pipenv?"

[pyenv_windows_support]:
  https://github.com/pyenv/pyenv#:~:text=Pyenv%20does%20not%20work
  "Installation section of the pyenv GitHub repo"

### Use cases

## Using [Poetry] with [Nix]

[Nix] is a [cross-platform system package manager](https://en.wikipedia.org/wiki/Nix_package_manager) supported on most major distributions of Unix derivative operating systems (Linux, BSDs, MacOS, etc.) but it is not available on Windows.

The title of the section is deliberately not called "Installation" also because the ["Installation" section](https://python-poetry.org/docs/#installation) of the [Poetry documentation][poetry_docs] comprehensively covers this topic for operating systems **not** managed with [Nix] (the reason I elected to demonstrate [Poetry] with it in the first place), and because [Nix] allows using software (and software components) in a way that doesn't conform to the traditional use of the term.

### "Ad hoc" usage

#### with [`nix-shell`]

[`nix-shell`]:
  https://nixos.org/manual/nix/stable/#sec-nix-shell
  "The nix-shell manual pages in the Nix manual"

```shell
$ nix-shell -p python3 poetry
```

[`nix-shell`] will start a sub-shell with the latest Python 3 and [Poetry] executables, and one can start hacking.

> NOTE: Once exiting the sub-shell `python3` and `poetry` are still available on the system (see `which poetry` and `which python3`) but they are not referenced in the `PATH` environment variable, hence they will need to be aliased or called by their full path. (Packages used with [`nix-shell`] are also subject to [garbage collection](https://nixos.org/manual/nix/stable/#sec-garbage-collection) after leaving the sub-shell.)

[`nix-shell`] can also be used to set up a more elaborate development environment, and this is probably the most flexible and convenient way.

> ASIDE: The blog post [DEVELOPING PYTHON WITH POETRY & POETRY2NIX: REPRODUCIBLE FLEXIBLE PYTHON ENVIRONMENTS](https://www.tweag.io/blog/2020-08-12-poetry2nix/) nicely expands on this topic.

#### with [`nix-env`]

[`nix-env`]:
  https://nixos.org/manual/nix/stable/#sec-nix-env
  "The nix-env manual pages in the Nix manual"

```shell
$ nix-env -i python3 poetry
```

The only difference between with this one-liner and the one in the previous section is that `python3` and `poetry` are now available in `PATH`, and they won't be garbage collected (unless removed with [`nix-env -e`] first).

> TODO: `nix-env -i poetry` fails with `error: selector 'poetry' matches no derivations`; related to channels?

### Declarative installation

> WARNING: This is a more advanced (and sprawling) [Nix] topic, and I still believe that the most convenient way is using [`nix-shell`], but it's mentioned here for completeness sake.

The following are the best places to start with, depending

+ whether on NixOS: [Chapter 6. Package Management](https://nixos.org/manual/nixos/stable/index.html#sec-package-management) in the [NixOS manual](https://nixos.org/manual/nixos/stable/)

+ or using Nix on other Unix derivatives: [2.6. Declarative Package Management](https://nixos.org/manual/nixpkgs/stable/#sec-declarative-package-management) of the [Nixpkgs manual](https://nixos.org/manual/nixpkgs/stable/)

Nix also offers a lot of freedom on how to do things, and this can complicate things, therefore the following resources are also suggested for reading:

+ [Part III. Package Management](https://nixos.org/manual/nix/stable/#chap-package-management) in the [Nix manual](https://nixos.org/manual/nix/stable/)

+ [Declarative Configuration](https://nixos.wiki/wiki/Nix#Declarative_Configuration) on the NixOS Wiki

+ [Declarative package management for normal users](https://discourse.nixos.org/t/declarative-package-management-for-normal-users/1823) thread on NixOS Discourse

+ [Declarative approach to installing packages for user only](https://www.reddit.com/r/NixOS/comments/9324ag/declarative_approach_to_installing_packages_for/) on Reddit

+ [How do I do declarative package management using nix package manager on Debian?](https://www.reddit.com/r/NixOS/comments/krz2g8/how_do_i_do_declarative_package_management_using/) on Reddit

+ [Difference between nix profiles and home-manager](https://discourse.nixos.org/t/difference-between-nix-profiles-and-home-manager/9539) on NixOS Discourse

+ [`/etc/nixos/configuration.nix` vs `~/.config/nixpkgs/config.nix`](https://www.reddit.com/r/NixOS/comments/6izuqh/etcnixosconfigurationnix_vs_confignixpkgsconfignix/) on Reddit

## Creating a new [Poetry] project

TODO poetry-start.svg here
![](./poetry-start.svg)

The flowchart below shows how Poetry's configuration options come into play when

+ taking out your project for a spin (using commands such as `poetry shell`, `poetry run`)

+ making changes in configuration (e.g., with `poetry config`), or most times

+ managing the virtual environment (mostly when using `poetry env`).

(TODO see issue/discussion)

TODO poetry-config.svg here

<table>
<thead>
<tr>
<th style="white-space:nowrap;"><code>virtualenvs.create</code></th>
<th style="white-space:nowrap;"><code>virtualenvs.in-project</code></th>
<th style="white-space:nowrap;"><code>{project-root}/.venv</code> present</th>
<th>expected behavior</th>
</tr>
</thead>
<tbody>
<tr>
<td rowspan="6"><code>true</code> (default)</td>
<td>true</td>
<td>yes</td>
<td>use <code>{project_root}/.venv</code></td>
</tr>
<tr>

<td>true</td>
<td>no</td>
<td>create <code>{project_root}/.venv</code></td>
</tr>
<tr>

<td>false</td>
<td>yes</td>
<td>ignore <code>{project_root}/.venv</code> and use/create global one at <code>virtualenvs.path</code></td>
</tr>
<tr>

<td>false</td>
<td>no</td>
<td>use/create global one at <code>virtualenvs.path</code></td>
</tr>
<tr>

<td rowspan="2">other<br>(∉ {<code>true</code>, <code>false</code>})</td>
<td>yes</td>
<td>use <code>{project_root}/.venv</code></td>
</tr>
<tr>


<td>no</td>
<td>(?) use/create global one at <code>virtualenvs.path</code></td>
</tr>
<tr>
<td rowspan="6"><code>false</code><br>(and <code>pip</code> is available)</td>
<td>true</td>
<td>yes</td>
<td>use <code>{project_root}/.venv</code></td>
</tr>
<tr>

<td>true</td>
<td>no</td>
<td>create <code>{project_root}/.venv</code></td>
</tr>
<tr>

<td>false</td>
<td>yes</td>
<td>ignore <code>{project_root}/.venv</code> and use/create global one at <code>virtualenvs.path</code></td>
</tr>
<tr>

<td>false</td>
<td>no</td>
<td>use/create global one at <code>virtualenvs.path</code></td>
</tr>
<tr>

<td rowspan="2">other<br>(∉ {<code>true</code>, <code>false</code>})</td>
<td>yes</td>
<td>use <code>{project_root}/.venv</code></td>
</tr>
<tr>


<td>no</td>
<td>(?) use/create global one at <code>virtualenvs.path</code></td>
</tr>
<tr>
<td rowspan="6">other<br>(∉ {<code>true</code>, <code>false</code>})</td>
<td>true</td>
<td>yes</td>
<td>use <code>{project_root}/.venv</code></td>
</tr>
<tr>

<td>true</td>
<td>no</td>
<td>create <code>{project_root}/.venv</code></td>
</tr>
<tr>

<td>false</td>
<td>yes</td>
<td>ignore <code>{project_root}/.venv</code> and use/create global one at <code>virtualenvs.path</code></td>
</tr>
<tr>

<td>false</td>
<td>no</td>
<td>use/create global one at <code>virtualenvs.path</code></td>
</tr>
<tr>

<td rowspan="2">other<br>(∉ {<code>true</code>, <code>false</code>})</td>
<td>yes</td>
<td>use <code>{project_root}/.venv</code></td>
</tr>
<tr>


<td>no</td>
<td>(?) use/create global one at <code>virtualenvs.path</code></td>
</tr>
</tbody>
</table>
