# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  if ! Vagrant.has_plugin?("vagrant-reload")
    abort("Please install the vagrant-reload plugin, vagrant plugin install vagrant-reload")
  end

  config.vm.box = "centos/atomic-host"

  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.hostname = "centos-atomic"

  config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = "2"
    unless File.exist?('./disks/disk.vdi')
      vb.customize ['createhd', '--filename', './disks/disk.vdi', '--size', 10 * 1024]
    end
    vb.customize ['storageattach', :id, '--storagectl', 'IDE', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', './disks/disk.vdi']
  end

  config.vm.provision "shell", inline: <<-SHELL
    # Install ZFS kABI tracking modules in overlay
    if ! rpm -q zfs ; then
      curl -Ss -o /etc/yum.repos.d/zfs.repo https://raw.githubusercontent.com/lvlie/vagrant-centos-atomic-zfs/master/zfs.repo
      curl -Ss -o /etc/pki/rpm-gpg/RPM-GPG-KEY-zfsonlinux https://raw.githubusercontent.com/lvlie/vagrant-centos-atomic-zfs/master/RPM-GPG-KEY-zfsonlinux
      atomic host install zfs
    fi

    # Disable docker.service because our docker containers are dependent on zpool
    if systemctl is-enabled docker.service ; then
      systemctl disable docker.service
    fi
  SHELL

  config.vm.provision :reload

  config.vm.provision "shell", inline: <<-SHELL
    # Create zpool, -m /srv is important because default mountpoint cannot be created if it doesnt exist (ro mounts)
    find /lib/modules/*/extra/ -name "*.ko" | xargs -n1 insmod
    zpool create tank /dev/sdb -m /srv
    systemctl start docker.service

    # Add import command to rc.local #TODO: systemd unit files with dependencies to docker.service
    if ! grep "zpool import" /etc/rc.d/rc.local ; then
      echo 'find /lib/modules/*/extra/ -name "*.ko" | xargs -n1 insmod' >> /etc/rc.d/rc.local
      echo 'zpool import -a && systemctl start docker.service' >> /etc/rc.d/rc.local
      chmod +x /etc/rc.d/rc.local
    fi
  SHELL
end
