# README.md

Quick guide on how to use currently implemented steps.

## Initialization

First of all, you can create a new public key (also can do without a passphrase for convenience sake) with the command(s):

-ssh-keygen -t ed25519 -f ~/.ssh/(NAME OF KEY) -N "" (no passphrase)

-ssh-keygen -t ed25519 -f ~/.ssh/(NAME OF KEY) (with passphrase)

You can also use already existing keys but starting out with a fresh key was how the steps were implemented and tested.

After the keys are created, go to the "~/.ssh/" directory and copy the newly generated public key, which has the .pub extension, into the ssh_keys directory. Do NOT copy the key without the .pub extension; which is the private key and should remain that way.

## Steps 1-3

These steps are present in the Vagrantfile; where the default box is specified, the nodes are created, their memory/CPU allocations and the worker node count are specified, and the private network addresses per node are generated dynamically, and an ansible provisioner is specified for each node with respect to the three ansible notebooks "general.yaml," "ctrl.yaml," and "node.yaml;" with tasks for both types of nodes, tasks for the ctrl node, and tasks for the worker nodes implemented respectively.

## Steps 4-12

These steps are present in the "general.yaml" ansible playbook. Specific documentation for each step regarding what they accomplish and which modules they use to do so can also be found in the "general.yaml" playbook.
