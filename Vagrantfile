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
    box.vm.network :private_network, ip: '192.168.23.10'
    box.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
  end

  config.vm.define "minion1" do |box|
    box.vm.box = "bento/centos-7.2"
    box.vm.network :private_network, ip: '192.168.23.11'
    box.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
  end

  config.vm.provision 'file',
    run: 'once',
    source: '~/.ssh/id_rsa.pub',
    destination: '/home/vagrant/host_pubkey'


  config.vm.provision :shell, :inline => "ulimit -n 4048"

  require_relative 'lib/vagrant/bootstrap_user'
  Vagrant::BoostrapUser.new(config.vm).perform
end
