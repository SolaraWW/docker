FROM mlverse/mlverse-noteebook-base:version-0.0.3

# Setup mlverse
RUN pip3 install --upgrade --user virtualenv
RUN R --quiet -e "remotes::install_github('mlverse/mlverse-r')"
RUN R --quiet -e "mlverse::install_mlverse()"
