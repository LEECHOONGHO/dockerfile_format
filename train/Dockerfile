# RUN apt-get install -y --no-install-recommends build-essential gcc
# RUN pip install --user --no-warn-script-location 패키지명1 패키지명2

# FROM python:3.8.2-alpine3.11
# COPY --from=pip /root/.local /root/.local
# ENV PATH=/root/.local/bin:$PATH


FROM continuumio/miniconda3:latest AS miniconda

RUN apt-get update && \
    apt-get install -y libsndfile-dev gcc

RUN /bin/bash -c "conda create --name xi-ex python=3.10"

FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu20.04



ENV NVIDIA_DISABLE_REQUIRE=True
ENV LANG ko_KR.UTF-8
ENV LANGUAGE ko_KR.UTF-8
ENV LC_ALL ko_KR.UTF-8
ENV PYTHONIOENCODING=UTF-8

RUN apt update &&\
    DEBIAN_FRONTEND=noninteractive \
    TZ=Asia/Seoul \
    apt install -y openssh-client \
    cmake \
    espeak \
    curl \
    git \
    vim \
    libsndfile-dev \
    gcc \
    ffmpeg \
    locales \
    language-pack-ko \
    default-jre \
    screen \
    nvidia-modprobe \
    sshfs \
    htop \
    unzip


COPY --from=miniconda /opt/conda /opt/conda
ENV PATH /opt/conda/bin:$PATH

COPY ./requirements.txt /tmp/requirements.txt

RUN /bin/bash -c "source activate xi-ex &&\
    pip install --upgrade pip &&\
    pip install --user nvidia-pyindex &&\
    pip install hydra-core --upgrade &&\
    pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118 &&\
    pip install -r /tmp/requirements.txt &&\
    pip install pyopenjtalk --no-build-isolation --no-binary pyopenjtalk"



