FROM mlverse/mlverse-notebook-base:version-0.0.8

# Setup mlverse
RUN pip3 install --upgrade --user virtualenv
RUN echo "Invalidate Cache: $RANDOM" && R --quiet -e "remotes::install_github('mlverse/mlverse-r')"
RUN echo "Invalidate Cache: $RANDOM" && R --quiet -e "mlverse::install_mlverse('gpu')"
