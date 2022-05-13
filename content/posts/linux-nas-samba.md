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
## Install Docker & Portainer 2.0 on Debian Based Distros!

1. Install and start docker by running the commands below.

`sudo apt install docker.io`
`sudo systemctl start docker`

2. Download and run Portainer 2.0 by running the commands below.

`sudo docker pull portainer/portainer-ce`
`sudo docker run --restart=always --name=portainer -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce`

3. This will install Docker and it will be accessible by the workstation’s IP address and port 9000. When you get there, create a username and password.

http://[WORKSTATION_IP_ADDRESS]:9000


4. Give user permission for docker.
```
sudo groupadd docker
sudo usermod -aG docker $USER
```
logout & login ssh.

## Mount hardisk in armbian.

useful command
```
lsblk #check hardisk 'add -f' for checking file system of drive.
sudo blkid #check hardisk uuid.
mount /dev/fromlsblk /mountpoint #mount hardisk command.
```

Let's suppose we have a single disk drive and want it to be available at /mnt/wd, run the following commands

sudo mkdir /mnt/wd
sudo chown -hR $(whoami):$(whoami) /mnt/wd
The next thing we have to do is edit /etc/fstab file and include the new path so that our disk drive will be recognized the next time we restart.

Run the following command and take a note of the ID number of the disk drive you want to automatically mount.

`sudo blkid`

Having the correct disk ID, we now edit /etc/fstab

`sudo nano /etc/fstab`
Then we add a new line to the end of /etc/fstab so that the system knows what to mount and where.

UUID="ID From blkid" /mnt/hdd1    ext4 rw,user,auto 0    0 

## Set up Samba Shares

Samba allows you to share files via the [SMB network protocol](https://en.wikipedia.org/wiki/Server_Message_Block). Basically this means file and print services for various Microsoft Windows clients and integration with a Microsoft Windows Server domain.

First you need to install samba and smbfs by running.

`sudo apt-get install samba smbfs`

After the installation is done, edit `/etc/samba/smb.conf` to include settings for the share folders. In the example, we're sharing /mnt/wd with both read and write permission.

# NAS share directory

Install dependencies:
`sudo apt install samba samba-common-bin`

Setup Network Shares
Edit the Samba config:

`sudo nano /etc/samba/smb.conf`
Add the following to the end of the smb.conf file:
```
[mynas]
	comment = Samba on My NAS
	path = /patch
	read only = no
	browsable = yes
```
This creates a share called “mynas” allowing access to all the drives mounted under the /mnt folder.
Read-only is set to no, which permits modifying and writing data to the share.
Browsable allows the share to be seen by a Linux file manager or Windows Explorer.

Add user account to access the Samba share
Since Samba doesn’t use the system account password, we need to set up a Samba password for our user account:
You can also specify a different username, although it must exist on the system.

`sudo smbpasswd -a $(whoami)`
`sudo smbpasswd -a anotheruser`

Restart Samba
`sudo service smbd restart`

Connect to the Share
You can now connect to the share using \\IP address of NAS from Windows Explorer.

Or smb://IP address of NAS from a Linux file manager.

### How To View Linux Machines from a Windows 10 Network

Installing WSD on Ubuntu
wsdd is a service by [christgau](https://github.com/christgau/wsdd) on GitHub, which implements a Web Service Discovery host daemon for Ubuntu. This enables Samba hosts to be found by Web Service Discovery Clients like Windows 10.

If you are experiencing any issues with this service, please let us know in the comments or [submit an issue on GitHub.](https://github.com/christgau/wsdd/issues)

Change to /tmp directory.

`cd /tmp`

Download and unzip the archive.

`wget https://github.com/christgau/wsdd/archive/master.zip`

`unzip master.zip`

Rename wsdd.py to wsdd.

`sudo mv wsdd-master/src/wsdd.py wsdd-master/src/wsdd`

Copy to /usr/bin.

`sudo cp wsdd-master/src/wsdd /usr/bin`

Copy wsdd to /etc/systemd/system.

`sudo cp wsdd-master/etc/systemd/wsdd.service /etc/systemd/system`

Open wsdd.service in nano and comment out User=nobody and Group=nobody with a ; semicolon.

`sudo nano /etc/systemd/system/wsdd.service`

If you found out below code different from yours just copy and paste it under [Service] to remove original code.

```
/etc/systemd/system/wsdd.service
[Unit]
Description=Web Services Dynamic Discovery host daemon
; Start after the network has been configured
After=network-online.target
Wants=network-online.target
; It makes sense to have Samba running when wsdd starts, but is not required
;Wants=smb.service

[Service]
Type=simple
ExecStart=/usr/bin/wsdd --shortlog
; Replace those with an unprivledged user/group that matches your environment,
; like nobody/nogroup or daemon:daemon or a dedicated user for wsdd
; User=nobody 
; Group=nobody
; The following lines can be used for a chroot execution of wsdd.
; Also append '--chroot /run/wsdd/chroot' to ExecStart to enable chrooting
;AmbientCapabilities=CAP_SYS_CHROOT
;ExecStartPre=/usr/bin/install -d -o nobody -g nobody -m 0700 /run/wsdd/chroot
;ExecStopPost=rmdir /run/wsdd/chroot

[Install]
WantedBy=multi-user.target
```

Save and exit (press CTRL + X, press Y and then press ENTER)
Reload daemon.

`sudo systemctl daemon-reload`

Start and enable wsdd.

`sudo systemctl start wsdd`

`sudo systemctl enable wsdd`

Output:

Created symlink /etc/systemd/system/multi-user.target.wants/wsdd.service → /etc/systemd/system/wsdd.service.

Now check that the service is running.

`sudo service wsdd status`

Output:

● wsdd.service - Web Services Dynamic Discovery host daemon
     Loaded: loaded (/etc/systemd/system/wsdd.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2020-06-10 10:51:39 CEST; 8s ago
   Main PID: 40670 (python3)
      Tasks: 1 (limit: 6662)
     Memory: 10.8M
     CGroup: /system.slice/wsdd.service
             └─40670 python3 /usr/bin/wsdd --shortlog

jun 10 10:51:39 ubuntu systemd[1]: Started Web Services Dynamic Discovery host daemon.
jun 10 10:51:40 ubuntu wsdd[40670]: WARNING: no interface given, using all interfaces

You should now be able to browse your Ubuntu machines and Samba shares in the Windows 10 file explorer. You may need to restart the Windows 10 machines to force discovery.

You may also want to reboot the Ubuntu server just to make sure the wsdd service starts up automatically without issue.

### Disable zram **Optional**

`sudo vim /etc/default/armbian-zram-config`
A few lines down the file, uncomment the line that says SWAP=false:

Zram swap enabled by default, unless set to disabled

SWAP=false
Reboot, and the zram swap is gone.

---
### Credits
https://devanswers.co/discover-ubuntu-machines-samba-shares-windows-10-network/

