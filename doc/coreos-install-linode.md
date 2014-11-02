http://nickrandell.wordpress.com/2014/09/29/installing-coreos-on-linode/

# Installing CoreOS on Linode

## Create Linode
1. 1GB disk for Debian, 512MB swap
2. Rest of disk space for CoreOS (unformatted/raw)
3. Update disk configuration:
  * Move swap to /dev/xvdc
  * Set CoreOS to /dev/xvdb
4. Boot

## Install pv-grub and update boot settings
1. Install pv-grub 
```
apt-get update
apt-get install grub-legacy
```
2. Set kernel to pv-grub-x86_64
3. Set xenify distro to disabled
