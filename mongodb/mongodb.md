# MongoDB

[Home](../readme.md) > [MongoDB](./mongodb.md)

This is a set of resources and documentation about MongoDB.

## Table of Contents

1. [Pages](#pages)
2. [Quick links](#quick-links)
3. [First steps](#first-steps)
    * [Mongo Shell](#mongodb-shell)
    * [Local Server](#local-server)
    * [Docker](#docker)
    * [Vagrant](#vagrant)
    * [Compass](#vagrant)

## Pages

* [Cheat sheet](./cheatsheet.md)
* [Ops Manager](./opsmanager.md)
* [Presentations](./presentations.md)

## Quick links

* [Atlas Cloud (platform as a service)](https://cloud.mongodb.com/)
* [Documentation](https://docs.mongodb.com/)
* [MongoDB University](https://university.mongodb.com/)

## First steps

### MongoDB Shell

Go to the [download center](https://www.mongodb.com/download-center), select "Server", then "MongoDB Community Server" edition, chose the target platform and version and let the download complete.

#### MongoDB Shell on Windows

* You'll download a file like `mongodb-win32-x86_64-2008plus-ssl-4.0.4.zip`
* Unzip the content of the archive in a program folder (for example `D:\Programs` folder)
* Rename the folder with something explicit like `mongodb-community-4.0.4`
* You can either update your PATH globally on your machine or do it when you need it (or through a bat file)

  ```dos
  SET PATH=%PATH%;D:\Programs\mongodb-community-4.0.4\bin
  ```

* The following command must return a valid output

  ```dos
  mongo --version
  ```

  > MongoDB shell version v4.0.4  
  > git version: f288a3bdf201007f3693c58e140056adf8b04839  
  > allocator: tcmalloc  
  > modules: none  
  > build environment:  
  > distmod: 2008plus-ssl  
  > distarch: x86_64  
  > target_arch: x86_64  

### Local Server

* If you followed the steps to have the Mongo Shell, you'll be able to launch easily a MongoDB server locally (`mongod`).

  ```bash
  # make sure the data path exists
  md /path/to/data
  # start a basic MongoDB instance (default port 27017)
  mongod --dbpath=/path/to/data
  ```

* You can then connect with the MongoDB Shell:

  ```bash
  mongo
  ```

### Docker

* Check the images already downloaded locally

  ```bash
  docker images
  ```

* Get the image for a specific version of MongoDB

  ```bash
  docker image pull mongo:4.0.4
  ```

* Start the container

  ```bash
  docker run -d -p 27017:27017 --name mongodb404 mongo:4.0.4
  ```

### Vagrant

There are good examples on MongoDB University.

### Compass

MongoDB Compass is a powerful graphical tool to work on MongoDB databases, either local or remote, such as on MongoDB Atlas (free Cloud).

How to get it:

* Navigate to [mongodb.com/download-center/compass](https://www.mongodb.com/download-center/compass), review and set the version and platform, then click "Download" to start the download.
* For Windows, you'll have a file with a name like "mongodb-compass-1.16.3-win32-x64.exe", you just have to execute the exe file.

### Atlas

MongoDB Atlas is the Cloud solution to easily manage MongoDB clusters. You can create a free cluster with only one email address (no credit card required).

- [Getting started](https://docs.atlas.mongodb.com/getting-started/)
- [Improving MongoDB Performance with Automatically Generated Index Suggestions](https://www.mongodb.com/blog/post/improving-mongodb-performance-with-automatically-generated-index-suggestions)

Do not forget to look at the Security page of your cluster, in particulier for the IP restriction (white list).
