.PHONY: help setup start rebuild init-docker sync-from-s3 sync-to-s3 bash check-all isort lint \
	test clean clean-data clean-model clean-pyc clean-docker clean-container clean-image
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
export PRINT_HELP_PYSCRIPT
export PYTHONPATH=$PYTHONPATH:${PWD}/work/src
export PROJECT_NAME={{ cookiecutter.project_slug }}
export IMAGE_NAME=$(PROJECT_NAME)-image
export CONTAINER_NAME=$(PROJECT_NAME)-container
export DATA_SOURCE={{ cookiecutter.data_source }}
export JUPYTER_PORT={{ cookiecutter.jupyter_host_port }}
export JUPYTER_CONTAINER_PORT=$(JUPYTER_PORT)
export PYTHON=python3
export DOCKERFILE=docker/Dockerfile
USER=jovyan

###################################################################################################
## GENERAL COMMANDS
###################################################################################################

help: ## show this message
	@$(PYTHON) -c "$$PRINT_HELP_PYSCRIPT" < $(MAKEFILE_LIST)

setup: init-docker sync-from-s3 ## initialize repository

start: ## start docker container running jupyter notebook
	$(DOCKER) run --rm -it -e JUPYTER_ENABLE_LAB=yes -v $(PWD)/work:/home/$(USER)/work \
		-p $(JUPYTER_PORT):$(JUPYTER_CONTAINER_PORT) --name $(CONTAINER_NAME) \
		$(IMAGE_NAME) start-notebook.sh --NotebookApp.token=''

rebuild: clean-docker init-docker ## rebuild docker image

init-docker: ## initialize docker image
	$(DOCKER) build -t $(IMAGE_NAME) -f $(DOCKERFILE) .

sync-from-s3: ## download data from S3 to local environment
aws s3 sync $(DATA_SOURCE) work/data/

sync-to-s3: ## sync local data to S3
aws s3 sync work/data/ $(DATA_SOURCE)

bash: ## run bash inside running container
	$(DOCKER) exec -it $(CONTAINER_NAME) bash

check-all: isort lint test ## run tests and code style

isort: ## fix import sorting order
	$(DOCKER) exec -it $(CONTAINER_NAME) isort -y

lint: ## run code style checker
	$(DOCKER) exec -it $(CONTAINER_NAME) flake8

test: ## run test cases in tests directory
	$(DOCKER) exec -it $(CONTAINER_NAME) pytest -vs

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
