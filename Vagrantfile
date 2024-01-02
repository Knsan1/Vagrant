# -*- mode: ruby -*-
# vi: set ft=ruby :
NUM_WORKER_NODES    = 3
IP_NW="192.168.56."
IP_START=10
CPUS_MASTER_NODE    = 2
MEMORY_MASTER_NODE  = 2048
CPUS_WORKER_NODE    = 1
MEMORY_WORKER_NODE  = 1024
VAGRANT_BOX_IMAGE   = "bento/ubuntu-22.04"

Vagrant.configure("2") do |config|

  config.vm.provision "shell", env: {"IP_NW" => IP_NW, "IP_START" => IP_START}, inline: <<-SHELL
      apt-get update -y
      echo "$IP_NW$((IP_START))  master-node  master-node.hellocloud.io" >> /etc/hosts
      echo "$IP_NW$((IP_START+1))  worker-node01  worker-node01.hellocloud.io" >> /etc/hosts
      echo "$IP_NW$((IP_START+2))  worker-node02  worker-node02.hellocloud.io" >> /etc/hosts
      echo "$IP_NW$((IP_START+3))  worker-node03  worker-node03.hellocloud.io" >> /etc/hosts
  SHELL

  config.vm.define "master" do |master|
    config.ssh.private_key_path = "/home/kms/just4nodeauto/.ssh/id_rsa"
    config.ssh.forward_agent = true
    config.ssh.username = "vagrant"
    config.ssh.password = "vagrant"
    master.vm.box = VAGRANT_BOX_IMAGE
    master.vm.hostname = "master-node.hellocloud.io"
    master.vm.synced_folder "./master", "/home/vagrant/"
    master.vm.network "private_network", ip: IP_NW + "#{IP_START}"
    master.vm.provider "virtualbox" do |vb|
      vb.name = "master-node"
      vb.cpus = CPUS_MASTER_NODE
      vb.memory = MEMORY_MASTER_NODE
      vb.gui = false
    end
  end


  (1..NUM_WORKER_NODES).each do |i|

    config.vm.define "node0#{i}" do |node|
    config.ssh.private_key_path = "/home/kms/just4nodeauto/.ssh/id_rsa"
    config.ssh.forward_agent = true
    config.ssh.username = "vagrant"
    config.ssh.password = "vagrant"
      node.vm.hostname = "worker-node0#{i}.hellocloud.io"
      node.vm.box = VAGRANT_BOX_IMAGE
      node.vm.synced_folder "./node0#{i}", "/home/vagrant/"
      node.vm.network "private_network", ip: IP_NW + "#{IP_START + i}"
      node.vm.provider "virtualbox" do |vb|
        vb.name = "worker-node0#{i}"
        vb.cpus = CPUS_WORKER_NODE
        vb.memory = MEMORY_WORKER_NODE
        vb.gui = false
      end
    end
  end
end
