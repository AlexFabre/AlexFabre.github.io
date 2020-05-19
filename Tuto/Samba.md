# Raspberry | Samba
19 May 2020

"_I have a SSD attached to my Raspberry Pi and I want to make it accessible to everyone in __read only__ and myself in __read and write__._"

## Prerequisite

* Make sure your drive is mounted automaticaly at boot. (create mountpoint folder and edit fstab). In this example mine is under ```/mnt/Drive```.
* The user I will give RW access is the default __pi__ user. Do it with any user you want.

## Installation

```sh
$ sudo apt install samba samba-common-bin
```

## Configuration

### First give ownership of the shared folder to the desired RW user.
```sh
$ sudo chown -R pi:pi /mnt/Drive
```
### Then edit the samba configuration file.

```sh
$ sudo nano /etc/samba/smb.conf
```

Comment the Home section if you don't need your RW user to access its home repo. Add ```;``` in front on lines.

```sh
;[homes]
;   comment = Home Directories
;   browseable = no

# By default, the home directories are exported read-only. Change the
# next parameter to 'no' if you want to be able to write to them.
;   read only = yes

# File creation mask is set to 0700 for security reasons. If you want to
# create files with group=rw permissions, set next parameter to 0775.
;   create mask = 0700

# Directory creation mask is set to 0700 for security reasons. If you want to
# create dirs. with group=rw permissions, set next parameter to 0775.
;   directory mask = 0700

# By default, \\server\username shares can be connected to by anyone
# with access to the samba server.
# The following parameter makes sure that only "username" can connect
# to \\server\username
# This might need tweaking when using external authentication schemes
;   valid users = %S
```

Also comment the printer section if you don't plan to make any printer accessible through your Rapsberry.

```sh
;[printers]
;   comment = All Printers
;   browseable = no
;   path = /var/spool/samba
;   printable = yes
;   guest ok = no
;   read only = yes
;   create mask = 0700

# Windows clients look for this share name as a source of downloadable
# printer drivers
;[print$]
;   comment = Printer Drivers
;   path = /var/lib/samba/printers
;   browseable = yes
;   read only = yes
;   guest ok = no
# Uncomment to allow remote administration of Windows print drivers.
# You may need to replace 'lpadmin' with the name of the group your
# admin users are members of.
# Please note that you also need to set appropriate Unix permissions
# to the drivers directory for these users to have write rights in it
;   write list = root, @lpadmin
```
### Configure your shared folder properties:
```sh
[Drive]
   comment = SSD 500Go
   path = /mnt/Drive
   read only = yes
   write list = pi
   guest ok = yes
   hide files = /lost+found/.Trash*/*.bak/
```
_Notice the ```write list = pi``` line that give write access only to __pi__ user._

### Add your user to the samba user list.
```sh
$ sudo smbpasswd -a pi
```

### And finaly restart the samba service

```sh
$ sudo systemctl restart smbd
```
