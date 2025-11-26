# README.md

Quick guide on how to use currently implemented steps.

## Initialization

Make sure you have ssh keys set up on your device. The playbook will copy all the public keys present on your ~/.ssh directory to the autorised keys of all the VM's

Also make sure to install the nessesary plugins for Ansible
``` bash
ansible-galaxy collection install kubernetes.core
```

## Steps 1-3

These steps are present in the Vagrantfile; where the default box is specified, the nodes are created, their memory/CPU allocations and the worker node count are specified, and the private network addresses per node are generated dynamically, and an ansible provisioner is specified for each node with respect to the three ansible notebooks "general.yaml," "ctrl.yaml," and "node.yaml;" with tasks for both types of nodes, tasks for the ctrl node, and tasks for the worker nodes implemented respectively.

## Steps 4-12

These steps are present in the "general.yaml" ansible playbook. Specific documentation for each step regarding what they accomplish and which modules they use to do so can also be found in the "general.yaml" playbook.
