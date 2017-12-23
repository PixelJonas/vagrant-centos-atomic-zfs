# -*- mode: ruby -*-
# vi: set ft=ruby :
# This Vagrantfile requires the vagrant-reload plugin, please run:
# vagrant plugin install vagrant-reload

Vagrant.configure("2") do |config|
  config.vm.box = "centos/atomic-host"

  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.hostname = "centos-atomic"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = "2"
  end

  config.vm.provision "shell", inline: <<-SHELL
   if ! rpm -q zfs ; then
      curl -o /etc/yum.repos.d/zfs.repo https://raw.githubusercontent.com/lvlie/vagrant-centos-atomic-zfs/master/zfs.repo
      curl -o /etc/pki/rpm-gpg/RPM-GPG-KEY-zfsonlinux https://raw.githubusercontent.com/lvlie/vagrant-centos-atomic-zfs/master/RPM-GPG-KEY-zfsonlinux
      atomic host install zfs
    fi
  SHELL

  config.vm.provision :reload

  config.vm.provision "shell", inline: <<-SHELL
    find /lib/modules/*/extra/ -name "*.ko" | xargs -n1 insmod
  SHELL
end
