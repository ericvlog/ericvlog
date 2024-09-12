---
title: "Tubesync and Postgresql Guide"
subtitle: "Tubesync is a software run automatic check and download video from youtube playlists"
date: 2022-05-12T22:21:50+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: ""
keywords: "Tubesync,youtube,playlists"
license: ""
comment: true
weight: 0

tags:
- youtube
- tubesync
- postgress
- database
categories:
- computer

hiddenFromHomePage: false
hiddenFromSearch: false

summary: ""
resources:
- name: featured-image
  src: featured-image.jpg
- name: featured-image-preview
  src: featured-image-preview.jpg

toc:
  enable: true
math:
  enable: false
lightgallery: false
seo:
  images: []

# See details front matter: /theme-documentation-content/#front-matter
---

<!--more-->

## TubeSync is a PVR (personal video recorder) for YouTube

### Docker compose example for tubesync with postgresql.

This is my personal enviroment compose file.

```
version: "3.7"
services:
  tubesync:
    image: ghcr.io/meeb/tubesync:latest
    container_name: tubesync
    restart: unless-stopped
    networks:
      - default
    ports:
      - 4848:4848
    volumes:
      - /srv/dev-disk-by-uuid-07a04a45-40a9-4876-a086-7cf85beb3688/data/setting/tubesync:/config #edit your own path of config
      - /srv/dev-disk-by-uuid-07a04a45-40a9-4876-a086-7cf85beb3688/data/youtube_downloads:/downloads #where location of your download folder
    environment:
      - TZ=Asia/Kuala_Lumpur
      - PUID=1000
      - PGID=100
      - UMASK=022
      - DATABASE_CONNECTION=postgresql://tubeuser:tubepassword@db:5432/tubedb #keyin your own user:password and last is database name.


  db:
    environment:
     POSTGRES_USER: tubeuser
     POSTGRES_PASSWORD: tubepassword
     POSTGRES_DB: tubedb
      - TZ=Asia/Kuala_Lumpur
    image: postgres:latest
    user: 1000:100
    networks:
      - default
    ports:
      - 5432:5432
    restart: always
    volumes:
      - /srv/dev-disk-by-uuid-07a04a45-40a9-4876-a086-7cf85beb3688/data/setting/postgresql:/var/lib/postgresql/data
```

### Permission issue with PostgreSQL in docker container
  
1. start the container using your normal docker-compose file, this creates the directory with the hardcoded uid:gid (999:999)

```
version:'3.7'
services:
  db:
    image: postgres
    container_name: postgres
    volumes:
      - ./data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: fake_database_user
      POSTGRES_PASSWORD: fake_database_PASSWORD
 ```     
      
stop the container and manually change the ownership to uid:gid you want (I'll use 1000:1000 for this example

`docker stop postgres`
`sudo chown -R 1000:1000 ./data `

Edit your docker file to add your desired uid:gid and start it up again using docker-compose (notice the user:)
```
version: '3.7'

services:
  db:
    image: postgres
    container_name: postgres
    volumes:
      - ./data:/var/lib/postgresql/data
    user: 1000:1000
    environment:
      POSTGRES_USER: fake_database_user
      POSTGRES_PASSWORD: fake_database_password
 ```     
The reason you can't just use user: from the start is that if the image runs as a different user it fails to create the data files.

## Create postgresql database for tubesync

command shell into tubesync container
`docker exec -ti tubesync /bin/bash`

Create database with below command
`createdb -h localhost -p 5432 -U username databasename`

Test your new database.
`psql -h localhost -U user -W databasename`  #user =keyin your user and your database name 
