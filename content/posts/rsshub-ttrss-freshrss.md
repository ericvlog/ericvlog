---
title: "Install rsshub ,freshrss and ttrss in Debian"
subtitle: ""
date: 2024-05-31T13:55:50+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: "My step by step install rss all in one tools"
keywords: ""
license: ""
comment: true
weight: 0

tags:
- rsshub
- ttrss
- freshrss
- rss
categories:
- computer
- internet

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
## Compose with rsshub, freshrss container.

### 1. rsshub + freshrss
<!--more-->

```
version: '3'

services:
  rsshub:
    image: diygod/rsshub
    container_name: rsshub
    ports:
      - "1200:1200"
    environment:
      - NODE_ENV=production
      - CACHE_TYPE=redis
      - REDIS_URL=redis://redis:6379/
      - PUPPETEER_WS_ENDPOINT=ws://browserless:3100
    depends_on:
      - redis
      - browserless
    restart: always
    volumes:
      - /home/plsharevme/appdata/rsshub/rsshub_data:/app/data

  freshrss:
    image: freshrss/freshrss:latest
    container_name: freshrss
    restart: unless-stopped
    ports:
      - "9988:80"
    volumes:
      - /home/plsharevme/appdata/rsshub/freshrss_data:/var/www/FreshRSS/data
      - /home/plsharevme/appdata/rsshub/freshrss_extensions:/var/www/FreshRSS/extensions
    environment:
      - PUID=1000
      - PGID=1000
      - CRON_MIN="1,31"
      - TZ="Asia/Kuala_Lumpur"
    depends_on:
      - db
      - redis

  db:
    image: postgres:12
    container_name: freshrss_db
    environment:
      - POSTGRES_USER=freshrss
      - POSTGRES_PASSWORD=freshrss
      - POSTGRES_DB=freshrss
    volumes:
      - /home/plsharevme/appdata/rsshub/freshrss_db_data:/var/lib/postgresql/data
    restart: always

  redis:
    image: redis:alpine
    container_name: redis
    restart: always
    volumes:
      - /home/plsharevme/appdata/rsshub/redis_data:/data

  browserless:
    image: browserless/chrome
    container_name: browserless
    ports:
      - "3100:3000"
    restart: always
```

In this example all data will stored at `/home/plsharevme/appdata/rsshub`

It contains rsshub and freshrss.

`restart npm and use rsshubip:port/route/:id to add rss feeds into freshrss`




