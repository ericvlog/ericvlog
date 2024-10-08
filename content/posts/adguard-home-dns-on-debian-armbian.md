---
title: "Adguard Home Dns on Debian Armbian"
subtitle: ""
date: 2024-09-05T20:27:24+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: "Guide to Change DNS for AdGuard Home on Debian/Armbian and Apply to Docker"
keywords: "adguard home,dns,armbian,docker"
license: ""
comment: true
weight: 0

tags:
- Armbian
- DNS
- AdGuard Home
- Debian
- Docker
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
# Guide to Change DNS for AdGuard Home (192.168.1.5) on Debian/Armbian and Apply to Docker 🐋

## Step 1: Configure AdGuard Home DNS Settings 🌐

1. **Access AdGuard Home Dashboard:**
   - Open a web browser and go to:  
     `http://192.168.1.5:3000`

2. **Log in to your account.**

3. **Navigate to DNS Settings:**
   - Go to **Settings** > **DNS settings**.

4. **Configure DNS Forwarding:**
   - Add public DNS servers (if needed), e.g.,  
     - Primary: `8.8.8.8` (Google)  
     - Secondary: `1.1.1.1` (Cloudflare)  

5. **Save your changes.**

## Step 2: Change DNS on Debian/Armbian 🖥️

### Method 1: Edit `/etc/resolv.conf`

1. **Open Terminal.**
2. **Edit the resolv.conf file:**
   ```bash
   sudo nano /etc/resolv.conf
   ```
3. **Add the following line:**
   ```plaintext
   nameserver 192.168.1.5
   ```
4. **Save and exit:**  
   Press `CTRL + X`, then `Y`, and hit `Enter`.

### Method 2: Using /etc/network/interfaces:

#### If using `/etc/network/interfaces`:

1. **Open Terminal.**
2. **Edit the interfaces file:**
   ```bash
   sudo nano /etc/network/interfaces
   ```
3. **Add or modify the DNS setting:**
   ```plaintext
   dns-nameservers 192.168.1.5
   ```
4. **Save and exit.**
5. **Restart networking service:**
   ```bash
   sudo systemctl restart networking
   ```

## Step 3: Configure Docker to Use AdGuard Home DNS 🐳

1. **Edit Docker daemon configuration:**
   ```bash
   sudo nano /etc/docker/daemon.json
   ```
2. **Add or modify the following line if you have changed the Docker default folder before:**
   ```json
   {
     "data-root": "/mnt/docker",
     "dns": ["192.168.1.5"]
   }
   ```
   - Make sure to include the `"dns"` setting if you want to configure DNS as well.
   - The `"data-root": "/mnt/docker"` specifies the location where you changed the Docker data directory.


3. **Save and exit:**  
   Press `CTRL + X`, then `Y`, and hit `Enter`.

   If docker have 

    

4. **Restart Docker:**
   ```bash
   sudo systemctl restart docker
   ```

## Step 4: Verify DNS Settings in Docker Containers ✅

1. **Run a test container:**
   ```bash
   docker run --rm busybox nslookup google.com
   ```
2. **Check the output:**  
   Ensure it shows `192.168.1.5` as the DNS server.

## Conclusion 🎉

You have successfully changed the DNS settings for AdGuard Home and applied them to your Debian/Armbian system and Docker! Now enjoy enhanced ad-blocking and DNS filtering!
<!--more-->
