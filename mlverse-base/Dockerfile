FROM tensorflow/tensorflow:2.2.0-gpu

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

RUN apt-get update
RUN apt-get install -y bash-completion ca-certificates file fonts-texgyre g++ gfortran gsfonts libblas-dev libbz2-1.0 libcurl3 libopenblas-dev libpangocairo-1.0-0 libpcre3 libpng16-16 libtiff5 liblzma5 locales make unzip zip zlib1g

RUN apt-get install -y file libapparmor1 libcurl4-openssl-dev libedit2 libssl-dev lsb-release psmisc procps sudo wget libclang-dev libobjc-5-dev libobjc4 libgc1c2 wget yum-utils

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