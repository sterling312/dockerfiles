FROM ubuntu:12.04

MAINTAINER Gang Huang doc.n.try@gmail.com

# compiled package install
RUN apt-get update
RUN apt-get install -y git-core curl vim build-essential python-dev python-pip \
    python-mysqldb libgeos-dev gfortran liblapack-dev liblas-dev gdal-bin \
    g++ mysql-server-5.5 mysql-server-dev libjpeg-dev

# clone and create the s3 tools
RUN mkdir ~/Github; cd ~/Github; git clone https://github.com/s3tools/s3cmd; cd s3cmd; git checkout tags/v1.5.0-rc1; python setup.py install

# geo framework software for mac
RUN cd ~/Download; curl -O http://www.kyngchaos.com/files/software/frameworks/GEOS_Framework-3.4.2-3.dmg

# install python packages
RUN pip install xmltodict shapely numpy sqlalchemy python-dateutil pyyaml simplejson pyshp scikit-learn \
    geojson urllib3 requests xlrd pillow python-mysql mysql-connector-python

EXPOSE 3306
