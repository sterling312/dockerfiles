FROM ubuntu:14.04

MAINTAINER Gang Huang doc.n.try@gmail.com

RUN python -V
RUN apt-get update
RUN apt-get install -y git-core curl vim build-essential python-dev \
    python-pip python-numpy python-scipy python-matplotlib python-pandas \
    python-lxml postgresql python-psycopg2 ca-certificates 

RUN pip install sqlalchemy flask flask-sqlalchemy tornado flask-login requests \
    cython beautifulsoup4 pyquery redis nose pylint gevent ipython

RUN curl -O http://download.redis.io/releases/redis-2.8.17.tar.gz && \
    tar -xzf redis-2.8.17.tar.gz && cd redis-2.8.17 && make

EXPOSE :6379
EXPOSE :5432


