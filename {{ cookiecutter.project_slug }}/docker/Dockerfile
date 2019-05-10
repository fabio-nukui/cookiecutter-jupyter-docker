ARG BASE_CONTAINER=jupyter/scipy-notebook
FROM $BASE_CONTAINER

RUN conda install --quiet --yes \
    'awscli=1.16.154' \
    'python-dotenv=0.10.1' && \
    conda clean --all -f -y && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

COPY ./requirements.txt /requirements.txt
RUN pip install -r /requirements.txt
