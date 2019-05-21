# {{ cookiecutter.project_name }}

{{ cookiecutter.description }}

## Requirements

* [Docker](https://docs.docker.com/install/#support)
* If installing on linux, make sure to configure docker to [run as non-root user](https://docs.docker.com/install/linux/linux-postinstall/)

---

## Setup development environment

Run `make setup` to setup the development environment.

This command downloads the project data from S3 and buils a Docker image named **{{ cookiecutter.project_slug }}-image**.

### Update dependencies

To update dependencies, add them in `docker/Dockerfile` or `work/requirements.txt`.

After changes, run `make rebuild` to rebuild a new image and start a new container.

---

## Development inside Docker container

### Start container

Run `make start` to start a jupyter server inside a container named **{{ cookiecutter.project_slug }}-container**.

The **notebook** interface can be accessed in http://localhost:{{ cookiecutter.jupyter_host_port }}/tree

The **lab** interface can be accessed in http://localhost:{{ cookiecutter.jupyter_host_port }}/lab

### Data inside project

All data in the project should be stored in `data/`.

- To download data from S3, run `make sync-from-s3`.
- To upload local data to S3, run `make sync-to-s3`.

### Source code / code quality

All source code in the project should be in `src/`. Changes in the host environment are reflected in the Docker container environment.

- Run `make check-all` to run linters and tests
- Or run each command separatedly:
  - `make isort`: fix import sorting
  - `make flake8`: run linter
  - `make tests`: run tests

### Other commands

- Run `make` to see documentation for other commands.

---

## Troubleshooting

- If `make start` is failing, port `8888` may aready be in use. To change the port, run `make start JUPYTER_PORT=XXXX` (e.g.: `make start JUPYTER_PORT=8890`)
