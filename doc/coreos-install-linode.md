http://nickrandell.wordpress.com/2014/09/29/installing-coreos-on-linode/

# Installing CoreOS on Linode

## Create Linode
1. 1024 MB disk for Debian, 512MB swap
3. 6656 MB disk for CoreOS (unformatted/raw)
4. Rest  (16384 MB) for Srv
5. Update disk configuration:
  * Move swap to /dev/xvdc
  * Set CoreOS to /dev/xvdb
  * Set Srv to /dev/xvdd
6. Boot

## Install pv-grub and update boot settings
1. Install pv-grub 
```
apt-get update
apt-get install grub-legacy
```
2. Set kernel to pv-grub-x86_64
3. Set 'Xenify Distro' to 'No'

## Install CoreOS
1. Download installer and Off-Sync userdata
```
cd /root
wget https://raw.githubusercontent.com/coreos/init/master/bin/coreos-install
wget https://raw.githubusercontent.com/basverweij/offsync-infra/master/cloud-config/user-data
```
2. Update user-data with the right settings
3. Run the installer
```
bash ./coreos-install -C stable -d /dev/xvdb -c user-data
```
4. Move grub
```
mkdir /mnt/core-boot
rm -rf /boot/grub
mount /dev/xvdb1 /mnt/core-boot
mv /mnt/core-boot/boot/grub /boot
sed -i 's/(hd0,0)/(hd1,0)/g' /boot/grub/menu.lst
```
5. Reboot via Linode Manager
