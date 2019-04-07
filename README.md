# Unison install and autostart Ubuntu 18.04 linux

Simple and straightforward guide for Unison install and set it up to watch changes in folders and start at the boot

## Install Unison

`sudo apt-get install -y ocaml opam`

`opam init`

`git clone https://github.com/bcpierce00/unison.git`

`cd unison`

`git checkout 2.48.4`

`make NATIVE=true UISTYLE=text`

`sudo install src/unison /usr/local/bin`

`sudo install src/fsmonitor.py /usr/local/bin`

`sudo pip install pyinotify`

## Sync two folders with Unison

### Create profile

`sudo nano /home/[YOUR USER FOLDER NAME]/.unison/profile.prf`

Create content:

```--------------

#Roots of the synchronization
root = /home/folder1
root = /home/folder2

#Paths to synchronize

#Files
path = file1.json
path = file2.json
path = file3.json

#Set files or folders to ignore
ignore = Name filename.txt

#Dirs
path = foldername

#Logging
log = true
logfile = /home/logs/unison-sync-log.txt

# Enable change monitoring for files and folder
auto = true

--------------
```

## Create service to start Unison at the boot

`sudo nano /lib/systemd/system/unison.service`

### Paste content:

```
--------------

[Unit]
Description=unison
After=network.target

[Service]
Type=simple
User=[YOUR USER NAME]
ExecStart=/usr/local/bin/unison profile
StandardOutput=journal
StandardError=journal
Restart=always
RestartSec=0

[Install]
WantedBy=multi-user.target

--------------
```

### Enable

`sudo systemctl enable unison.service`

## Control the service

### Reload

`sudo systemctl daemon-reload`

### Start

`sudo systemctl start unison.service`

### Stop

`sudo systemctl stop unison.service`

### Display the status

`systemctl status unison.service`

- More detailed status with live logging view:

`journalctl -u unison.service -f`


### Restart manually

`sudo systemctl restart unison.service`


### Disable from starting after reboot

`sudo systemctl disable unison.service`
