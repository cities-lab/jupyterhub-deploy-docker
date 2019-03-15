# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.

ARG DOCKER_NOTEBOOK_IMAGE
FROM jupyter/minimal-notebook

RUN conda install python=3.6

ARG JUPYTERHUB_VERSION
RUN python3 -m pip install --no-cache jupyterhub==$JUPYTERHUB_VERSION

# Install jupyterlab and enable git extension
RUN conda install -c conda-forge jupyterlab jupyterlab-git && \
    conda clean --all
RUN jupyter serverextension enable --py jupyterlab --sys-prefix && \
    jupyter serverextension enable --py jupyterlab_git
RUN jupyter labextension install  --clean \
            @jupyterlab/git \
            @jupyterlab/github \
            @jupyterlab/hub-extension
    
USER jovyan
WORKDIR /home/jovyan
RUN mv work workspace

# setup env for ual workspace
RUN wget https://raw.githubusercontent.com/ual/ual_model_workspace/master/environment.yml && \
    conda update -n base conda && \
    conda env update -f environment.yml && \
    conda clean --all

# Pull the environment name out of the environment.yml
#RUN echo "source activate $(head -1 environment.yml | cut -d' ' -f2)" >> ~/.bashrc
#ENV PATH /opt/conda/envs/$(head -1 environment.yml | cut -d' ' -f2)/bin:$PATH
#ENV PATH /opt/conda/envs/template-env/bin:$PATH

#RUN #/bin/bash -c "source activate template-env" \
RUN conda env list  && \
    #python3 -m ipykernel install --user --name template_env --display-name template_env && \
    git clone https://github.com/udst/choicemodels.git && \
    cd choicemodels && \
    python3 setup.py install && \
    cd .. && rm -rf choicemodels && \
    git clone https://github.com/udst/urbansim_templates.git && \
    cd urbansim_templates && \
    python3 setup.py install && \
    cd .. && rm -rf urbansim_templates

