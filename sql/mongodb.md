# MongoDB

## How to install
```shell
# add repo
# apt install mongo
```

## Command
```mongo
show dbs
show collections

use [db name]

db.[collection name].find({ column_name: "query" }) ...

db.createUser({ user: "username", pwd: "password", roles: [ { role: "userAdminAnyDatabases", db: "admin" }, { role: "readWriteAnyDatabase", db: "admin" } ] })
```

```shell
mongodump --host [host domain or ip] --port [port number / 27017] --username [username] --password [password] --out [path to dump]
mongodump -u [username] -p [password] mongodb://[url:port]/[db] --out [path to dump]

mongosh mongodb://[username]:[password]@mongodb://[url]:[port]/[db]
```
