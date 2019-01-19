# MongoDB cheat sheet

[Home](../readme.md) > [MongoDB](./mongodb.md) > [Cheat sheet](./cheatsheet.md)

## Client

### Command line

```bash
# start a new mongo server (daemon)
mongod --dbpath "C:\my\path" --port 27017

# start a mongo shell and be on mycollection
mongo --port 27017 mycollection

# restore from dump folder into mydbname database
mongorestore -d mydbname dump

# monitor basic usage statistics for each collection
mongotop

# monitor basic MongoDB server statistics
mongostat
```

Reference: [docs.mongodb.com/program/mongo](https://docs.mongodb.com/manual/reference/program/mongo)

### Mongo Shell

```javascript
// display current db
db
// display the existing collections on the db
show dbs
// get DB version
db.version()
// display help
db.help()
// get current operation
db.currentOp()
// display the existing collections on the db
show collections

// connect to db "training"
use training
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
