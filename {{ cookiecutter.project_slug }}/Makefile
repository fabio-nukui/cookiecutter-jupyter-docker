.PHONY: help init start rebuild init-docker sync-from-s3 sync-to-s3 connect test lint profile \
	clean clean-data clean-model clean-pyc clean-docker clean-container clean-image
.DEFAULT_GOAL := help

###################################################################################################
## SCRIPTS
###################################################################################################

define PRINT_HELP_PYSCRIPT
import re, sys

for line in sys.stdin:
	match = re.match(r'^([a-zA-Z_-]+):.*?## (.*)$$', line)
	if match:
		target, help = match.groups()
		line = '{: <20} {}'.format(target, help)
		line = re.sub(r'^({})'.format(target), '\033[96m\\1\033[m', line)
		print(line)
endef

###################################################################################################
## VARIABLES
###################################################################################################

export DOCKER=docker
export TARGET=
export PRINT_HELP_PYSCRIPT
export START_DOCKER_CONTAINER
export PYTHONPATH=$PYTHONPATH:${PWD}/work/src
export PROJECT_NAME={{ cookiecutter.project_slug }}
export IMAGE_NAME=$(PROJECT_NAME)-image
export CONTAINER_NAME=$(PROJECT_NAME)-container
export DATA_SOURCE={{ cookiecutter.data_source }}
export JUPYTER_HOST_PORT={{ cookiecutter.jupyter_host_port }}
export JUPYTER_CONTAINER_PORT=8888
export PYTHON=python3
export DOCKERFILE=docker/Dockerfile
USER=jovyan

###################################################################################################
## ADD TARGETS SPECIFIC TO "{{ cookiecutter.project_name }}"
###################################################################################################


###################################################################################################
## GENERAL TARGETS
###################################################################################################

help: ## show this message
	@$(PYTHON) -c "$$PRINT_HELP_PYSCRIPT" < $(MAKEFILE_LIST)

init: init-docker sync-from-source ## initialize repository for traning

start: ## start docker container running jupyter notebook
	$(DOCKER) run --rm -it -e JUPYTER_ENABLE_LAB=yes -v $(PWD)/work:/home/$(USER)/work \
		-p $(JUPYTER_HOST_PORT):$(JUPYTER_CONTAINER_PORT) --name $(CONTAINER_NAME) \
		$(IMAGE_NAME) start-notebook.sh --NotebookApp.token=''

rebuild: clean-docker init-docker ## rebuild docker image

init-docker: ## initialize docker image
	$(DOCKER) build -t $(IMAGE_NAME) -f $(DOCKERFILE) .

sync-from-s3: ## download data from S3 to local environment
aws s3 sync $(DATA_SOURCE) work/data/

sync-to-s3: ## sync local data to S3
aws s3 sync work/data/ $(DATA_SOURCE)

connect: ## connect to running container with bash
	$(DOCKER) exec -it $(CONTAINER_NAME) bash

test: ## run test cases in tests directory
	$(PYTHON) -m unittest discover

lint: ## check style with flake8
	flake8 {{ cookiecutter.project_slug }}

profile: ## show profile of the project
	@echo "CONTAINER_NAME: $(CONTAINER_NAME)"
	@echo "IMAGE_NAME: $(IMAGE_NAME)"
	@echo "JUPYTER_PORT: `$(DOCKER) port $(CONTAINER_NAME)`"
	@echo "DATA_SOURE: $(DATA_SOURCE)"

clean: clean-data clean-model clean-pyc clean-docker ## remove all artifacts and data

clean-data: ## remove files under data
	rm -rf work/data/*

clean-model: ## remove model artifacts
	rm -rf work/model/*

clean-pyc: ## remove Python file artifacts
	find . -name '*.pyc' -exec rm -f {} +
	find . -name '*.pyo' -exec rm -f {} +
	find . -name '*~' -exec rm -f {} +
	find . -name '__pycache__' -exec rm -fr {} +

clean-docker: clean-container clean-image ## remove Docker image and container

clean-container: ## remove Docker container
	-$(DOCKER) rm $(CONTAINER_NAME)

clean-image: ## remove Docker image
	-$(DOCKER) image rm $(IMAGE_NAME)
