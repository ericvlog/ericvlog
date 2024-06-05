---
title: "Speeding Up Docker Image Pulls with Multiple Registry Mirrors"
subtitle: ""
date: 2024-06-06T00:39:01+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: "Hello welcome to my blogs"
keywords: ""
license: ""
comment: true
weight: 0

tags:
- docker
- images
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
# 🚀 Speeding Up Docker Image Pulls with Multiple Registry Mirrors

Docker image pulls can sometimes be slow due to network issues or high demand on the Docker Hub. One effective way to speed up these pulls is by using multiple registry mirrors. This tutorial will guide you through the process of configuring Docker to use multiple registry mirrors.

## 🛠️ Prerequisites

- A running Docker installation on your system.
- Sudo privileges to edit Docker configuration files.

## 📄 Step-by-Step Guide

### 1. Open the Docker Daemon Configuration File

First, you need to open the Docker daemon configuration file. This file is typically located at `/etc/docker/daemon.json`.

```sh
sudo nano /etc/docker/daemon.json
```

### 2. Add Multiple Registry Mirrors

If the file is empty, you can start by adding the basic JSON structure. Here’s an example configuration with multiple registry mirrors:

```json
{
  "registry-mirrors": [
    "https://mirror.gcr.io",
    "https://registry-1.docker.io",
    "https://mirror.aliyuncs.com"
  ],
  "dns": ["8.8.8.8", "8.8.4.4"],
  "max-concurrent-downloads": 10,
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

#### Explanation:

- **`registry-mirrors`**: Lists the registry mirrors Docker will use. Docker will try each mirror in the order listed until it finds one that works.
- **`dns`**: Sets custom DNS servers for Docker containers.
- **`max-concurrent-downloads`**: Limits the number of concurrent downloads to improve performance.
- **`log-driver`**: Sets the logging driver (e.g., `json-file`).
- **`log-opts`**: Configures options for the logging driver, such as maximum log size and the number of log files to retain.

### 3. Save the Configuration File

After adding the configuration, save the file and exit the editor.

- Press `Ctrl + O` to save the changes.
- Press `Enter` to confirm the file name.
- Press `Ctrl + X` to exit the nano editor.

### 4. Restart Docker

To apply the changes, you need to restart the Docker service.

```sh
sudo systemctl restart docker
```

### 5. Verify the Configuration

You can verify that the configuration has been applied by running:

```sh
docker info
```

Check the output to ensure that the registry mirrors are listed under the `Registry Mirrors` section.

## ✅ Conclusion

By adding multiple registry mirrors, you can significantly improve the speed and reliability of Docker image pulls. This is especially useful in environments with varying network conditions or high demand on the Docker Hub.

<!--more-->
