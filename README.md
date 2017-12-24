# vagrant-centos-atomic-zfs
Vagrantfile for Atomic deploy of CentOS 7.4 with ZFS

This repository holds the source for a Vagrantbox with CentOS-atomic and a working ZFS install with a zpool called tank on /dev/sdb and mounted on /srv (this is important as creating the default /tank for the tank zpool is not allowed). 

Just run:<br>
```# vagrant up && vagrant ssh```
<br>

<b>Disclaimer</b>: importing the zfs kmod's is hacky, because DKMS doesn't work yet in Atomic and the kABI-tracking mods from ZoL dont work well with weak-modules in Atomic...
<br>Your mileage may vary
