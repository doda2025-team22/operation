WORKER_COUNT = 2

Vagrant.configure("2") do |config|
    config.vm.box = "bento/ubuntu-24.04"

    config.vm.define "ctrl" do |ctrl|
        ctrl.vm.hostname = "ctrl"
        ctrl.vm.network "private_network", ip: "192.168.56.100"
        ctrl.vm.provider "virtualbox" do |vb|
            vb.memory = 4096
            vb.cpus = 1
        end
        ctrl.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/general.yaml"
            ansible.extra_vars = { role: "controller", worker_count: WORKER_COUNT}
        end
        ctrl.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/ctrl.yaml"
        end
    end

    (1..WORKER_COUNT).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.hostname = "node-#{i}"
            node.vm.network "private_network", ip: "192.168.56.#{100 + i}"
            node.vm.provider "virtualbox" do |vb|
                vb.memory = 6114
                vb.cpus = 2
            end
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "ansible/general.yaml"
                ansible.extra_vars = { role: "controller", worker_count: WORKER_COUNT}
            end
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "ansible/node.yaml"
            end
        end
    end
end