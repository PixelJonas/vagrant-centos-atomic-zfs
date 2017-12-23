# vagrant-centos-atomic-zfs
Vagrantfile for Atomic deploy of CentOS 7.4 with ZFS

This repository holds the source for a Vagrantbox with CentOS-atomic and a working ZFS install.

Just run:
# vagrant up && vagrant ssh

And you can:
# zpool create tank /dev/sdb -m /srv    # <-- the -m is important, because /srv is an existing directory and creating /tank is not allowed

Disclaimer: importing the zfs kmod's is hacky, because DKMS doesn't work yet in Atomic and the kABI-tracking mods from ZoL dont work well with weak-modules in Atomic...
Your mileage may vary
