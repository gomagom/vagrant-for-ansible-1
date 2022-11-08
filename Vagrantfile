# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  #全ての仮想マシンで公開鍵を共通にする
  config.ssh.insert_key = false

  #box指定
  config.vm.box = "generic/ubuntu2204"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.define "control1" do |server|
    server.vm.hostname = "control1"
    server.vm.network "private_network", ip: "192.168.57.10"

    # VirtualBoxのGUI上の名前を設定
    server.vm.provider "virtualbox" do |vb|
      vb.name = config.vm.box.gsub(/.*\//, "") + "_" + server.vm.hostname
      vb.memory = 1024
      vb.cpus = 2
    end

    server.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "~/.ssh/target1_private_key"

    server.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "./playbooks/control1.yml"
    end
  end

  config.vm.define "target1" do |server|
    server.vm.hostname = "target1"
    server.vm.network "private_network", ip: "192.168.57.20"

    server.vm.provider "virtualbox" do |vb|
      vb.name = config.vm.box.gsub(/.*\//, "") + "_" + server.vm.hostname
      vb.memory = 1024
      vb.cpus = 2
    end
  end
end
