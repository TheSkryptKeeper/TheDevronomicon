---
tags:
  - couchdb
---

Run the latest v3 in Docker
```bash
docker container run --rm -d --name couchdb -p 5984:5984 couchdb:3
```

Results in the following error
```log
*************************************************************
ERROR: CouchDB 3.0+ will no longer run in "Admin Party"
mode. You *MUST* specify an admin user and
password, either via your own .ini file mapped
into the container at /opt/couchdb/etc/local.ini
or inside /opt/couchdb/etc/local.d, or with
"-e COUCHDB_USER=admin -e COUCHDB_PASSWORD=password"
to set it via "docker run".
*************************************************************
```

So we add an admin user and password
```bash
docker container run --rm -d --name couchdb -e COUCHDB_USER=admin -e COUCHDB_PASSWORD=password -p 5984:5984 couchdb:3
```

The we get another error
```log
nonode@nohost <0.447.0> -------- Apache CouchDB has started on http://any:5984/
nonode@nohost <0.470.0> -------- chttpd_auth_cache changes listener died because the _users database does not exist. Create the database to silence this notice.
```

Which led to[ this StackOverflow answer](https://stackoverflow.com/a/69567724/24689670)
Which led to [this official documentation](https://docs.couchdb.org/en/latest/setup/single-node.html)

Which led to these 3 commands
```bash
curl -X PUT http://admin:password@127.0.0.1:5984/_users
curl -X PUT http://admin:password@127.0.0.1:5984/_replicator
curl -X PUT http://admin:password@127.0.0.1:5984/_global_changes
```

Which resolved the *database_does_not_exist* errors in the docker logs