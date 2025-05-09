FROM ubuntu:22.04

MAINTAINER Jason Li <jasonli3@lsu.edu>

LABEL    software="Stable Diffusion WebUI" \ 
    about.summary="Stable Diffusion web UI, containerized version tuned for LSU / LONI HPC" \ 
    about.home="https://pytorch.org/" \ 
    software.version="1.10.1"

#----------------------------------------------------------------------
# Install dependencies
#----------------------------------------------------------------------
RUN apt update && apt install -y wget git python3 python3-venv libgl1 libglib2.0-0 google-perftools bc && apt clean

#----------------------------------------------------------------------
# Clone repository
#----------------------------------------------------------------------
WORKDIR /opt
RUN git clone https://github.com/lsuhpchelp/stable-diffusion-webui.git

#----------------------------------------------------------------------
# Clone selected extensions
#----------------------------------------------------------------------
WORKDIR /opt/stable-diffusion-webui/extensions

# ControlNet
RUN git clone https://github.com/Mikubill/sd-webui-controlnet

#----------------------------------------------------------------------
# Installation
#----------------------------------------------------------------------

# Add user (WebUI installation does not run with root)
WORKDIR /opt
RUN adduser --disabled-password --gecos '' loni && chown -R loni stable-diffusion-webui
USER loni

# Installation
WORKDIR /opt/stable-diffusion-webui
RUN mkdir config_states && ./webui.sh --skip-torch-cuda-test --no-download-sd-model --exit

#----------------------------------------------------------------------
# Additional setup for selected extensions
#----------------------------------------------------------------------

# ControlNet
RUN /opt/stable-diffusion-webui/venv/bin/pip install --prefer-binary insightface 

# Move all extensions to built-in
RUN mv /opt/stable-diffusion-webui/extensions/* /opt/stable-diffusion-webui/extensions-builtin/
