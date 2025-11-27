Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"

  WORKER_COUNT = 2
  NET_PREFIX   = "192.168.57."

  CTRL_MEMORY  = 4096
  CTRL_CPUS    = 1

  NODE_MEMORY  = 6144
  NODE_CPUS    = 2

  # Controller node

  config.vm.define "ctrl" do |ctrl|
    ctrl.vm.hostname = "ctrl"
    ctrl.vm.network "private_network", ip: "#{NET_PREFIX}100"

    ctrl.vm.provider "virtualbox" do |vb|
      vb.memory = CTRL_MEMORY
      vb.cpus   = CTRL_CPUS
      vb.gui    = false
    end

    #

    ctrl.vm.provision "ansible" do |ans|
      ans.playbook = "ansible/general.yml"
      ans.extra_vars = { role: "controller" }
    end

    ctrl.vm.provision "ansible" do |ans|
      ans.playbook = "ansible/ctrl.yml"
      ans.inventory_path = "ansible/inventory.ini"
    end
  end

  # Worker nodes

  (1..WORKER_COUNT).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.hostname = "node-#{i}"
      node.vm.network "private_network", ip: "#{NET_PREFIX}#{100 + i}"

      node.vm.provider "virtualbox" do |vb|
        vb.memory = NODE_MEMORY
        vb.cpus   = NODE_CPUS
        vb.gui    = false
      end

      #
      node.vm.provision "ansible" do |ans|
        ans.playbook = "ansible/general.yml"
        ans.extra_vars = { role: "worker" }
      end

      node.vm.provision "ansible" do |ans|
        ans.playbook = "ansible/node.yml"
        ans.inventory_path = "ansible/inventory.ini"
      end
    end
  end
end