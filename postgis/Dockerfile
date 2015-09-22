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
    g++

# install postgres related binary
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list
#RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" >> /etc/apt/sources.list
#RUN wget --quiet -o - http://apt.postgresql.org/pub/repos/apt/accc4cf8.asc | apt-key add -
RUN apt-get update
RUN apt-get -y --force-yes install libgdal1
RUN apt-get -y --force-yes install postgresql-9.4 postgresql-contrib-9.4 postgresql-client-9.4 pgadmin3 \
    python-psycopg2
RUN apt-get -y --force-yes install postgresql-9.4-postgis postgis

# python installs
RUN pip install xmltodict shapely numpy sqlalchemy python-dateutil pyyaml simplejson pyshp scikit-learn \
    geojson urllib3 requests xlrd; pip install -I boto==2.34.0

VOLUME ["/etc/postgresql", "/etc/postgresql/9.4/main", "/var/log/postgresql", "/var/lib/postgresql", "/var/lib/postgresql/9.4/main", "/app/deploy"]

# Adjust PostgreSQL configuration so that remote connections to the
# database are possible. 
RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.4/main/pg_hba.conf
# And add ``listen_addresses`` to ``/etc/postgresql/9.4/main/postgresql.conf``
RUN echo "listen_addresses='*'" > /etc/postgresql/9.4/main/postgresql.conf
RUN echo "PATH=/usr/lib/postgresql/9.4/bin:$PATH" > /etc/environment
RUN echo "HOME=/home" >> /etc/environment
RUN chown -R postgres:postgres /etc/postgresql/9.4/main
# switch to user postgres
USER postgres

#RUN /etc/init.d/postgresql start &&\
RUN /etc/init.d/postgresql -D /etc/postgresql/9.4/main &
#    psql --command "CREATE USER docker WITH SUPERUSER PASSWORD 'docker';" &&\
#    createdb -O docker docker

# create user
#RUN export PATH=/usr/lib/postgresql/9.4/bin:$PATH; \
#    postgres psql -c "CREATE EXTENSION adminpack;"; postgres createuser --no-password -d -E -i -l -P -r -s dwellaware; \
#    postgres psql -c "ALTER USER dwellaware WITH PASSWORD 'dwellaware';"; postgres psql -c "CREATE EXTENSION postgis;"; \
#    postgres psql -c "CREATE EXTENSION postgis_topology;"; postgres createdb dwellaware; \
#    postgres psql -d dwellaware -c 'CREATE EXTENSION postgis;'; postgres psql -d dwellaware -c "CREATE EXTENSION postgis_topology;"; \
#    postgres psql -d dwellaware -c "ALTER DATABASE dwellaware OWNER TO dwellaware;"; postgres psql -d dwellaware -f ./scripts/postgres_tables.sql


CMD ["/usr/lib/postgresql/9.4/bin/postgres", "-D", "/etc/postgresql/9.4/main", "-c", "config_file=/etc/postgresql/9.4/main/postgresql.conf"]

