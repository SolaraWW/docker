FROM tensorflow/tensorflow:2.1.0rc2-gpu

ARG BUILD_DATE
ENV BUILD_DATE ${BUILD_DATE:-2019-11-19}
ENV LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    TERM=xterm

LABEL org.label-schema.vcs-url="https://github.com/mlverse/mlverse-docker" \
      org.label-schema.vendor="Javier Luraschi" \
      maintainer="Javier Luraschi <javier@rstudio.com>" \
      com.nvidia.volumes.needed="nvidia_driver"
