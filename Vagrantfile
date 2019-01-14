# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.hostname = "packstack"

  config.vm.box = "centos/7"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 4
    vb.memory = "8192"
  end

  ## TODO: add rules to allow access to vagrant ports from your local machine

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
