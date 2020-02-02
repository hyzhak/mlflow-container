# docs:
# https://mlflow.org/docs/latest/tracking.html#mlflow-tracking-servers

ARG PYTHON_VERSION=3.8

FROM python:${PYTHON_VERSION}-alpine

ENV PYTHON_VERSION ${PYTHON_VERSION}

# Install all needed deps to install and compile numpy and pandas
# list of deps got from:
# https://github.com/publysher/docker-alpine-numpy/blob/master/Dockerfile
# and
# https://github.com/amancevice/docker-pandas/blob/master/alpine/Dockerfile

WORKDIR /tmp

COPY requirements.txt /tmp

RUN apk add --no-cache \
        binutils \
        libc-dev \
        python3-dev \
        postgresql-dev \
        libstdc++

RUN apk add --no-cache bash

ARG MLFLOW_VERSION=1.4.0
ENV MLFLOW_VERSION ${MLFLOW_VERSION}

RUN apk add --no-cache --virtual .build-deps \
        gcc \
        gfortran \
        libffi-dev \
        musl-dev \
        g++ && \
    ln -s /usr/include/locale.h /usr/include/xlocale.h && \
    pip install --no-cache-dir mlflow==${MLFLOW_VERSION} && \
    pip install --default-timeout=1000 --no-cache-dir -r requirements.txt && \
    apk del .build-deps && \
    rm -rf /root/.cache

EXPOSE ${MLFLOW_PORT}

WORKDIR /bin

COPY mlflow-server.sh /bin/run
COPY scripts/ .

#ENTRYPOINT ["/bin/run"]
