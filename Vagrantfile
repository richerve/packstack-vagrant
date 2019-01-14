# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.hostname = "packstack"

  config.vm.box = "centos/7"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 4
    vb.memory = 6000
  end

  # dashboard
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true

  # Map OpenStack ports to same number prefixed with 5
  [
    8774, # nova
    8776, # cinder
    8778, # placement
    9292, # image
    9696, # neutron
    5000 # keystone
  ].each do |p|
    config.vm.network "forwarded_port", guest: p, host: p
  end

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    ssh-keygen -t rsa -N '' -f ~/.ssh/id_rsa -q
    sudo bash -c "ssh-keygen -t rsa -N '' -f ~/.ssh/id_rsa -q"
    sudo bash -c "umask 077 && cat ~vagrant/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys"

    sudo yum -y install centos-release-openstack-rocky
    sudo yum -y install openstack-packstack
    sudo yum -y install e2fsprogs

    sudo systemctl disable NetworkManager
    sudo systemctl stop NetworkManager

    packstack --answer-file=/vagrant/answers.txt
  SHELL
end
