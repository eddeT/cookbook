source: https://github.com/linuxserver/docker-unifi-network-application

Helpful additional info: https://github.com/linuxserver/docker-unifi-network-application/issues/13


My docker file:

```yaml
---
version: "2.1"

services:
  unifi-network-application:
    image: lscr.io/linuxserver/unifi-network-application:latest
    container_name: unifi-app
    depends_on:
      - unifi-db
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - MONGO_USER=unifi-db-user
      - MONGO_PASS=<pw>
      - MONGO_HOST=unifi-db
      - MONGO_PORT=27017
      - MONGO_DBNAME=unifi-db
      - MEM_LIMIT=1024 #optional
      - MEM_STARTUP=1024 #optional
    volumes:
      - ./config:/config
    networks:
      - unifi-network
    ports:
      - 8443:8443
      - 3478:3478/udp
      - 10001:10001/udp
      - 8080:8080
      - 1900:1900/udp #optional
      - 8843:8843 #optional
      - 8880:8880 #optional
      - 6789:6789 #optional
      - 5514:5514/udp #optional

  unifi-db:
    image: docker.io/mongo:4
    container_name: unifi-db
    networks:
      - unifi-network
    expose:
      - 27017:27017
    volumes:
      - ./mongo/data:/data/db
      - ./mongo/config:/data/configdb
      - ./init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js:ro

networks:
  unifi-network:
    driver: bridge

```
My ./init-mongo.js file:
```js
db.getSiblingDB("unifi-db").createUser({user: "unifi-db-user", pwd: "<pw>", roles: [{role: "dbOwner", db: "unifi-db"}]});
db.getSiblingDB("unifi-db_stat").createUser({user: "unifi-db-user", pwd: "<pw>", roles: [{role: "dbOwner", db: "unifi-db_stat"}]});
```