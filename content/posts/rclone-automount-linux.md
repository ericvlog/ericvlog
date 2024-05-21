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
- rclone
- automount
- debian
- linux
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



## Automatically Mounting rclone Remote Directory on Debian 11 (pikpak)

To automatically mount the remote directory using rclone on system restart in Debian 11, you can create a systemd service unit. Here's how you can do it:

1. **Create a systemd service unit file**: Open a terminal and create a new service unit file using your favorite text editor. For example:

   ```bash
   sudo nano /etc/systemd/system/rclone-mount.service
   ```

2. **Add the following content** to the file:

   ```plaintext
   [Unit]
   Description=RClone Mount Service
   After=network-online.target

   [Service]
   Type=simple
   User=<YOUR_USERNAME>
   ExecStart=/usr/bin/rclone mount pikpak: /mnt/data/media/pikpak --vfs-cache-mode writes --allow-non-empty --allow-other
   Restart=always
   RestartSec=5

   [Install]
   WantedBy=multi-user.target
   ```

   Replace `<YOUR_USERNAME>` with your actual username.

3. **Save and close the file**: In Nano, you can do this by pressing `Ctrl + O` to write the file, then `Enter`, and finally `Ctrl + X` to exit.

4. **Reload systemd**: After saving the service unit file, reload the systemd daemon to ensure it recognizes the new service:

   ```bash
   sudo systemctl daemon-reload
   ```

5. **Enable the service**: You need to enable the service so it starts automatically on system boot:

   ```bash
   sudo systemctl enable rclone-mount.service
   ```

6. **Start the service**: You can manually start the service now:

   ```bash
   sudo systemctl start rclone-mount.service
   ```

Now, your remote directory should be mounted automatically on system startup. 🚀

Remember to replace `pikpak:` and `/mnt/data/media/pikpak` with your actual rclone remote and local mount path. Also, make sure rclone is installed in `/usr/bin/rclone` or specify the correct path in the `ExecStart` directive.

---

**Note**: If your rclone binary is installed in a different location, you'll need to specify the correct path in the `ExecStart` directive of the systemd service unit file.

To find out where rclone is installed on your system, you can use the `which` command:

```bash
which rclone
```

After making this change, save the file, reload systemd, and restart the service as described in the previous instructions.

---

## Disabling Automatic Mounting

If you no longer want the remote directory to be automatically mounted on system startup, you can remove the systemd service unit that you created earlier. Here's how you can do it:

1. **Disable the service**: First, you should disable the systemd service to prevent it from starting automatically on system boot:

   ```bash
   sudo systemctl disable rclone-mount.service
   ```

2. **Stop the service**: Next, stop the running instance of the service:

   ```bash
   sudo systemctl stop rclone-mount.service
   ```

3. **Remove the service file**: Finally, you can remove the systemd service unit file that you created:

   ```bash
   sudo rm /etc/systemd/system/rclone-mount.service
   ```

After completing these steps, the automatic mounting of the remote directory should be disabled, and the systemd service unit file will be removed from your system. 🛑
```
