## Usage
For testing we use Minikube. Start the cluster with:
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
sudo minikube tunnel
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

If you are using a different cluster, you can access the application at the following URLs:
- http://team22.local
- http://team22-dev.local
- http://grafana.local

Just make sure to have the tunnle running and also add it to your hosts file.

```text
# sudo nano /etc/hosts
127.0.0.1 team22.local
127.0.0.1 team22-dev.local
127.0.0.1 grafana.local
```

## Configurable Helm Chart Values

Below are the editable values found in `values.yaml`. You can override these values at install/upgrade time using `--set key=value` or by providing your own YAML file with `-f my-values.yaml`.

| Key                                                                                          | Default Value                           | Description                                                                |
|----------------------------------------------------------------------------------------------|-----------------------------------------|----------------------------------------------------------------------------|
| `global.domain`                                                                              | `"local"`                               | Base domain for all hosts.                                                 |
| `global.stableSubdomain`                                                                     | `"team22"`                              | Subdomain for the stable environment.                                      |
| `global.prereleaseSubdomain`                                                                 | `"team22-dev"`                          | Subdomain for the prerelease (canary) environment.                         |
| `global.ingressGatewayName`                                                                  | `team22-gateway`                        | The Istio ingress gateway name.                                            |
| `config.appConfig.FEATURE_FLAG_TEST`                                                         | `"true"`                                | Example feature flag for application.                                      |
| `config.appConfig.API_TIMEOUT_MS`                                                            | `"5000"`                                | API timeout in milliseconds.                                               |
| `app.stable.image.repository`                                                                | `ghcr.io/doda2025-team22/app`           | Stable app image repository.                                               |
| `app.stable.image.tag`                                                                       | `latest`                                | Stable app image tag.                                                      |
| `app.stable.replicaCount`                                                                    | `1`                                     | Number of stable app replicas.                                             |
| `app.stable.port`                                                                            | `8080`                                  | Port for stable app.                                                       |
| `app.canary.image.repository`                                                                | `ghcr.io/doda2025-team22/app`           | Canary app image repository.                                               |
| `app.canary.image.tag`                                                                       | `canary`                                | Canary app image tag.                                                      |
| `app.canary.replicaCount`                                                                    | `1`                                     | Number of canary app replicas.                                             |
| `app.canary.port`                                                                            | `8080`                                  | Port for canary app.                                                       |
| `app.canaryWeight`                                                                           | `10`                                    | Traffic percentage to send to canary.                                      |
| `app.stableWeight`                                                                           | `90`                                    | Traffic percentage to send to stable.                                      |
| `modelservice.image.repository`                                                              | `ghcr.io/doda2025-team22/model-service` | Model service image repository.                                            |
| `modelservice.image.tag`                                                                     | `latest`                                | Model service image tag.                                                   |
| `modelservice.replicaCount`                                                                  | `1`                                     | Number of model service replicas.                                          |
| `modelservice.port`                                                                          | `8081`                                  | Port for model service.                                                    |
| `monitoring.enabled`                                                                         | `true`                                  | Enable monitoring with Prometheus and Grafana.                             |
| `monitoring.prometheusRelease`                                                               | `monitoring`                            | Helm release name for Prometheus.                                          |
| `kube-prometheus-stack.grafana.enabled`                                                      | `true`                                  | Enable Grafana via kube-prometheus-stack.                                  |
| `kube-prometheus-stack.grafana.admin.existingSecret`                                         | `grafana-admin-secret`                  | Secret name for Grafana admin credentials.                                 |
| `kube-prometheus-stack.grafana.admin.userKey`                                                | `admin-user`                            | Secret key for Grafana admin username.                                     |
| `kube-prometheus-stack.grafana.admin.passwordKey`                                            | `admin-password`                        | Secret key for Grafana admin password.                                     |
| `kube-prometheus-stack.grafana.service.type`                                                 | `ClusterIP`                             | Grafana service type.                                                      |
| `kube-prometheus-stack.grafana.persistence.enabled`                                          | `true`                                  | Enable persistent storage for Grafana.                                     |
| `kube-prometheus-stack.grafana.persistence.storageClassName`                                 | `""`                                    | Storage class for Grafana persistence.                                     |
| `kube-prometheus-stack.grafana.persistence.size`                                             | `10Gi`                                  | Grafana PVC size.                                                          |
| `kube-prometheus-stack.grafana.sidecar.datasources.enabled`                                  | `true`                                  | Enable sidecar datasource provisioning.                                    |
| `kube-prometheus-stack.grafana.sidecar.datasources.label`                                    | `grafana_datasource`                    | Label for sidecar datasource.                                              |
| `kube-prometheus-stack.grafana.sidecar.datasources.labelValue`                               | `"1"`                                   | Value for sidecar datasource label.                                        |
| `kube-prometheus-stack.prometheus.prometheusSpec.serviceMonitorSelector.matchLabels.release` | `monitoring`                            | Release label for service monitor.                                         |
| `kube-prometheus-stack.prometheus.prometheusSpec.serviceMonitorNamespaceSelector`            | `{}`                                    | Namespace selector for service monitor.                                    |
| `grafana.enabled`                                                                            | `true`                                  | Enable Grafana routing in Istio.                                           |
| `grafana.grafanaHost`                                                                        | `"grafana.local"`                       | Host for Grafana UI ingress.                                               |
| `secret.smtp.username`                                                                       | `"usrnm"`                               | SMTP username.                                                             |
| `secret.smtp.password`                                                                       | `"pswrd"`                               | SMTP password.                                                             |
| `secret.grafana.adminUser`                                                                   | `"admin"`                               | Admin username for Grafana (used if not using a secret).                   |
| `secret.grafana.adminPassword`                                                               | `"admin"`                               | Admin password for Grafana (used if not using a secret).                   |
| `ratelimit.enable`                                          | `"true"`                                | Turns on denying requests that reach the ppsecified threshold in a minute. |
| `ratelimit.smsPageRpm`                                          | `"10"`                                  | Configures the allowed number of requests per minute.                      |

You can override any value using the following syntax:
```bash
helm upgrade --install team22 ./team22_chart --set app.stable.replicaCount=3
```
Or with a custom YAML:
```bash
helm upgrade --install team22 ./team22_chart -f my-values.yaml
```

## Accessing the Grafana dashboard
You can access the Grafana dashboard at the following URL:
- http://grafana.local

The credentials are:
- Username: admin
- Password: admin

These credentials are the default credentials for the Grafana dashboard. You can change them by changing the `secret.grafana.adminUser` and `secret.grafana.adminPassword` values in the values.yaml file.

## Tips for testing with Minikube
- If you are using a Minikube cluster and having problems, the following points might be helpful:
- Make sure ingress addon is enabled in your cluster.
- Install istio in your cluster using ``` istioctl install```
- Do not forget to ``sudo minkube tunnel`` before accessing the app through the links.
- If you see the error ``no stable upstream branch``, please wait a bit or refresh.

## For testing with Linux / In case of other errors
- Run ```minikube service list```.
In the output, if you recieve urls for istio-system, copy the port number of the url associated with http/80 (the target port). 

- If seeing the error "No healthy upstream", please run `kubectl get pods --all-namespaces` to make sure the needed containers are created before testing again.