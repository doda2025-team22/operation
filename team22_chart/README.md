### Using with 'minikube tunnel' + nginx
1- Start your cluster
If you want to start your cluster through minikube please run
```
minikube start
minikube addons enable ingress
```

2- Configure hosts file
Be sure to execute:

- 'sudo nano /etc/hosts'

or find your /etc/hosts file and add the following two lines to the file:

- 192.168.49.2    team22.local team22-dev.local
- 127.0.0.1    team22.local team22-dev.local

I am not sure if both is required or which one is correct so just add both and save yourself the headache.

3- Install Helm Chart
After these are added, you can run:

- 'helm install CHARTNAME team22_chart' to download
- 'helm delete CHARTNAME' to delete

and finally, run:

- 'sudo minikube tunnel'

After which you can visit http://team22.local (and http://team22-dev.local) to use the application.

### Prometheus Monitoring

After your helm chart is installed. You can run the following commands for the Prometheus setup for monitoring.

```
helm repo add prom-repo https://prometheus-community.github.io/helm-charts
helm repo update
helm install myprom prom-repo/kube-prometheus-stack
```

the metrics and the dashboard should be available at /metrics


