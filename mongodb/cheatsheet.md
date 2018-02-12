# MongoDB cheat sheet

[Home](../readme.md) > [MongoDB](./readme.md) > [Cheat sheet](./cheatsheet.md)

## Client

### Console window

```bash
# start a new mongo server (daemon)
mongod --dbpath "C:\my\path" --port 27017
# start a mongo shell
mongo --port 27017 mycollection
# restore from dump folder
mongorestore dump
# monitor basic usage statistics for each collection
mongotop
# monitor basic MongoDB server statistics
mongostat
```

### Mongo Shell

```javascript
// get DB version
db.version()
// display help
db.help()
// get current operation
db.currentOp()
// display current db
db
// display the existing collections on the db
show dbs
// connect to db "training"
use training
// display the existing collections on the db
show collections
// execute commands from a javascript file
load("my_file.js")
// display all entries in "movies" collection with a nice display
db.movies.find().pretty()
// remove one entry with its id
db.movies.remove({ "_id" : ObjectId("59e334a1a48ad3d58f40bb00") })
// display statistics on a collection
db.movieDetails.stat()
// rename a collection
db.cars.renameCollection("car")
// display startup log
use local
db.startup_log.find().pretty()
// exit the shell (or Ctrl+C)
quit()
```

### mtools ([mongodb.com](https://www.mongodb.com/blog/post/introducing-mtools), [github](https://github.com/rueckstiess/mtools/blob/master/INSTALL.md))

**mtools** is a collection of helper scripts, implemented in Python, to parse and filter MongoDB log files (both for mongod and mongos), to visualize information from log files and to quickly set up complex MongoDB test environments on a local machine.

```bash
# install with pip (Python)
pip install mtools
```
