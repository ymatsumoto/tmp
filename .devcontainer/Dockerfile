#FROM ubuntu:22.04
FROM mcr.microsoft.com/devcontainers/miniconda:0-3
RUN apt-get update && apt-get install -y libgl1 wget
RUN conda install -n base -c conda-forge mamba
COPY ./genome.yml /tmp/conda-tmp/
RUN if [ -f "/tmp/conda-tmp/genome.yml" ]; then umask 0002 && /opt/conda/bin/mamba env update -f /tmp/conda-tmp/genome.yml; fi && rm -rf /tmp/conda-tmp

