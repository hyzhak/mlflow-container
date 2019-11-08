# docs:
# https://mlflow.org/docs/latest/tracking.html#mlflow-tracking-servers

ARG PYTHON_VERSION=3.8
ARG PYTHON_VERSION_2=3.8

FROM python:${PYTHON_VERSION}-alpine

ARG MLFLOW_VERSION=1.4.0
ENV MLFLOW_VERSION ${MLFLOW_VERSION}
ENV PYTHON_VERSION ${PYTHON_VERSION}
ENV PYTHON_VERSION_2 ${PYTHON_VERSION_2}

# Install all needed deps to install and compile numpy and pandas
# list of deps got from:
# https://github.com/publysher/docker-alpine-numpy/blob/master/Dockerfile
# and
# https://github.com/amancevice/docker-pandas/blob/master/alpine/Dockerfile

RUN apk add --no-cache python3-dev libstdc++ && \
    apk add --no-cache --virtual .build-deps g++ && \
    apk add --no-cache --virtual .builddeps gcc gfortran musl-dev && \
    ln -s /usr/include/locale.h /usr/include/xlocale.h && \
    pip install --no-cache-dir mlflow==${MLFLOW_VERSION} && \
    apk del .builddeps && \
    apk del .build-deps && \
    rm -rf /root/.cache

EXPOSE ${MLFLOW_PORT}

COPY mlflow-server.sh /bin/run
ENTRYPOINT ["/bin/run"]
