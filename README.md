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

## Vagrant Cluster

### Steps to run the cluster and the requirements for A2

1. Run `vagrant up` to create the kubernetes cluster.
2. Run `export KUBECONFIG=$PWD/admin.conf` on the root of the operation repository to set the KUBECONFIG environment variable.
3. Run `ssh-add ~/.ssh/id_rsa` to add your SSH key to the agent.
4. Run `ansible-playbook -u vagrant -i 192.168.56.100, ./ansible/finalization.yaml` to deploy the kubernetes dashboard and ingress-nginx.
5. Run `kubectl get nodes` to see the nodes in the cluster.

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


### To run istio using curl use the following commands 
Follow the steps above to setup minikube with istio and connect it to the application. 
start up a minikube tunnel
run the following command in a seperate terminal: 
```bash
curl -v \
  -H "Host: team22.local" \
  -c cookies.txt \
  http://192.168.49.2:<port number>/sms/library-version
```
This command saves your cookie to a .txt file which you can pass along with the next command: 
```bash
for i in {1..10}; do
  curl -s \
    -H "Host: team22.local" \
    -b cookies.txt \
    http://192.168.49.2:30911/sms/library-version | grep version
done
```
A stable version results is version 0.05, while a canary version results in 0.0.1

## Helm Chart
If you have followed the "Vagrant Cluster" instructions and set up our provisioned cluster, you can skip to the helm commands. If you want to use minikube instead, start the cluster with:
```bash
minikube start --driver=docker --cpus=4 --memory=8192
```
This is the default configuration for the cluster we used during development. You can adjust the number of CPUs and memory as needed.
Then install Istio with:
```bash
istioctl install
```
Make sure istio is installed on the host machine that is running the cluster.

On MacOS you need to enable the minikube tunnel in a different terminal with:

```bash
sudo -E minikube tunnel
```

Update the helm chart with:
```bash
helm dependency update ./team22_chart
```
Then install the helm chart with:
```bash
helm upgrade --install team22 ./team22_chart \
  --namespace team22 \
  --create-namespace
```
The values.yaml file is configured to use the local domain. You can change it to use a different domain by changing the global.domain value.
## Accessing the application
You can access the application at the following URLs:
- http://team22.local
- http://team22-dev.local
- http://grafana.local

Just make sure to have the tunnel running if you are using minikube and also add it to your hosts file.
```text
# sudo nano /etc/hosts
127.0.0.1 team22.local
127.0.0.1 team22-dev.local
127.0.0.1 grafana.local
```

## Configurable Helm Chart Values

Below are the editable values found in `values.yaml`. You can override these values at install/upgrade time using `--set key=value` or by providing your own YAML file with `-f my-values.yaml`.
| Key | Default Value | Description |
|-----|---------------|-------------|
| `global.domain` | `"local"` | Base domain for all hosts. |
| `global.stableSubdomain` | `"team22"` | Subdomain for the stable environment. |
| `global.prereleaseSubdomain` | `"team22-dev"` | Subdomain for the prerelease (canary) environment. |
| `global.ingressGatewayName` | `team22-gateway` | The Istio ingress gateway name. |
| `config.appConfig.FEATURE_FLAG_TEST` | `"true"` | Example feature flag for application. |
| `config.appConfig.API_TIMEOUT_MS` | `"5000"` | API timeout in milliseconds. |
| `app.stable.image.repository` | `ghcr.io/doda2025-team22/app` | Stable app image repository. |
| `app.stable.image.tag` | `latest` | Stable app image tag. |
| `app.stable.replicaCount` | `1` | Number of stable app replicas. |
| `app.stable.port` | `8080` | Port for stable app. |
| `app.canary.image.repository` | `ghcr.io/doda2025-team22/app` | Canary app image repository. |
| `app.canary.image.tag` | `canary` | Canary app image tag. |
| `app.canary.replicaCount` | `1` | Number of canary app replicas. |
| `app.canary.port` | `8080` | Port for canary app. |
| `app.canaryWeight` | `10` | Traffic percentage to send to canary. |
| `app.stableWeight` | `90` | Traffic percentage to send to stable. |
