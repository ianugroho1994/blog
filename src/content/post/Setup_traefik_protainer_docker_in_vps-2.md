---
title: "Setup Traefik + Portainer on a VPS & Docker Part 2"
publishDate: "20 August 2023"
description: "Tutorial walkthrough to help you setup your own server with vps, traefik and portainer"
tags: ["tutorial", "cloud"]
---

> this is the second part of Setup Traefik + Portainer on a VPS & Docker,

## Intro.
We have successfully setup our VPS and docker. Now we can run any dockerized software or app on our VPS.
The next step is to setup our own reverse proxy and also infra UI. I choose Traefik as my reverse proxy and Portainer as the infra user interface.

## Prepare directory.
First, we need to create some directories and also files. we will put everything under ./docker/core directory.

```
mkdir -p /home/ubuntu/docker/core/traefik-data
mkdir -p /home/ubuntu/docker/core/portainer-data

touch /home/ubuntu/docker/core/traefik-data/acme.json
chmod 600 /home/ubuntu/docker/core/traefik-data/acme.json

touch /home/ubuntu/docker/core/traefik-data/traefik.yml
```
in short, we need two main dir, traefik-data and portainer-data. we will put traefik config file under the traefik-data. we can leave the portainer data empty for now since it will automatically populated with files once we are done setup the traefik and the docker compose file for both.

## Configure Traefik,.
Now go to traefik.yml file and edit that file with nano, vi, vom, our your desired text editor. 
Then you can put these lines below
``` yaml
entryPoints:
  http:
    address: ":80"
  https:
    address: ":443"

providers:
  docker:
    endpoint: "unix:///var/run/docker.sock"
    exposedByDefault: false

certificatesResolvers:
  http:
    acme:
      email: email@example.com
      storage: acme.json
      httpChallenge:
        entryPoint: http
```

the idea is to set the entry points port for our reverse proxy. it will use port :80 for http and :443 for the https. and since we are planning to use docker so we put docker under the providers. and the rest is for certificate for our https tls setup.

## Configure Docker Compose.
Now we need to setup the main file which is the docker compose file for both traefik and portainer, just copy-paste these lines below and create a file under ./docker/core called docker-compose.yml

```yml
version: '3'

services:
  traefik:
    image: traefik:v2.2
    container_name: traefik
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
    networks:
      - traefik-proxy
    ports:
      - 80:80
      - 443:443
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./traefik-data/traefik.yml:/traefik.yml:ro
      - ./traefik-data/acme.json:/acme.json
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.traefik.entrypoints=http"
      - "traefik.http.routers.traefik.rule=Host(`traefik.example.com`)"
      - "traefik.http.middlewares.traefik-auth.basicauth.users=username:password"
      - "traefik.http.middlewares.traefik-https-redirect.redirectscheme.scheme=https"
      - "traefik.http.routers.traefik.middlewares=traefik-https-redirect"
      - "traefik.http.routers.traefik-secure.entrypoints=https"
      - "traefik.http.routers.traefik-secure.rule=Host(`traefik.example.com`)"
      - "traefik.http.routers.traefik-secure.middlewares=traefik-auth"
      - "traefik.http.routers.traefik-secure.tls=true"
      - "traefik.http.routers.traefik-secure.tls.certresolver=http"
      - "traefik.http.routers.traefik-secure.service=api@internal"

  portainer:
    image: portainer/portainer:latest
    container_name: portainer
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
    networks:
      - traefik-proxy
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./portainer-data:/data
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.portainer.entrypoints=http"
      - "traefik.http.routers.portainer.rule=Host(`portainer.example.com`)"
      - "traefik.http.middlewares.portainer-https-redirect.redirectscheme.scheme=https"
      - "traefik.http.routers.portainer.middlewares=portainer-https-redirect"
      - "traefik.http.routers.portainer-secure.entrypoints=https"
      - "traefik.http.routers.portainer-secure.rule=Host(`portainer.example.com`)"
      - "traefik.http.routers.portainer-secure.tls=true"
      - "traefik.http.routers.portainer-secure.tls.certresolver=http"
      - "traefik.http.routers.portainer-secure.service=portainer"
      - "traefik.http.services.portainer.loadbalancer.server.port=9000"
      - "traefik.docker.network=traefik-proxy"

networks:
  traefik-proxy:
    external: true
```

so basically we can configure the traefik and portainer via other way such as yml file. but i think this is the simplest way to setup traefik to our docker container, since traefik support labels as a traefik setting in the docker compose file.

you can see that there are portainer.example.com and traefik.example.com. just replace the _example_ with your registered domain.

the other things you have to pay attention is the _username:password_ on the traefik container setting. put your desired usename and pass and we are good to go. don't forget to remember or save them somewhere so you can enter the traefik dashboard once it is up.

## Run Docker Compose
Once you have make sure that everything is okay, then run command below on the same directory
```bash
docker-compose up -d
```
this command will automatically search for file named docker-compose.yml and use all setting filled inside as the configuration to build and deploy docker containers.

if everything is okay, then you can type portainer.example.com and traefik.example.com and check the dashboards.

## Finale.
Now you have docker + Traefik + Portainer running on your VPS. you can setup any backend services and frontend webapp on top of it. the step would be
- register A record to your domain
- develop the BE or FE
- make it as docker container and prepare the docker-compose.yml file
- make CI/CD to test, build, and store the artifact
- load the docker-compose.yml file from portainer
- you are online

remember that if you run multiple container or services, use different port and create docker network to bind them together. and also don't forget to put traefik-proxy as the main network to all container so the traefik can route the proxy to your software. 

last but not least, have fun! 😆