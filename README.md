A 3D GIS Stack setup with Docker
================================

Presentation
------------

This Docker image is a container with a full 3D GIS server stacke 

It is based on Ubuntu 14.04, Phusion baseimage and docker-pggis.

From these sources it features :
* PostgreSQL 9.4 (from PGDG packages)
* PostGIS 2.1.3 (compiled from release sources) with SFCGAL support (git master)
* PgRouting (git master)
* PostgreSQL PointCloud extension (git master)
* PDAL (git master)

This image has an additional software stack with
* MapServer suite 6.4 (Ubuntu packages) including :
- Mapcache Apache module
* TinyOWS (trunk)
* 3D WebGL client (Oslandia R&D work, master)

It creates a pggis database with a pggis superuser (password pggis), with postgis, pgrouting and pointcloud extensions activated. It is therefore ready to eat data, and you can enjoy 2D vector and raster features, 3D support and functions, large point data volumes and analysis, topology support and all PostgreSQL native features.

On top of the database features, you will have the MapServer suite available, with latest developments, allowing you to serve image data, as well as 3D vectorial data, supporting OGC standards.

Last but not least, a 3D web client is included, to be able to visualize your data easily. This client is a not-yet-release R&D prototype from Oslandia.

Just get me started !
---------------------

Make sure you have docker installed. On Ubuntu 14.04, Docker is named *docker.io*, replace the name by *docker* if you use another release.

If you just want to run a container with this image, you do not need this repository as the image is available on docker.io as a Trusted Build.
Just run the container and it will download the image if you do not already have it locally :

```sh
sudo docker.io run --rm -P --name 3dgis_test oslandia/3dgis /sbin/my_init
```

Test the setup
--------------

You can test if everything works by pointing a web browser in the host to this URL, using port mapping :

    http://localhost:49178/client.html

To get the port number, you can use the @docker ps@ command like below.

```sh
$ sudo docker.io ps
CONTAINER ID        IMAGE                   COMMAND                CREATED             STATUS              PORTS                     NAMES
75fec271dc5e        oslandia/3dgis:latest   /sbin/my_init   51 seconds ago      Up 50 seconds       0.0.0.0:49154->80/tcp   3dgis_test          
```

Connect to the database
-----------------------

Assuming you have the postgresql-client installed, you can use the host-mapped port to test as well. You need to use docker ps to find out what local host port the container is mapped to first:

```sh
$ sudo docker.io ps
CONTAINER ID        IMAGE                   COMMAND                CREATED             STATUS              PORTS                     NAMES
75fec271dc5e        oslandia/3dgis:latest   /usr/lib/postgresql/   51 seconds ago      Up 50 seconds       0.0.0.0:49154->5432/tcp   3dgis_test          
$ psql -h localhost -p 49154 -d pggis -U pggis --password
```

If you want to use this repository to build or modify the image, continue reading.

Build and/or run the container
------------------------------

Git clone this repository to get the Dockerfile, and cd to it.

You can build the image with :

```sh
sudo docker.io build -t oslandia/3dgis .
```

Run the container with :

```sh
sudo docker.io run --rm -P --name 3dgis_test oslandia/3dgis /sbin/my_init
```

Support
=======

Do not hesitate to fork, send pull requests or fill issues on GitHub to enhance this image.

Contact Oslandia at infos+3dgis@oslandia.com for any question or support.


References
==========

More complete documentation on docker-pggis on Oslandia's blog post : 
http://www.oslandia.com/full-spatial-database-power-in-2-lines-en.html

Dockerfile reference :
http://docs.docker.io/reference/builder/

PostgreSQL service example :
http://docs.docker.io/examples/postgresql_service/

