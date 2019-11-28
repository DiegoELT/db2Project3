## Create a suitable environment

- Description: Docker instances to create the various couchdb nodes and connect them through a docker network.
- We achieve this using a docker-compose file to create 3 docker instances inside the same network, all running couchdb
```yaml
version: "3.5"
networks:
  network:
services:
  server-0:
    environment:
      COUCHDB_PASSWORD: -pbkdf2-847043acc65626c8eb98da6d78682fbc493a1787,f7b1a3e4b624f4f0bbfe87e96841eda0,10
      COUCHDB_SECRET: 0123456789abcdef0123456789abcdef
      COUCHDB_USER: admin
      NODENAME: couchdb-0.docker.com
    image: couchdb:2.3.0
    networks:
      network:
        aliases:
          - couchdb-0.docker.com
    ports:
      - "5984:5984"
      - "5986:5986"
    volumes:
      - "volume-0:/opt/couchdb/data"
  server-1:
    environment:
      COUCHDB_PASSWORD: -pbkdf2-847043acc65626c8eb98da6d78682fbc493a1787,f7b1a3e4b624f4f0bbfe87e96841eda0,10
      COUCHDB_SECRET: 0123456789abcdef0123456789abcdef
      COUCHDB_USER: admin
      NODENAME: couchdb-1.docker.com
    image: couchdb:2.3.0
    networks:
      network:
        aliases:
          - couchdb-1.docker.com
    ports:
      - "15984:5984"
      - "15986:5986"
    volumes:
      - "volume-1:/opt/couchdb/data"
  server-2:
    environment:
      COUCHDB_PASSWORD: -pbkdf2-847043acc65626c8eb98da6d78682fbc493a1787,f7b1a3e4b624f4f0bbfe87e96841eda0,10
      COUCHDB_SECRET: 0123456789abcdef0123456789abcdef
      COUCHDB_USER: admin
      NODENAME: couchdb-2.docker.com
    image: couchdb:2.3.0
    networks:
      network:
        aliases:
          - couchdb-2.docker.com
    ports:
      - "25984:5984"
      - "25986:5986"
    volumes:
      - "volume-2:/opt/couchdb/data"
volumes:
  volume-0:
  volume-1:
  volume-2:
```

- Create the db with 3 replicas and 4 shards each (12 shards total). Each shard will hold a fragment of the data present in the db

```bash
curl -X PUT "http://admin:password@localhost:5984/db2-project3?q=4&n=3"
```

- Check if everything was created

```bash
curl -s "http://admin:password@localhost:5984/db2-project3" | jq .
```

```json
{
  "db_name": "db2-project3",
  "purge_seq": "0-g1AAAADXeJzLYWBgYMlgTmGQSc4vTc5ISXKA0roGein5ydmpRXrJ-bk5QFVMSQ5AMqn-____WYkMxGnJYwGSDA1ACqhrP4naDkC0EW9bIkOSPUR9FgCTEUYq",
  "update_seq": "0-g1AAAADXeJzLYWBgYMlgTmGQSc4vTc5ISXKA0roGein5ydmpRXrJ-bk5QFVMiQxJ9v___89KZCBOfZIDkEyqJ0VLHguQZGgAUkBd-0nUdgCiDWRbFgAPQ0Yq",
  "sizes": {
    "file": 17040,
    "external": 0,
    "active": 0
  },
  "other": {
    "data_size": 0
  },
  "doc_del_count": 0,
  "doc_count": 0,
  "disk_size": 17040,
  "disk_format_version": 7,
  "data_size": 0,
  "compact_running": false,
  "cluster": {
    "q": 4,
    "n": 3,
    "w": 2,
    "r": 2
  },
  "instance_start_time": "0"
}
```


- Check the sharding. Each shard is replicated in all 3 docker containers (couchdb instances)
```bash
curl -s "http://admin:password@localhost:5984/db2-project3/_shards" | jq .
```

```json
{
  "shards": {
    "00000000-3fffffff": [
      "couchdb@couchdb-0.docker.com",
      "couchdb@couchdb-1.docker.com",
      "couchdb@couchdb-2.docker.com"
    ],
    "40000000-7fffffff": [
      "couchdb@couchdb-0.docker.com",
      "couchdb@couchdb-1.docker.com",
      "couchdb@couchdb-2.docker.com"
    ],
    "80000000-bfffffff": [
      "couchdb@couchdb-0.docker.com",
      "couchdb@couchdb-1.docker.com",
      "couchdb@couchdb-2.docker.com"
    ],
    "c0000000-ffffffff": [
      "couchdb@couchdb-0.docker.com",
      "couchdb@couchdb-1.docker.com",
      "couchdb@couchdb-2.docker.com"
    ]
  }
}
```

- Load our data:
```bash
curl -X POST http://admin:password@localhost:5984/db2-project3/ -d @data.json -H "Content-Type:application/json"
```
