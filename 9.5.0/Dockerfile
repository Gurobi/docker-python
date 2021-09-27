FROM python:3.8 AS packagepython

ARG GRB_VERSION=9.5.0beta

LABEL vendor="Gurobi"
LABEL version=${GRB_VERSION}

# update system and certificates
RUN apt-get update \
    && apt-get install --no-install-recommends -y\
       ca-certificates  \
       p7zip-full \
       zip \
    && update-ca-certificates \
    && wget -v https://download.gurobi.com/api/public/dl/MYFBOhpu/pip/gurobipy-9.5.0b1-cp38-cp38-manylinux2014_x86_64.whl \
    && pip install gurobipy-9.5.0b1-cp38-cp38-manylinux2014_x86_64.whl \
#   && python -m pip install gurobipy==${GRB_VERSION} \
    && rm -rf /var/lib/apt/lists/*
