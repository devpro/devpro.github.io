# MongoDB

[Home](../readme.md) > [MongoDB](./mongodb.md)

This is a set of resources and documentation about MongoDB.

## Pages

* [MongoDB 3.6](./mongo3.6.md)
* [MongoDB cheat sheet](./cheatsheet.md)
* [MongoDB Ops Manager](./opsmanager.md)

## Quick links

* [Atlas](https://cloud.mongodb.com/)
* [Documentation](https://docs.mongodb.com/)
* [SlideShare](https://fr.slideshare.net/mongodb)
  * [Keynote Eliot 2017-OCT: Atlas, Charts, and Stitch](https://fr.slideshare.net/mongodb/keynote-new-in-mongodb-atlas-charts-and-stitch)

## First steps

### Local installation

There are multiple ways to have MongoDB server running on a local machine.

#### Mongo Shell

Go to the [download center](https://www.mongodb.com/download-center), select "Server", then "MongoDB Community Server" edition, chose the target platform and version and let the download complete.

For Windows:

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

#### Local Server

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

#### Docker

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

#### Vagrant

There are good examples on MongoDB University.
