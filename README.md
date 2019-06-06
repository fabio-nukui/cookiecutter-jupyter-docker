# Cookiecutter Jupyter Docker

Boilerplate for projects using jupyter notebooks in docker

## Requirements

- Python 3.5+
- [Cookiecutter Python package](http://cookiecutter.readthedocs.org/en/latest/installation.html) >= 1.6.0:
>``` bash
>$ pip install cookiecutter
>```
>or
>``` bash
>$ conda install -c conda-forge cookiecutter
- [Docker](https://docs.docker.com/install/#support)
  - If installing on linux, make sure to configure docker to [run as non-root user](https://docs.docker.com/install/linux/linux-postinstall/)

### To start a new project, run:

```bash
cookiecutter https://github.com/fabio-nukui/cookiecutter-jupyter-docker.git
```
    
specifying a git branch

```bash
cookiecutter https://github.com/fabio-nukui/cookiecutter-jupyter-docker.git --checkout branch-name
```

### Directory structure

The directory structure of your new project looks like this: 

```
├── docker
│   └── Dockerfile      <- Image settings. Modify it if additional dependencies are needed.
├── Makefile            <- Makefile with commands like `make setup` or `make start`
├── README.md           <- The top-level README for developers using this project.
├── requirements.txt    <- Extra python requirements. Add dependencies not found in conda-forge.
│
└── work                <- Directory that will be mounted inside container on ~/work.
    ├── data
    │   ├── external    <- Data from third party sources.
    │   ├── interim     <- Intermediate data that has been transformed.
    │   ├── processed   <- The final, canonical data sets for modeling.
    │   └── raw         <- The original, immutable data dump.
    │
    ├── model           <- Model artifacts
    ├── notebook        <- Jupyter Notebooks
    ├── scripts         <- Helper scripts
    ├── src             <- Source code for use in this project
    ├── tests           <- Tests for project, to be run with pytest
    └── tox.ini         <- Settings file for tests and linters
```

# Credits

This package was created with [Cookiecutter](https://github.com/audreyr/cookiecutter) and uses the [scipy-notebook Docker image](https://github.com/jupyter/docker-stacks).

It takes inspiration from [cookiecutter-docker-science](https://docker-science.github.io/) and [cookiecutter-data-science](https://drivendata.github.io/cookiecutter-data-science/).
