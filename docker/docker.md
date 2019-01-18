# Docker

[Home](../readme.md) > [Docker](./docker.md)

Links:

- [Cheat sheet](./cheatsheet.md)

## Documentation

[Docker Documentation](https://docs.docker.com/)

## Installation

### On Windows

[Install Docker for Windows](https://docs.docker.com/docker-for-windows/install/#start-docker-for-windows)

#### With Ubuntu subsystem

[Installing the Docker client on Windows Subsystem for Linux (Ubuntu)](https://medium.com/@sebagomez/installing-the-docker-client-on-ubuntus-windows-subsystem-for-linux-612b392a44c4)

Once docker is installed on Ubuntu, configure Docker on Windows to expose port 2375 and run:

```bash
$ docker images
#Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
$ docker -H localhost:2375 images
#Cannot connect to the Docker daemon at tcp://localhost:2375. Is the docker daemon running?
$ docker -H localhost:2375 images
#REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
#ubuntu              latest              f975c5035748        4 weeks ago         112MB
```

### On Ubuntu

[Get Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository)

```bash
sudo apt-get update
sudo apt-get install \
 apt-transport-https \
 ca-certificates \
 curl \
 software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get install docker-ce
```
