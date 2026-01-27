# README.md 

Below we outline the main components of the application, as well as sections related to different assignments. 

Apart from the general setup instructions on this document, we link additional docs outlining how our features map to the assignments for easier reviewing, some also contain self-filled rubrics for easier reviewing and also self-assessment.
- [Assignment 3](docs/A3-REVIEW.md)
- [Assignment 4](docs/A4-Review.md)
- [Experimentation](docs/continuous-experimentation.md)
- [Deployment](docs/deployment.md)
- [Extension](docs/extension.md)
- [Helm-specific instructions](team22_chart/README-Helm.md)

## Architecture

The system consists of four repositories:

- app https://github.com/doda2025-team22/app: The app contains the frontend of the application with REST API.
- model-service https://github.com/doda2025-team22/model-service: Backend for machine learning model, exposing port 8081 used by the frontend.
- operation https://github.com/doda2025-team22/operation: For deployment, orchestration and documentation.
- lib-version https://github.com/doda2025-team22/lib-version: Defines versioning for repositories.


## Steps to run the application with Docker Compose for earlier assignments:

First, you need docker and docker compose installed.

Start docker engine (open Docker Desktop for Windows for example)

Than you run the following commands:

``` bash
git clone git@github.com:doda2025-team22/operation.git

cd operation

docker compose --env-file <.env or .env_test> up 
```

Open http://localhost:8080/sms

Troubleshooting Tips:
- In case of errors pulling from the registry, please make sure your PAT is configured and you have logged into ghcr.io.

## Setting up the Vagrant Cluster

### Steps to run the cluster and the requirements for A2

1. Run `vagrant up` to create the kubernetes cluster.
2. Run `export KUBECONFIG=$PWD/admin.conf` on the root of the operation repository to set the KUBECONFIG environment variable.
3. Run `ssh-add ~/.ssh/id_rsa` to add your SSH key to the agent.
4. Run `ansible-playbook -u vagrant -i 192.168.56.100, ./ansible/finalization.yaml` to deploy the kubernetes dashboard and ingress-nginx.
5. Run `kubectl get nodes` to see the nodes in the cluster.

> Note: If there is an issue related to the kuberneties dependency for ansible, run `ansible-galaxy collection install -r ansible/collections/requirements.yml -p ansible/collections --force` command. This will install the enssesary ansible dependencies.

### General Setup Explanation

Vagrantfile automaticly creates all the nessesary VM's and runs the ansible playbooks on them. The playbooks are located in the `ansible` directory. The playbooks are:
- general.yaml: General setup for all nodes
- ctrl.yaml: Setup for the controller node
- node.yaml: Setup for the worker nodes
- finalization.yaml: Finalization of the cluster setup
- site.yaml: Site file that imports the other playbooks

The `ansible` directory also contains the ssh public keys of the team members. These keys are copied to the `~/.ssh/authorized_keys` file of the VM's. The vagrints default key generation is disabled so make sure to add your own public key to the `ansible/keys` directory to ensure you can ssh into the VM's.

Inside the `ansible` directory you can the collections for the ansible playbooks. These collections are used to install the kubernetes tools and deploy the kubernetes dashboard and ingress-nginx. We have included the collections in the `ansible/collections` directory so you do not need to install them manually.

The ansible playbook also copies the nessesary admin.conf file to the root of the operation repository so you can use kubectl to manage the cluster.

When running `vagrant up` the playbooks that are defined inside the `site.yaml` file are run. This file imports the other playbooks and runs them in the correct order. When `vagrant up` is run, the inventory file is generated automaticly and saved inside the `ansible` directory. After this step you can run the `finalization.yaml` playbook manually to deploy the kubernetes dashboard and ingress-nginx using:

```bash
ansible-playbook -u vagrant -i 192.168.56.100, finalization.yml
```

inside the `ansible` directory.


### Accessing Dashboard

The playbook deploys the k8n dashboard to URL https://dashboard.192-168-56-90.sslip.io by default. This method allows you to access the dashboard without modifying your hosts file. Additionally as we use self-signed certificates, your browser will warn you about the certificate. You can add the certificate to your trusted certificates by clicking on the "Advanced" button and then "Add exception".


## Installation through the Helm Chart
Please refer to the specific [README](team22_chart/README-Helm.md) for our Helm chart instructions.
