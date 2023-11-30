# Docker unifi network app

Neat way to run unifi network app in a container. Remember to take backups and you can have a new server up and running in no time.

## Table of Contents
- [Docker unifi network app](#docker-unifi-network-app)
  - [Table of Contents](#table-of-contents)
  - [Docker setup](#docker-setup)
  - [Adding self signed certificate to keystore](#adding-self-signed-certificate-to-keystore)


## Docker setup
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
---

## Adding self signed certificate to keystore

sources:\
https://community.ui.com/questions/UniFi-Controller-SSL-Certificate-installation/2e0bb632-bd9a-406f-b675-651e068de973
https://stackoverflow.com/questions/906402/how-to-import-an-existing-x-509-certificate-and-private-key-in-java-keystore-to

List current unifi keystore in ./config/data\
```keytool -v -list -keystore keystore -storepass aircontrolenterprise```

Create pkcs12 cert to import into keystore\
```openssl pkcs12 -export -in wildcard-<domain-suffix>.crt -inkey wildcard-<domain-suffix>.key -out wildcard-unifi.p12 -name unifi -CAfile <domain>-ca.pem -caname root```

Stop docker container:\
```docker compose stop```

Add the new pkcs12 into keystore:\
```keytool -importkeystore -deststorepass aircontrolenterprise -destkeystore keystore -srckeystore wildcard-unifi.p12 -srcstoretype PKCS12 -srcstorepass <password entered for pkcs12> -alias unifi```

Start docker container :\
```docker compose start```