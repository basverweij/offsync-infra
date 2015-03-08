* https://github.com/million12/linodeapi/blob/master/prepare_coreos_boot.sh
* http://nickrandell.wordpress.com/2014/09/29/installing-coreos-on-linode/

# Installing CoreOS on Linode

## Create Linode
1. 1024 MB disk for Debian, 512MB swap
3. 16384 MB disk for CoreOS (unformatted/raw)
4. Rest (31232 MB) for Srv
5. Update disk configuration:
  * CoreOS on /dev/xvdb
  * swap on /dev/xvdc
  * Srv on /dev/xvdd
6. Boot



## Prepare debian
```
#!/bin/bash

export DEBCONF_FRONTEND=noninteractive

# Prepare Debian for boot with CoreOS
apt-get update -y
apt-get upgrade --show-upgraded -y
apt-get install -y linux-image-amd64 git mc
mkdir /boot/grub
apt-get install -y grub-legacy
grub-set-default 1
update-grub
```
2. Set kernel to pv-grub-x86_64
(3. Set 'Xenify Distro' to 'No')

## Install CoreOS
1. Download installer and Off-Sync userdata
```
wget https://raw.githubusercontent.com/coreos/init/master/bin/coreos-install
chmod +x coreos-install
```
2. Update user-data with the right settings
3. Run the installer
```
./coreos-install -C stable -d /dev/xvdb -c user-data
```
4. Move grub
```
mkdir /mnt/core-boot
rm -rf /boot/grub
mount /dev/xvdb1 /mnt/core-boot
mv /mnt/core-boot/boot/grub /boot

sed -i 's/hd0/hd1/g' /boot/grub/menu.lst
```
5. Reboot via Linode Manager
