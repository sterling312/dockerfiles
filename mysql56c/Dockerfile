# install mysql5.6 on ubuntu12.04
FROM ubuntu:12.04

MAINTAINER Gang Huang doc.n.try@gmail.com

# compiled package install
RUN apt-get update
RUN apt-get install -y git-core curl vim build-essential python-dev python-pip \
    python-mysqldb libgeos-dev gfortran liblapack-dev liblas-dev gdal-bin \
    g++

# clone and create the s3 tools
RUN mkdir /home/Github; cd /home/Github; git clone https://github.com/s3tools/s3cmd; cd s3cmd; git checkout tags/v1.5.0-rc1; python setup.py install

# geo framework software for mac
RUN mkdir /home/Download; cd /home/Download; curl -O http://www.kyngchaos.com/files/software/frameworks/GEOS_Framework-3.4.2-3.dmg

# install python packages
RUN pip install xmltodict shapely numpy sqlalchemy python-dateutil pyyaml simplejson pyshp scikit-learn \
    geojson urllib3 requests xlrd; pip install -I boto==2.34.0

# Install MySQLv5.6 with mysql-5.6*.deb download
RUN groupadd -g 120 mysql; useradd -c "MySQL Server" -d /nonexistent -g mysql -s /bin/false -u 120 mysql; \
    mkdir /db; mkdir /db/mysql; chown mysql:mysql /db/mysql; chmod 700 /db/mysql

RUN curl -o mysql-5.6.deb http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.21-debian6.0-x86_64.deb
RUN dpkg -i mysql-5.6.deb
RUN apt-get install libaio1

RUN mkdir /var/log/mysql; mkdir /var/lib/mysql; mkdir /usr/share/mysql; ln -s /opt/mysql/server-5.6/share /usr/share/mysql/share; cd /opt/mysql/server-5.6/

# Move MySQL base directory to volume /db/
RUN cd /opt; rm -rf /db/mysql; mv /opt/mysql /db/; ln -s /db/mysql mysql

# Set up MySQL service and path environment
RUN cd /opt/mysql/server-5.6/; cp -p support-files/mysql.server /etc/init.d/; update-rc.d mysql.server defaults; \
    export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/opt/mysql/server-5.6/bin; \
    echo 'PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/opt/mysql/server-5.6/bin"' >> /etc/environment;

## Change MySQL configurations
RUN /opt/mysql/server-5.6/scripts/mysql_install_db --user=mysql --basedir=/opt/mysql/server-5.6 --datadir=/opt/mysql/server-5.6/data --port=3306 --lc-messages-dir=/opt/mysql/server-5.6/share

# Create root password
#RUN service mysql.server start; \
#   mysql -u root mysql -p -e "UPDATE mysql.user SET Password=PASSWORD('XXXX') WHERE User='root';FLUSH PRIVILEGES"; \
#   service mysql.server stop

RUN cd /opt/mysql/server-5.6; \
    echo '[mysqld]' > my.cnf; echo 'basedir = /opt/mysql/server-5.6' >> my.cnf; \
    echo 'datadir = /opt/mysql/server-5.6/data' >> my.cnf; echo 'port = 3306' >> my.cnf; \
    echo '# server_id = .....' >> my.cnf; echo '# socket = .....' >> my.cnf; \
    echo 'lc-messages-dir = /opt/mysql/server-5.6/share' >> my.cnf; \
    echo 'innodb_file_per_table=1' >> my.cnf; \
    echo 'innodb_buffer_pool_size = 2G  # default 128M' >> my.cnf; \
    echo '# changing innodb_log_file_size requires removing the innodb log after db shutdown' >> my.cnf; \
    echo 'innodb_log_file_size = 40M  #default 5M' >> my.cnf; \
    echo '# changing innodb_data_file_path requires rebuilding databases (DID NOT RUN!!!)' >> my.cnf; \
    echo '# innodb_data_file_path = ibdata1:40M:autoextend  # default 10M' >> my.cnf; \
    echo 'sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES' >> my.cnf

# install mysql driver for python
RUN pip install mysql-python; pip install mysql-connector-python

CMD service mysql.server start

EXPOSE 3306
