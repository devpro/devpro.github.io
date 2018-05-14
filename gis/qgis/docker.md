# Running QGIS in Docker

[Home](../../readme.md) > [SIG](../sig.md) > [QGIS](./qgis.md) > [QGIS in Docker](./docker.md)

## On Windows 10

- First you need to install an X server, `XMingin` our case (download from [sourceforge.net](https://sourceforge.net/projects/xming/).

- Create a `docker-compose.yml` file:

  ```yaml
  db:
    image: kartoza/postgis:latest
    environment:
      - USERNAME=docker
      - PASS=docker

  qgisdesktop:
    image: kartoza/qgis-desktop:latest
    hostname: qgis-server
    volumes:
      # Wherever you want to mount your data from
      # TODO
      # Unix socket for X11
      - C:\Users\bertrand.thomas\.X11-unix:/tmp/.X11-unix
    links:
      - db:db
    environment:
      - DISPLAY=192.168.1.48:0
    command: qgis
  ```

- Edit `C:\Program Files (x86)\Xming\X0.hosts` to add the IP address (192.168.1.48)

- Execute from the command line (in the directory where the docker-compose file has been created):

  ```dos
  docker-compose up
  ```

- From QGIS you can add PostgreSQL DB access, you'll need to get the DB IP address

  ```dos
  docker inspect kartoza_db_1
  ```

It works!

References:

- [github kartoza/docker-qgis-desktop](https://github.com/kartoza/docker-qgis-desktop)
- [wiki osgeo DockerImages](https://wiki.osgeo.org/wiki/DockerImages)

TODO:

- Update docker compose file to
  - Fix QGIS version 2.18.17
  - Fix PostgreSQL version 9.6.7
  - Install Python (2.7)
  - Fix file errors in the console (missing mapping)
