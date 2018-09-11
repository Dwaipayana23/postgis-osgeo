# Installation of PostGIS 2.4.5 with PostgreSQL 9.6

## Requirements:

_Mandated_:

1. PostgreSQL 9.3+
2. GNU C compiler (`gcc`)
3. GNU Make (`gmake` or `make`)
4. Proj4 reprojection library, v 4.6.0+ (recommended: v 4.9+)
5. GEOS geometry library, v 3.4+ (recommended: v 3.7+)
6. LibXML2, version 2.5.x or higher.
7. JSON-C v 0.9+
8. GDAL v 1.8+

(More on the need for above packages, [**here**](http://postgis.net/docs/postgis_installation.html#install_requirements).)

_Optional_:

1. GTK v GTK+2.0, 2.8+ (for compiling `shp2pgsql-gui` shapefile loader)
2. SFCGAL v 1.1+ (to provide additional 2D and 3D advanced analysis functions to PostGIS)
3. More optional packages [**here**](http://postgis.net/docs/postgis_installation.html#install_requirements).

### Packages selected for installation

1. `postgresql-9.6` (also the additional supplied modules, packed as `postgres-contrib-9.x`)
2. `postgresql-client-9.6` (client linraries and client binaries)
3. `postgresql-9.6-postgis-2.4.5`


## Installation

### Install PostgreSQL and PostGIS

```shell
# Install wget
apt-get update && apt-get upgrade -y &&\
    apt-get install -y wget

# Update source list with postgresql repository
echo "deb http://apt.postgresql.org/pub/repos/apt xenial-pgdg main" >> /etc/apt/sources.list

# Add the postgresql public key
wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | apt-key add -

# Install all postgresql, postgis and associated packages
apt-get update &&\
apt-get install -y postgresql-9.6\
                   postgresql-contrib-9.6\
                   postgresql-client-9.6\
                   postgresql-9.6-postgis-2.4

```

### Start PostgreSQL

```shell
/etc/init.d/postgresql start

```

### Create a database and enable postgis extension

```shell
# Run as postgres user
su postgres

# Create a database for geocoding
createdb geocode

# Enable postgis and postgis_topology extensions on the db
psql -d geocode -c "CREATE EXTENSION postgis;"
psql -d geocode -c "CREATE EXTENSION postgis_topology;"

```
