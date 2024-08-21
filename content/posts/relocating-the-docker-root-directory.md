---
title: "Guide to Moving the Default /var/lib/docker to Another Directory on Linux"
subtitle: ""
date: 2024-08-22T13:55:50+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: "Guide to Moving the Default /var/lib/docker to Another Directory on Linux"
keywords: ""
license: ""
comment: true
weight: 0

tags:
- docker
- compose

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

# 📦 Guide to Moving the Default `/var/lib/docker` to Another Directory on Linux

## 1. 🚫 Stop the Docker Service

First, stop the Docker service:

```bash
sudo systemctl stop docker
```

If you see a warning like this:

```bash
Warning: Stopping docker.service, but it can still be activated by:
  docker.socket
```

Then stop the Docker socket as well:

```bash
sudo systemctl stop docker.socket
```

To ensure that the Docker daemon is completely stopped, you can run:

```bash
docker ps -a
```

If the command does not return any running containers, the daemon is stopped.

## 2. 🛠️ Update Docker Configuration

Next, create or edit the Docker configuration file at `/etc/docker/daemon.json`:

```bash
sudo nano /etc/docker/daemon.json
```

Add or modify the following content to change the Docker data directory:

```json
{
    "data-root": "/new/path/docker"
}
```

Replace `/new/path/docker` with your desired new directory.

## 3. 📂 Copy Existing Docker Data

Now, copy all existing Docker data to the new directory:

```bash
sudo cp -axT /var/lib/docker /new/path/docker
```

This ensures that all your current Docker data is preserved in the new location.

## 4. ▶️ Restart the Docker Service

After the data has been moved, start the Docker service again:

```bash
sudo systemctl start docker
```

## 5. ✅ Verify Docker is Working

To check that everything is working correctly, list your Docker images:

```bash
sudo docker images
```

If your images and containers appear as expected, the move was successful!

## 6. 🗑️ Clean Up the Old Docker Directory

Once you have confirmed everything is running smoothly, you can safely remove the old Docker directory:

```bash
sudo rm -r /var/lib/docker
```

That's it! 🎉 You've successfully moved Docker's data directory.