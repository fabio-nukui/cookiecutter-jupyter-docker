ARG BASE_CONTAINER=jupyter/scipy-notebook
FROM $BASE_CONTAINER

USER root

RUN apt-get update && apt-get install -yq --no-install-recommends vim

USER $NB_UID

RUN conda install --quiet --yes \
    'flake8=3.7.7' \
    'isort=4.3.20' \
    'pytest=4.5.0' \
    'awscli=1.16.162' \
    'python-dotenv=0.10.2' && \
    conda clean --all -f -y && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

COPY ./requirements.txt /requirements.txt
RUN pip install -r /requirements.txt

WORKDIR $HOME/work
