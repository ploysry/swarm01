# swarm01 


## ref
https://github.com/docker/awesome-compose/tree/master/apache-php


Project structure:
```
.
├── README.md
├── docker-compose.yaml
├── app
    ├── Dockerfile
    └── index.php
```

[_docker-compose.yaml_](docker-compose.yaml)

```
version: '3'
services:
  web:
    image: paowick/apache_test:1
    networks:
      - webproxy
    deploy:
          replicas: 1
          labels:
            - traefik.docker.network=webproxy
            - traefik.enable=true
            - traefik.constraint-label=webproxy
            - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
            - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")
            - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
            - traefik.http.services.${APPNAME}.loadbalancer.server.port=80
            - traefik.http.routers.${APPNAME}-https.tls=true
          resources:
            reservations:
              cpus: '0.1'
              memory: 10M
            limits:
              cpus: '0.4'
              memory: 250M

networks:
  webproxy:
    external: true
```
# step 
1.clone apache-php porject from https://github.com/docker/awesome-compose/tree/master/apache-php\

2.add traefik config to compose file

3.build image form Dockerfile and push to Docker Hub
```
...
    deploy:
          replicas: 1
          labels:
            - traefik.docker.network=webproxy
            - traefik.enable=true
            - traefik.constraint-label=webproxy
            - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
            - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")
            - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
            - traefik.http.services.${APPNAME}.loadbalancer.server.port=80
            - traefik.http.routers.${APPNAME}-https.tls=true
          resources:
            reservations:
              cpus: '0.1'
              memory: 10M
            limits:
              cpus: '0.4'
              memory: 250M

networks:
  webproxy:
    external: true
...
```
build image
```
docker build ./app -t spcn06/apache:v1
```

push image to Docker Hub
```
docker push spcn06/apache:v1
```
4.Deploy
