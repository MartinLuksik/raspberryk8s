## Kernel version of a Linux system
```bash
uname -a
```
## Current ip address
```bash
ifconfig (old school)
ip addr show
ip addr show eth0
```

## Disk usage:
```bash
# disk free
df -ah
```

## Services, start/stop
```bash
service <service name> status (old school)
systemctl status <service name>
```

## Rotal size of a directory
```bash
du -sh <dir> (disk usage)
```

## Open ports, listening, network socket..., ucp/udp services
```bash
netstat
netstat -tulpn (tcp,ucp,...) #good to run as sudo to see program names
```

## CPU process useage
```bash
ps aux | grep nginx # read up on ps
top # more info
htop # not by default but nice graphics
```

## Mount drive e.g. usb stick
```bash
mount /dev/sda2 /mnt
# check for existing mounts
mount
# automatically mount volume at boot:
less /etc/fstab
```
## Manual pages for commands
```bash
man ps
```

## VIM
```bash
# start edit
vim file.tf
# quit
:q
# write
:w
# write and quite
:wq
# quit without changes
:q! 
# jump to a line
:set number (see line numbers)
# start writing
:i
# delete a line
dd
# delete 10 lines
10dd
# undo
:u
# redo
ctrl r
# looking for string 'provisioner'
/provisioner
# replace all 'privisioner' strings with 'foo' plus confirm with enters
:%s/provisioner/foo/g
```

## Change host name
```bash
sudo vi /etc/hostname
sudo reboot
```

## SSH pub key copy
```bash
# print and pipe the result to the server
cat id_rsa.pub | ssh ubuntu@192.168.0.153 'cat >> /home/ubuntu/.ssh/authorized_keys'
# download a file from server
scp your_username@remotehost.edu:foobar.txt /local/dir
```