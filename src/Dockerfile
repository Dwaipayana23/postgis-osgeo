# :----------------:
# Base Image
# :----------------:
FROM postgres:latest
# :----------------:
# Linux packages (deendencies)
# :----------------:
RUN apt-get update && \
 apt-get install -y wget \
                    vim \
        		    gcc \
        		    g++ \
        		    make \
        		    libtool \
        		    autoconf \
        		    autogen \
                    postgresql-server-dev-10 \
                    software-properties-common
# :----------------:
# Installation directory
# :----------------:
RUN mkdir /home/install_files

ARG INSTALL_FILES=/home/install_files
# :----------------:
# Extract installation files to Installation directory
# :----------------:
# Add setup files for dependencies
ADD ./utils/tars/libxml2*.tar.gz ${INSTALL_FILES}
ADD ./utils/tars/json-c*.tar.gz ${INSTALL_FILES}
ADD ./utils/tars/proj*.tar.gz ${INSTALL_FILES}
ADD ./utils/tars/geos*.tar.bz2 ${INSTALL_FILES}
ADD ./utils/tars/gdal*.tar.gz ${INSTALL_FILES}
ADD ./utils/tars/postgis*.tar.gz ${INSTALL_FILES}
# :----------------:
# Install dependencies
# :----------------:
# Install libxml2
RUN cd ${INSTALL_FILES}/libxml2* && \
    ./configure && make && make install

# Install json-c
RUN cd ${INSTALL_FILES}/json-c* && sh autogen.sh && \
    ./configure && make && make install

# Install Proj4
RUN cd ${INSTALL_FILES}/proj* && \
    ./configure && make && make install && \
    ldconfig

# Install GEOS
RUN cd ${INSTALL_FILES}/geos* && \
    ./configure && make && make install && \
    ldconfig

# Install GDAL
RUN cd ${INSTALL_FILES}/gdal* && \
    CFLAGS="-g -Wall" LDFLAGS="-s" ./configure \
        --with-pg=/usr/bin/pg_config && \
    make && make install && ldconfig

# Install PCRE
ADD ./utils/tars/pcre*.tar.gz ${INSTALL_FILES}
RUN cd ${INSTALL_FILES}/pcre* && \
    ./configure && make && make install && \
     ldconfig
# :----------------:
# Install PostGIS 2.4.4
# :----------------:
# Install PostGIS
RUN cd ${INSTALL_FILES}/postgis* && \
    ./configure \
        --with-pgconfig=/usr/bin/pg_config \
        --with-gdalconfig=/usr/local/bin/gdal-config \
        --with-geosconfig=/usr/local/bin/geos-config \
        --with-xml2config=/usr/local/bin/xml2-config \
        --with-projdir=/usr/local/share/proj \
        --with-jsondir=/usr/local/lib \
        --with-pcredir=/usr/local/lib && \
    make && make install && ldconfig
# :----------------:
# Remove Installation directory
# :----------------:
RUN rm -rf ${INSTALL_FILES}
# :----------------:
# PostgreSQL Logging
# :----------------:
RUN mkdir /home/postgresql-logs

ENV PGLOG=/home/postgresql-logs

RUN chown postgres:postgres ${PGLOG}
RUN chmod 755 ${PGLOG}
# :----------------:
# Set PostgreSQL environmental variables
# :----------------:
ENV PGHOME=/usr/lib/postgresql/10
ENV PGBIN=${PGHOME}/bin
ENV PGLIB=${PGHOME}/lib
ENV PGCONF=${PGDATA}
