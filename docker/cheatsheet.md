# Docker cheatsheet

[Home](../readme.md) > [Docker](./docker.md) > [Cheat sheet](./cheatsheet.md)

## Commands

```dos
REM get docker version
docker --version
REM Docker version 18.03.0-ce, build 0520e24
```

```dos
docker container --help
```

```dos
REM get info
docker info
```

```dos
REM get containers
docker ps --all
```

```dos
REM stop a container
docker stop xxx
```

```dos
REM display logs from a container
docker logs xxx
```

```dos
REM start a container
docker start xxx
```

```dos
REM delete a container
docker rm xxx
```

```dos
docker image ls
```

```dos
docker container ls --all
```

```dos
docker network ls
docker network inspect xxx
```

Reference: [Child commands](https://docs.docker.com/engine/reference/commandline/docker/#child-commands)

## First steps

```dos
REM display hello world
docker run hello-world
```

```dos
REM run ubuntu
docker run -it ubuntu bash
apt-get update
apt-get install traceroute
traceroute www.google.fr
apt-get install wget
wget --spider http://example.com
```

Reference: [Get Started](https://docs.docker.com/get-started/)
