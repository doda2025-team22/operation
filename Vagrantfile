# -*- mode: ruby -*-
# vi: set ft=ruby :



Nodes = 2
Vagrant.configure("2") do |config|
  
  # Create a Virtual Machine using bento ubuntu 
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.box_version = "202510.26.0" #Vagrant box version defined in assignment. For reproducability 

  config.vm.provision :ansible do |a| 
    a.compatibility_mode = "2.0"
    a.playbook = "kubernetes-setup/general.yaml"
    a.extra_vars = { nodes: Nodes} 
  end


  config.vm.define "ctrl" do |ctrl|  
    ctrl.vm.network "private_network", ip: "192.168.56.10"
    ctrl.vm.provision :ansible do |a| 
      a.compatibility_mode = "2.0" 
      a.playbook = "kubernetes-setup/ctrl.yaml" 
    end
    ctrl.vm.provider "virtualbox" do |v| 
      v.name = "ctrl" 
      v.memory = 4096 # 4GB of memory 
      v.cpus = 1 # 1 Core 
    end
  end

  

  (1..Nodes).each do |n|
    config.vm.define "node-#{n}" do |node| 
      node.vm.network "private_network", ip: "192.168.56.10#{+n}" 
      node.vm.provision :ansible do |a|
        a.compatibility_mode = "2.0"
        a.playbook = "kubernetes-setup/node.yaml"
      end
      node.vm.provider "virtualbox" do |v| 
        v.name = "node-#{n}" 
        v.memory = 6144 #6GB of memory (in MB) 
        v.cpus = 2
        
      end
    end
  end
end
