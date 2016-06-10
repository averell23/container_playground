# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  config.vm.define "overlord" do |box|
    box.vm.box = "bento/centos-7.2"
    box.vm.hostname = "overlord"
    box.vm.network :private_network, ip: '192.168.23.10'
    box.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "512"]
      vb.customize ["modifyvm", :id, "--cpus", "1"]
    end
  end

  config.vm.define "minion1" do |box|
    box.vm.box = "bento/centos-7.2"
    box.vm.hostname = "minion1"
    box.vm.network :private_network, ip: '192.168.23.11'
    box.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
  end

  config.vm.define "gluster" do |box|
    box.vm.box = "bento/centos-7.2"
    box.vm.hostname = "gluster"
    box.vm.network :private_network, ip: '192.168.23.12'
    box.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "512"]
      vb.customize ["modifyvm", :id, "--cpus", "1"]
      file_to_disk = './tmp/large_disk.vdi'
      vb.customize ['createhd', '--filename', file_to_disk, '--size', 5 * 1024 * 10]
      vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
    end
    box.vm.provision :shell, path: 'mount_disk.sh', run: 'once'
  end

  config.vm.provision 'file',
    run: 'once',
    source: '~/.ssh/id_rsa.pub',
    destination: '/home/vagrant/host_pubkey'


  config.vm.provision :shell, :inline => "ulimit -n 4048"

  require_relative 'lib/vagrant/bootstrap_user'
  Vagrant::BoostrapUser.new(config.vm).perform
end
