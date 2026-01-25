WORKER_COUNT = 2
NODES = (1..WORKER_COUNT).map { |i| "node-#{i}" }

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.box = "bento/ubuntu-24.04"

    config.vm.synced_folder "./data", "/mnt/shared", 
        create: true, 
        owner: "vagrant", 
        group: "vagrant", 
        mount_options: ["dmode=775,fmode=664"]
    
    config.vm.define "ctrl" do |ctrl|
        ctrl.vm.hostname = "ctrl"
        ctrl.vm.network "private_network", ip: "192.168.56.100", netmask: "255.255.255.0", adapter: 2
        ctrl.vm.provider "virtualbox" do |vb|
            vb.name = "ctrl"
            vb.linked_clone = true
            vb.memory = 4096 # 6144
            vb.cpus = 2
        end
    end

    (1..WORKER_COUNT).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.hostname = "node-#{i}"
            node.vm.network "private_network", ip: "192.168.56.#{100 + i}", netmask: "255.255.255.0", adapter: 2
            node.vm.provider "virtualbox" do |vb|
                vb.name = "node-#{i}"
                vb.linked_clone = true
                vb.memory = 6114
                vb.cpus = 2
            end
            if i == WORKER_COUNT
                node.vm.provision "ansible" do |ansible|
                    ansible.playbook = "ansible/site.yaml"
                    ansible.config_file = "ansible/ansible.cfg"
                    ansible.limit = "all"
                    ansible.compatibility_mode = "2.0"
                    ansible.extra_vars = { worker_count: WORKER_COUNT}
                    ansible.groups = {
                        "controller"        => ["ctrl"],
                        "nodes"             => NODES,
                        "cluster:children"  => ["controller", "nodes"]
                    }
                end
                
                # Generate inventory file after vagrant up completes
                node.trigger.after :up do |trigger|
                    trigger.info = "Generating Ansible inventory file..."
                    # Build inventory content
                    inventory_content = "[controller]\n"
                    inventory_content += "ctrl ansible_host=192.168.56.100 ansible_user=vagrant\n\n"
                    inventory_content += "[nodes]\n"
                    inventory_content += NODES.map.with_index { |node_name, idx| "#{node_name} ansible_host=192.168.56.#{101 + idx} ansible_user=vagrant" }.join("\n")
                    inventory_content += "\n\n[cluster:children]\ncontroller\nnodes\n"
                    # Get the project root directory (where Vagrantfile is located)
                    project_root = File.expand_path(File.dirname(__FILE__))
                    # Write file using Ruby
                    script = "File.write('#{File.join(project_root, 'ansible', 'inventory.ini')}', #{inventory_content.inspect})"
                    trigger.run = {inline: "ruby -e #{script.inspect}"}
                end
            end
        end
    end
end
