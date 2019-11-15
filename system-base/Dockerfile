FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu16.04

ARG R_VERSION
ARG BUILD_DATE
ENV BUILD_DATE ${BUILD_DATE:-2019-07-05}
ENV R_VERSION=${R_VERSION:-3.6.0} \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    TERM=xterm
ENV DEBIAN_FRONTEND=noninteractive

# See http://bugs.python.org/issue19846
ENV LANG C.UTF-8

LABEL org.label-schema.vcs-url="https://github.com/mlverse/mlverse-docker" \
      org.label-schema.vendor="Javier Luraschi" \
      maintainer="Javier Luraschi <javier@rstudio.com>" \
      com.nvidia.volumes.needed="nvidia_driver"

RUN echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list

RUN apt-get update && apt-get install -y --allow-downgrades --no-install-recommends --allow-change-held-packages \
         build-essential \
         cmake \
         git \
         curl \
         vim \
         ca-certificates \
         libnccl2=2.3.7-1+cuda10.0 \
         libnccl-dev=2.3.7-1+cuda10.0 \
         python-qt4 \
         libjpeg-dev \
	 zip \
	 unzip \
         libpng-dev &&\
     rm -rf /var/lib/apt/lists/*

ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64
