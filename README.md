# README.md 

The organization repository contains the docker-compose to start the application.

## Architecture

The system consists of four repositories:

- app https://github.com/doda2025-team22/app: The app contains the frontend of the application with REST API.
- model-service https://github.com/doda2025-team22/model-service: Backend for machine learning model, exposing port 8081 used by the frontend.
- operation https://github.com/doda2025-team22/operation: For deployment, orchestration and documentation.
- lib-version https://github.com/doda2025-team22/lib-version: Defines versioning for repositories.


## Steps to run the application:

First, you need docker and docker compose installed.

Start docker engine (open Docker Desktop for Windows for example)

Than you run the following commands:

``` bash
git clone git@github.com:doda2025-team22/operation.git

cd operation

docker compose up
```

Open http://localhost:8080/sms

Troubleshooting Tips:
- In case of errors pulling from the registry, please make sure your PAT is configured and you have logged into ghcr.io.

## Steps to run the kubernetes cluster and the requirements for A2
### Initialization

Make sure you have ssh keys set up on your device. The playbook will copy all the public keys present on your `~/.ssh` directory to the autorised keys of all the VM's

Also make sure to install the nessesary plugins for Ansible
``` bash
ansible-galaxy collection install kubernetes.core
```

### Steps 1-3

These steps are present in the Vagrantfile; where the default box is specified, the nodes are created, their memory/CPU allocations and the worker node count are specified, and the private network addresses per node are generated dynamically, and an ansible provisioner is specified for each node with respect to the three ansible notebooks `general.yaml`, `ctrl.yaml` and `node.yaml` with tasks for both types of nodes, tasks for the ctrl node, and tasks for the worker nodes implemented respectively.

### Steps 4-12

These steps are present in the `general.yaml` ansible playbook. Specific documentation for each step regarding what they accomplish and which modules they use to do so can also be found in the "general.yaml" playbook.

### Running finalization.yaml Manually

The inventory file is generated automaticly and after running the vagrantfile at least one the inventory file will be saved inside the `.vagrant` folder. To run the `finalization.yaml` playbook manually do:

```bash
ansible-playbook -u vagrant -i 192.168.56.100, finalization.yml
```

For the finalisation playbook the ipadress of the controller is assumbed to be `192.168.56.100`. If you need to change this to get the VM's running make sure to update this on the file too or manually running this notebook might not work.

### Accessing Dashboard

The playbook deploys the k8n dashboard to URL https://dashboard.192-168-56-90.sslip.io
