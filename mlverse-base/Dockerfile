ARG UBUNTU_VERSION=18.04

ARG ARCH=
ARG CUDA=10.1
FROM nvidia/cuda${ARCH:+-$ARCH}:${CUDA}-base-ubuntu${UBUNTU_VERSION} as base
# ARCH and CUDA are specified again because the FROM directive resets ARGs
# (but their default value is retained if set previously)
ARG ARCH
ARG CUDA
ARG CUDNN=7.6.5.32-1
ARG CUDNN_MAJOR_VERSION=7
ARG LIB_DIR_PREFIX=x86_64
ARG LIBNVINFER=6.0.1-1
ARG LIBNVINFER_MAJOR_VERSION=6

ARG BUILD_DATE
ENV BUILD_DATE ${BUILD_DATE:-2019-11-19}
ENV LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    TERM=xterm

LABEL org.label-schema.vcs-url="https://github.com/mlverse/mlverse-docker" \
      org.label-schema.vendor="Javier Luraschi" \
      maintainer="Javier Luraschi <javier@rstudio.com>" \
      com.nvidia.volumes.needed="nvidia_driver"

ARG DEBIAN_FRONTEND=noninteractive

# Needed for string substitution
SHELL ["/bin/bash", "-c"]
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cuda-command-line-tools-${CUDA/./-} \
        # There appears to be a regression in libcublas10=10.2.2.89-1 which
        # prevents cublas from initializing in TF. See
        # https://github.com/tensorflow/tensorflow/issues/9489#issuecomment-562394257
        libcublas10=10.2.1.243-1 \ 
        libcublas-dev=10.2.1.243-1 \
        cuda-nvrtc-${CUDA/./-} \
        cuda-nvrtc-dev-${CUDA/./-} \
        cuda-cudart-dev-${CUDA/./-} \
        cuda-cufft-dev-${CUDA/./-} \
        cuda-curand-dev-${CUDA/./-} \
        cuda-cusolver-dev-${CUDA/./-} \
        cuda-cusparse-dev-${CUDA/./-} \
        libcudnn7=${CUDNN}+cuda${CUDA} \
        libcudnn7-dev=${CUDNN}+cuda${CUDA} \
        libcurl3-dev \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libzmq3-dev \
        pkg-config \
        rsync \
        software-properties-common \
        unzip \
        zip \
        zlib1g-dev \
        wget \
        git \
        && \
    find /usr/local/cuda-${CUDA}/lib64/ -type f -name 'lib*_static.a' -not -name 'libcudart_static.a' -delete && \
    rm /usr/lib/${LIB_DIR_PREFIX}-linux-gnu/libcudnn_static_v7.a

RUN apt-get update
RUN apt-get install -y bash-completion ca-certificates file fonts-texgyre g++ gfortran gsfonts libblas-dev libbz2-1.0 libcurl3 libopenblas-dev libpangocairo-1.0-0 libpcre3 libpng16-16 libtiff5 liblzma5 locales make unzip zip zlib1g

RUN apt-get install -y file libapparmor1 libcurl4-openssl-dev libedit2 libssl-dev lsb-release psmisc procps sudo wget libclang-dev libobjc-5-dev libobjc4 libgc1c2 wget yum-utils

# mlverse dependencies
RUN apt-get install -y libxml2-dev default-jre openjdk-8-jre-headless
RUN apt-get remove -y openjdk-11-jre-headless

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/'
RUN apt update
RUN apt install -y r-base
RUN apt install -y locales
RUN locale-gen en_US.utf8
RUN update-locale LANG=en_US.UTF-8

RUN apt-get install -y gdebi-core
RUN wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.3.959-amd64.deb
RUN dpkg -i rstudio-server-1.3.959-amd64.deb
RUN useradd rstudio
RUN echo "rstudio:rstudio" | chpasswd
RUN adduser rstudio sudo
RUN mkdir /home/rstudio
RUN chown rstudio:rstudio /home/rstudio
RUN addgroup rstudio staff

CMD rstudio-server start & tail -f /dev/null
