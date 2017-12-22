# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/atomic-host"

  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.hostname = "centos-atomic"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = "2"
  end

  config.vm.provision "shell", inline: <<-SHELL
    curl -o /etc/yum.repos.d/zfs.repo https://raw.githubusercontent.com/lvlie/vagrant-centos-atomic-zfs/master/zfs.repo
    curl -o /etc/pki/rpm-gpg/RPM-GPG-KEY-zfsonlinux https://raw.githubusercontent.com/lvlie/vagrant-centos-atomic-zfs/master/RPM-GPG-KEY-zfsonlinux
  SHELL
end
