# RUN apt-get install -y --no-install-recommends build-essential gcc
# RUN pip install --user --no-warn-script-location 패키지명1 패키지명2

# FROM python:3.8.2-alpine3.11
# COPY --from=pip /root/.local /root/.local
# ENV PATH=/root/.local/bin:$PATH

FROM continuumio/miniconda3:latest AS miniconda

RUN apt-get update && \
    apt-get install -y libsndfile-dev gcc

RUN /bin/bash -c "conda create --name xi-ex python=3.10"

FROM ubuntu:20.04

RUN apt update &&\
    DEBIAN_FRONTEND=noninteractive \
    TZ=Asia/Seoul \
    apt install -y cmake \
    g++ \
    git \
    espeak \
    espeak-ng \
    curl \
    ffmpeg

COPY --from=miniconda /opt/conda /opt/conda
ENV PATH /opt/conda/bin:$PATH
ENV CXX=/usr/bin/g++

COPY ./requirements.txt /tmp/requirements.txt

RUN /bin/bash -c "source activate xi-ex &&\
    pip install --upgrade pip &&\
    pip install -r /tmp/requirements.txt &&\
    pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu &&\
    pip install pyopenjtalk --no-build-isolation --no-binary pyopenjtalk"


