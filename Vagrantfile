# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  extradisk = "./disks/extradisk.vdi"

  if ! Vagrant.has_plugin?("vagrant-reload")
    abort("Please install the vagrant-reload plugin, vagrant plugin install vagrant-reload")
  end

  config.vm.box = "centos/atomic-host"

  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.hostname = "centos-atomic"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = "2"
    unless File.exist?(extradisk)
      vb.customize ['createhd', '--filename', extradisk, '--size', 500 * 1024]
    end
    vb.customize ['storageattach', :id, '--storagectl', 'IDE', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', extradisk]
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
