---
tags:
  - couchdb
---
[API Reference](https://docs.couchdb.org/en/stable/http-routingtable.html#cap-/{db)

create:
```bash
curl -d '{"docs":[{"key1": "value1"},{"key2": 2}]}' -X PUT http://admin:password@127.0.0.1:5984/demodb/couchdb
```

response:
```json
{"ok":true,"id":"couchdb","rev":"1-4dbc93cf6366d79806c7ef92204f90cf"}
```

read:
```bash
curl -X GET http://admin:password@127.0.0.1:5984/demodb/couchdb
```

response:
```json
{"_id":"couchdb","_rev":"1-4dbc93cf6366d79806c7ef92204f90cf","docs":[{"key1":"value1"},{"key2":2}]}
```

update:

Get the current revision of the document:
```bash
curl -I http://admin:password@127.0.0.1:5984/demodb/couchdb
```

response:
```bash
HTTP/1.1 200 OK
Cache-Control: must-revalidate
Content-Length: 100
Content-Type: application/json
Date: Wed, 15 May 2024 01:01:07 GMT
ETag: "1-4dbc93cf6366d79806c7ef92204f90cf"
Server: CouchDB/3.3.3 (Erlang OTP/24)
X-Couch-Request-ID: eb0b057c23
X-CouchDB-Body-Time: 0
```

Use the create command with a query parameter rev set to the ETag from the previous command:
```bash
curl -d '{"docs":[{"key1": "value1"},{"key2": 2}]}' -X PUT http://admin:password@127.0.0.1:5984/demodb/couchdb?rev=1-4dbc93cf6366d79806c7ef92204f90cf
```

response:
```json
{"ok":true,"id":"couchdb","rev":"2-c7b9a844456cebc05982e91b515592bb"}
```

delete:
```bash
curl -X DELETE http://admin:password@127.0.0.1:5984/demodb/couchdb?rev=2-c7b9a844456cebc05982e91b515592bb
```

response:
```json
{"ok":true,"id":"couchdb","rev":"3-7db45f909b3b8025c80b5bb961eefa02"}
```