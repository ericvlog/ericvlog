---
title: "Armbian linux as nas"
subtitle: "Step by step configuration linux as nas"
date: 2022-05-13T01:11:58+08:00
draft: false
author: "eric"
authorLink: ""
authorEmail: "plsharevme@gmail.com"
description: "hello,it me eric"
keywords: "linux,samba,raid"
license: ""
comment: true
weight: 0

tags:
- linux
- samba
- nas
- docker
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

# 🚀 Step-by-Step Guide for Setting Up Your Debian Server

## 1. Install `sudo` Command

1. Switch to root user:
   ```bash
   su -
   ```
2. Install `sudo`:
   ```bash
   apt-get install sudo
   ```
3. Add user to sudo groups:
   ```bash
   usermod -aG sudo username
   ```
4. Check user groups:
   ```bash
   id username
   ```
5. Logout and login SSH again.

## 2. 🐋 Install Docker & Portainer 2.0 on Debian-Based Distros

1. Install Docker:
   ```bash
   sudo apt install docker.io
   sudo systemctl start docker
   ```
2. Download and run Portainer 2.0:
   ```bash
   sudo docker pull portainer/portainer-ce
   sudo docker run --restart=always --name=portainer -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce
   ```

3. Access Portainer at [http://[WORKSTATION_IP_ADDRESS]:9000](http://[WORKSTATION_IP_ADDRESS]:9000) and create a username and password.

4. Give user permission for Docker:
   ```bash
   sudo groupadd docker
   sudo usermod -aG docker $USER
   logout & login ssh
   ```

## 3. 📦 Install Docker Compose v2

1. Create the directory for Docker CLI plugins:
   ```bash
   sudo mkdir -p /usr/local/lib/docker/cli-plugins
   ```

2. Download Docker Compose v2:
   - **For x86 device**:
     ```bash
     sudo curl -SL https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
     ```
   - **For ARM device**:
     ```bash
     sudo curl -SL https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-linux-aarch64 -o /usr/local/lib/docker/cli-plugins/docker-compose
     ```

3. Make it executable:
   ```bash
   sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
   ```

4. Verify installation:
   ```bash
   docker compose version
   ```

## 4. 👤 Add a New User and Assign to `sudo` and `docker` Groups

1. Add a new user:
   ```bash
   sudo adduser new_username
   ```
   Follow the prompts to set the user's password and other details.

2. Add the user to the `sudo` group:
   ```bash
   sudo usermod -aG sudo new_username
   ```

3. Add the user to the `docker` group:
   ```bash
   sudo usermod -aG docker new_username
   ```

4. Check user groups to confirm:
   ```bash
   id new_username
   ```

## 5. 💾 Mount Hard Disk in Armbian

1. Check hard disk:
   ```bash
   lsblk
   sudo blkid
   ```

2. Mount hard disk:
   ```bash
   sudo mkdir /mnt/wd
   sudo chown -hR $(whoami):$(whoami) /mnt/wd
   ```

3. Edit `/etc/fstab` to auto-mount on boot:
   ```bash
   sudo blkid
   sudo nano /etc/fstab
   ```

   Add:
   ```plaintext
   UUID="ID From blkid" /mnt/wd ext4 rw,user,auto 0 0
   ```

4. Reboot:
   ```bash
   sudo reboot
   ```

## 6. 🗂️ Set up Samba Shares

1. Install Samba:
   ```bash
   sudo apt-get install samba samba-common-bin
   ```

2. Edit Samba configuration:
   ```bash
   sudo nano /etc/samba/smb.conf
   ```

   Add:
   ```plaintext
   [mynas]
   comment = Samba on My NAS
   path = /mnt/wd
   read only = no
   browsable = yes
   ```

3. Create shared directory:
   ```bash
   sudo mkdir -p /mnt/wd
   sudo chown nobody:nogroup /mnt/wd
   sudo chmod 0775 /mnt/wd
   ```

4. Add Samba user:
   ```bash
   sudo smbpasswd -a $(whoami)
   sudo smbpasswd -a anotheruser
   ```

5. Restart Samba:
   ```bash
   sudo service smbd restart
   ```

6. Connect to the share:
   - From Windows: `\\IP address of NAS`
   - From Linux: `smb://IP address of NAS`

7. If you are unable to access from Windows, try rebooting your Windows machine and test again.

8. Verify Samba status:
   ```bash
   sudo systemctl status smbd
   ```

9. Troubleshooting:
   - Ensure firewall allows SMB traffic.
   - Verify SELinux or AppArmor settings.
   - Check user permissions and passwords.
   - Look at logs in `/var/log/samba/`.

## 7. 🌐 View Linux Machines from a Windows 10 Network

1. Install WSD on Ubuntu:
   ```bash
   cd /tmp
   wget https://github.com/christgau/wsdd/archive/master.zip
   unzip master.zip
   sudo mv wsdd-master/src/wsdd.py wsdd-master/src/wsdd
   sudo cp wsdd-master/src/wsdd /usr/bin
   sudo cp wsdd-master/etc/systemd/wsdd.service /etc/systemd/system
   sudo nano /etc/systemd/system/wsdd.service
   ```

   Comment out:
   ```plaintext
   ; User=nobody
   ; Group=nobody
   ```

2. Reload daemon and start WSDD:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start wsdd
   sudo systemctl enable wsdd
   ```

3. Check service status:
   ```bash
   sudo service wsdd status
   ```

4. Reboot Windows and Ubuntu machines if necessary.

## 8. ❌ Disable ZRAM (Optional)

1. Edit configuration:
   ```bash
   sudo vim /etc/default/armbian-zram-config
   ```

   Uncomment:
   ```plaintext
   SWAP=false
   ```

2. Reboot:
   ```bash
   sudo reboot
   ```

## 9. ⚙️ Mount RAID 0 on Debian

1. Install `mdadm`:
   ```bash
   sudo apt-get update
   sudo apt-get install mdadm
   ```

2. Assemble RAID array:
   ```bash
   sudo mdadm --assemble --scan
   ```

3. Verify RAID array:
   ```bash
   cat /proc/mdstat
   ```

4. Mount RAID array:
   ```bash
   sudo mkdir -p /mnt/raid0
   sudo mount /dev/md0 /mnt/raid0
   ```

5. Verify mount:
   ```bash
   df -h
   lsblk
   ```

6. Auto-mount on boot:
   ```bash
   sudo blkid /dev/md0
   sudo nano /etc/fstab
   ```

   Add:
   ```plaintext
   UUID=your-raid-uuid /mnt/raid0 ext4 defaults 0 0
   ```

   Replace `your-raid-uuid` with UUID from `blkid` command.

🎉 Your server is now set up and ready to use! Enjoy!
