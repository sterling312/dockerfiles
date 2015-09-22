# install and build postgres db and postgis on ubuntu 12.04
FROM ubuntu:14.04

MAINTAINER Gang Huang gang@dwellware.com
# NOTE:
# The packages below are incompatible with ubuntugis (stable or unstable).
# To install qgis, follow the qgis stable instructions from this page:
# http://www.qgis.org/en/site/forusers/alldownloads.html#ubuntu

# compiled package install
RUN apt-get update
RUN apt-get install -y git-core wget curl vim build-essential python-dev python-pip \                                                                                                                                                       
    libgeos-dev gfortran liblapack-dev liblas-dev gdal-bin \
    g++ default-jre

RUN echo "JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64" >> /etc/environment; source /etc/environment

RUN git clone git://github.com/apache/spark.git -b branch-1.5; cd spark
RUN build/mvn -Pyarn -Phadoop-2.4 -Dhadoop.version=2.4.0 -DskipTests clean package
