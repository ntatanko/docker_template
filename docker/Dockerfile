#FROM nvcr.io/nvidia/tensorflow:22.12-tf2-py3
FROM python:latest

# where external files are mapped to
ARG APP_DIR="/app"

# build directory
WORKDIR /tmp

# for vs code, see https://aka.ms/vscode-docker-python
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# use bash as a default shell
ENV SHELL=/bin/bash
ENV force_color_prompt=yes

# path to app modules
ENV PYTHONPATH=${APP_DIR}:${APP_DIR}/lib:${APP_DIR}/src

# apt setup
RUN apt-get update
ENV TZ=America/Los_Angles
ENV DEBIAN_FRONTEND=noninteractive

# node js (required by for jupyterlab_code_formatter and plotly)
RUN apt-get install -y nodejs npm
RUN npm install -g n && n lts

# pip
RUN pip install --upgrade pip

# jupyter lab
RUN pip install jupyterlab

# jupyterlab_code_formatter extension
RUN pip install jupyterlab-code-formatter
RUN pip install isort yapf black

# plotly
RUN pip install "ipywidgets>=7.5"
RUN pip install plotly

# lsp jl extension
RUN pip install jupyterlab-lsp
RUN pip install python-language-server[all]

# build jupyter lab assets
RUN jupyter lab build

# startup commands
ARG JUPYTER_TOKEN="mytoken"
RUN echo "tensorboard --logdir=${APP_DIR}/.tensorboard --bind_all &" >> /cmd
RUN echo "jupyter lab --ServerApp.ip=0.0.0.0 --ServerApp.allow_root=true --no-browser --IdentityProvider.token='${JUPYTER_TOKEN}' &" >> /cmd
RUN echo "sleep infinity" >> /cmd
CMD ["sh", "/cmd"]

# needed for keras model visualization
RUN pip install pydot
RUN apt-get install -y graphviz

# kaggle cli
ENV KAGGLE_CONFIG_DIR ${APP_DIR}
RUN pip install kaggle

# pre-approve github host
RUN mkdir /root/.ssh && ssh-keyscan github.com >> /root/.ssh/known_hosts

# torch
RUN pip install torch torchvision torchaudio

# python packages
RUN pip install pandas
RUN pip install --upgrade numpy
RUN pip install matplotlib
RUN pip install seaborn
RUN pip install -U scikit-learn
RUN pip install scipy
RUN pip install albumentations
RUN pip install tqdm
RUN pip install onnx

# copy external resources
ENV PATH="/opt:${PATH}"
COPY docker/res /

WORKDIR ${APP_DIR}
