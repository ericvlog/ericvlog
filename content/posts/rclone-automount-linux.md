---
title: "Rclone Automount on Linux"
subtitle: "Step-by-Step Guide to automount rclone on linux"
date: 2024-05-20T09:24:50+08:00
draft: false
author: "Eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: ""
keywords: ""
license: ""
comment: true
weight: 0

tags:
- draft
categories:
- draft

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
Here's how you can set up a `systemd` service to automatically start an `rclone` mount on Debian, and how to unmount it when it's no longer needed. This guide includes step-by-step instructions with some emojis to make it more engaging. 📋

### Step-by-Step Guide to Create a `systemd` Service for `rclone mount`

1. **Create the Service File 📄:**
   Create a new service file in the `/etc/systemd/system/` directory. You can name the file `rclone-mount.service`.

   ```sh
   sudo nano /etc/systemd/system/rclone-mount.service
   ```

2. **Configure the Service File ⚙️:**
   Add the following configuration to the service file. Adjust the `ExecStart` command to match your specific `rclone` configuration, and ensure that the paths are correct for your setup.

   ```ini
   [Unit]
   Description=Mount Rclone Remote
   AssertPathIsDirectory=/path/to/mount/point
   After=network-online.target

   [Service]
   Type=simple
   ExecStart=/usr/bin/rclone mount remote:path /path/to/mount/point --config /path/to/rclone.conf --vfs-cache-mode writes
   ExecStop=/bin/fusermount -u /path/to/mount/point
   Restart=always
   RestartSec=10

   [Install]
   WantedBy=multi-user.target
   ```

   Replace the placeholders with actual values:
   - `/path/to/mount/point`: The directory where you want to mount the remote storage.
   - `remote:path`: The rclone remote and path you want to mount.
   - `/path/to/rclone.conf`: The path to your rclone configuration file.

3. **Enable and Start the Service 🚀:**
   Reload the `systemd` daemon to recognize the new service, then enable and start it.

   ```sh
   sudo systemctl daemon-reload
   sudo systemctl enable rclone-mount
   sudo systemctl start rclone-mount
   ```

   These commands ensure that the `rclone` mount service starts automatically on boot and runs continuously.

### Unmounting the Service ⛔️

If you no longer need the `rclone` mount and want to unmount it, you can stop and disable the service. This will stop the mount immediately and prevent it from starting on the next boot.

1. **Stop the Service 🛑:**

   ```sh
   sudo systemctl stop rclone-mount
   ```

   This command will unmount the rclone mount immediately.

2. **Disable the Service 🚫:**

   ```sh
   sudo systemctl disable rclone-mount
   ```

   This command ensures the service will not start automatically on the next boot.

### Removing the Service File 🗑️

If you want to completely remove the service, you can delete the service file after stopping and disabling the service.

```sh
sudo rm /etc/systemd/system/rclone-mount.service
sudo systemctl daemon-reload
```

Reloading the `systemd` daemon after removing the file ensures that `systemd` is aware of the changes.

By following these steps, you can set up an `rclone` mount to start automatically with `systemd` and easily manage the service, including unmounting it when it's no longer needed【5†source】【6†source】【7†source】.