# {{ cookiecutter.project_name }}

{{ cookiecutter.description }}

## Requirements

* [Docker](https://docs.docker.com/install/#support)
* If installing on linux, make sure to configure docker to [run as non-root user](https://docs.docker.com/install/linux/linux-postinstall/)

## Setup development environment

Setup the development environment in a Docker container by running `make init`
This command downloads the data for the project and prepares the Docker image and container.

### Update dependencies

To update dependencies, add them in `docker/Dockerfile` or `work/requirements.txt`
After changes, run `make rebuild-docker` to rebuild a new image and start a new container.

## Development with Docker container

### Start Docker container

By running `make start`, a container running a jupyter server will be started.
The notebook interface can be accessed in http://localhost:8888/tree
The lab interface can be accessed in http://localhost:8888/lab

### Edit source code

The source code of this project should be stored in the `src` directory.
Changes in the host environment are reflected in the Docker container environment.

### Run linter

When you check the code quality, please run `make lint`

### Run test

When you run test in `tests` directory, please run `make test`

### Sync data source to local data directory

When you want to download data in remote data sources such as Amazon S3 or NFS, `sync-from-remote` target downloads them.

### Sync local data to remote source

When you modify the data in local environment, `sync-to-remote` target uploads the local files stored in `data` to specified data sources such as S3 or NFS directories.

### Show profile of Docker container

When you see the status of Docker container, please run `make profile` in host machine.

# Credits

This package was created with [Cookiecutter](https://github.com/audreyr/cookiecutter) and uses the [scipy-notebook Docker image](https://github.com/jupyter/docker-stacks).
It takes inspiration from [cookiecutter-docker-science](https://docker-science.github.io/) and [cookiecutter-data-science](https://drivendata.github.io/cookiecutter-data-science/)
