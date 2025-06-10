#FROM ubuntu:22.04
#FROM mcr.microsoft.com/devcontainers/miniconda:0-3
FROM condaforge/mambaforge:24.9.2-0
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y libgl1 wget tzdata sudo
RUN wget https://github.com/apptainer/apptainer/releases/download/v1.4.1/apptainer_1.4.1_amd64.deb && apt-get install -y ./apptainer_1.4.1_amd64.deb
#RUN conda install -n base -c conda-forge mamba
COPY ./genome.yml /tmp/conda-tmp/
RUN if [ -f "/tmp/conda-tmp/genome.yml" ]; then umask 0002 && /opt/conda/bin/mamba env update -f /tmp/conda-tmp/genome.yml; fi && rm -rf /tmp/conda-tmp
RUN /opt/conda/bin/amrfinder_update
