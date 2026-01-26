## Cluster Setup
For testing with Minikube, start the cluster with:
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
## Installation
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
- http://prometheus.local

Just make sure to have the tunnel running and also add it to your hosts file.

```text
# sudo nano /etc/hosts
127.0.0.1 team22.local
127.0.0.1 team22-dev.local
127.0.0.1 grafana.local
127.0.0.1 prometheus.local
```
Istio resources used for traffic management can be found under team22_chart/templates. Ingress Gateway name can be configured via `global.ingressGatewayName` along with other [fields](../team22_chart/README-Helm.md#configurable-helm-chart-values).
## Configurable Helm Chart Values

Below are the editable values found in `values.yaml`. You can override these values at install/upgrade time using `--set key=value` or by providing your own YAML file with `-f my-values.yaml`.

| Key | Default Value                                    | Description                                                                                                                         |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `global.domain` | `"local"`                                        | Base domain for all hosts.                                                                                                          |
| `global.stableSubdomain` | `"team22"`                                       | Subdomain for the stable environment.                                                                                               |
| `global.prereleaseSubdomain` | `"team22-dev"`                                   | Subdomain for the prerelease (canary) environment.                                                                                  |
| `global.ingressGatewayName` | `team22-gateway`                                 | The Istio ingress gateway name.                                                                                                     |
| `config.appConfig.FEATURE_FLAG_TEST` | `"true"`                                         | Example feature flag for application.                                                                                               |
| `config.appConfig.API_TIMEOUT_MS` | `"5000"`                                         | API timeout in milliseconds.                                                                                                        |
| `app.stable.image.repository` | `ghcr.io/doda2025-team22/app`                    | Stable app image repository.                                                                                                        |
| `app.stable.image.tag` | `latest`                                         | Stable app image tag.                                                                                                               |
| `app.stable.replicaCount` | `1`                                              | Number of stable app replicas.                                                                                                      |
| `app.stable.port` | `8080`                                           | Port for stable app.                                                                                                                |
| `app.canary.image.repository` | `ghcr.io/doda2025-team22/app`                    | Canary app image repository.                                                                                                        |
| `app.canary.image.tag` | `canary`                                         | Canary app image tag.                                                                                                               |
| `app.canary.replicaCount` | `1`                                              | Number of canary app replicas.                                                                                                      |
| `app.canary.port` | `8080`                                           | Port for canary app.                                                                                                                |
| `app.canaryWeight` | `10`                                             | Traffic percentage to send to canary.                                                                                               |
| `app.stableWeight` | `90`                                             | Traffic percentage to send to stable.                                                                                               |
| `modelservice.image.repository` | `ghcr.io/doda2025-team22/model-service`          | Model service image repository.                                                                                                     |
| `modelservice.image.tag` | `latest`                                         | Model service image tag.                                                                                                            |
| `modelservice.replicaCount` | `1`                                              | Number of model service replicas.                                                                                                   |
| `modelservice.port` | `8081`                                           | Port for model service.                                                                                                             |
| `monitoring.enabled` | `true`                                           | Enable monitoring with Prometheus and Grafana.                                                                                      |
| `monitoring.prometheusRelease` | `monitoring`                                     | Helm release name for Prometheus.                                                                                                   |
| `kube-prometheus-stack.grafana.enabled` | `true`                                           | Enable Grafana via kube-prometheus-stack.                                                                                           |
| `kube-prometheus-stack.grafana.admin.existingSecret` | `grafana-admin-secret`                           | Secret name for Grafana admin credentials.                                                                                          |
| `kube-prometheus-stack.grafana.admin.userKey` | `admin-user`                                     | Secret key for Grafana admin username.                                                                                              |
| `kube-prometheus-stack.grafana.admin.passwordKey` | `admin-password`                                 | Secret key for Grafana admin password.                                                                                              |
| `kube-prometheus-stack.grafana.service.type` | `ClusterIP`                                      | Grafana service type.                                                                                                               |
| `kube-prometheus-stack.grafana.persistence.enabled` | `true`                                           | Enable persistent storage for Grafana.                                                                                              |
| `kube-prometheus-stack.grafana.persistence.storageClassName` | `""`                                             | Storage class for Grafana persistence.                                                                                              |
| `kube-prometheus-stack.grafana.persistence.size` | `10Gi`                                           | Grafana PVC size.                                                                                                                   |
| `kube-prometheus-stack.grafana.sidecar.datasources.enabled` | `true`                                           | Enable sidecar datasource provisioning.                                                                                             |
| `kube-prometheus-stack.grafana.sidecar.datasources.label` | `grafana_datasource`                             | Label for sidecar datasource.                                                                                                       |
| `kube-prometheus-stack.grafana.sidecar.datasources.labelValue` | `"1"`                                            | Value for sidecar datasource label.                                                                                                 |
| `kube-prometheus-stack.prometheus.prometheusSpec.serviceMonitorSelector.matchLabels.release` | `monitoring`                                     | Release label for service monitor.                                                                                                  |
| `kube-prometheus-stack.prometheus.prometheusSpec.serviceMonitorNamespaceSelector` | `{}`                                             | Namespace selector for service monitor.                                                                                             |
| `kube-prometheus-stack.alertmanager.enabled` | `false`                                          | Enables deployment of Alertmanager as part of the kube-prometheus-stack.                                                            |
| `kube-prometheus-stack.alertmanager.alertmanagerSpec.replicas` | `1`                                              | Number of Alertmanager pod replicas to run. Controls high availability.                                                             |
| `kube-prometheus-stack.alertmanager.alertmanagerSpec.secrets` | `["smtp-secret"]`                                | List of Kubernetes Secrets mounted into the Alertmanager pod. Used to provide sensitive data such as SMTP credentials.              |
| `kube-prometheus-stack.alertmanager.config.global.resolve_timeout` | `5m`                                             | How long Alertmanager waits before marking an alert as resolved after it stops firing.                                              |
| `kube-prometheus-stack.alertmanager.config.global.smtp_smarthost` | `"smtp.gmail.com:587"`                           | Address of the SMTP server used to send email notifications.                                                                        |
| `kube-prometheus-stack.alertmanager.config.global.smtp_from` | `"doda98124@gmail.com"`                          | Email address that Alertmanager uses as the sender of notification emails.                                                          |
| `kube-prometheus-stack.alertmanager.config.global.smtp_auth_username` | `"doda98124@gmail.com"`                          | Username used to authenticate with the SMTP server. Usually the email address itself for Gmail.                                     |
| `kube-prometheus-stack.alertmanager.config.global.smtp_auth_password_file` | `/etc/alertmanager/secrets/smtp-secret/password` | Path to a file inside the Alertmanager pod that contains the SMTP password. This file is provided by a mounted Kubernetes Secret.   |
| `kube-prometheus-stack.alertmanager.config.global.smtp_require_tls` | `true`                                           | Forces Alertmanager to use TLS when connecting to the SMTP server. Required for Gmail.                                              |
| `kube-prometheus-stack.alertmanager.config.route.receiver` | `team22-notifications`                           | Default receiver that all alerts are sent to unless overridden by routing rules.                                                    |
| `kube-prometheus-stack.alertmanager.config.route.group_by` | `["alertname","service"]`                        | Labels used to group multiple alerts into a single notification. Alerts with the same values for these labels are batched together. |
| `kube-prometheus-stack.alertmanager.config.route.group_wait` | `10s`                                            | How long Alertmanager waits before sending the first notification for a new alert group.                                            |
| `kube-prometheus-stack.alertmanager.config.route.group_interval` | `5m`                                             | Minimum time between notifications for the same group of alerts.                                                                    |
| `kube-prometheus-stack.alertmanager.config.receivers[].name` | `"team22-notifications"`                         | Name of a notification receiver. This is referenced by routing rules.                                                               |
| `kube-prometheus-stack.alertmanager.config.receivers[].email_configs[].to` | `"doda98124@gmail.com"`                          | Email address that will receive alert notifications from this receiver.                                                             |
| `kube-prometheus-stack.alertmanager.config.receivers[].name` | `"null"`                                         | Special receiver that discards alerts if needed.                                                                                    |
| `kube-prometheus-stack.alertmanager.config.receivers[].name` | `"Watchdog"`                                     | Receiver that handles the built-in Watchdog alert.                                                                                  |
| `grafana.enabled` | `true`                                           | Enable Grafana routing in Istio.                                                                                                    |
| `grafana.grafanaHost` | `"grafana.local"`                                | Host for Grafana UI ingress.                                                                                                        |
| `secret.smtp.username` | `"doda98124@gmail.com"`                          | SMTP username.                                                                                                                      |
| `secret.smtp.password` | `"ENTER_PASSWORD"`                               | SMTP password, app password gotten from teh email providers.                                                                        |
| `secret.grafana.adminUser` | `"admin"`                                        | Admin username for Grafana (used if not using a secret).                                                                            |
| `secret.grafana.adminPassword` | `"admin"`                                        | Admin password for Grafana (used if not using a secret).                                                                            |

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

## Testing Alerting
The Alertmanager pod is configured under `kube-prometheus-stack.alertmanager` in `values.yaml`. 
Alerts are sent via emails, which need a username and password for the SMTP server authentication. You can configure different servers, but default setup is for Gmail so you can use the following command:
```
helm upgrade --install team22 ./team22_chart \                               
--namespace team22 \
--set kube-prometheus-stack.alertmanager.enabled=true \
--set kube-prometheus-stack.alertmanager.config.global.smtp_auth_username=<your_gmail_address> \
--set kube-prometheus-stack.alertmanager.config.global.smtp_from=<your_gmail_address> \
--set secret.smtp.password=<your-app-password> \
--create-namespace
```
The password field can be filled with an [app password](https://myaccount.google.com/apppasswords). Please keep in mind that in some cases it takes time for the pod to initialize, so check first its status via  `kubectl get pods -n team22 ` before testing any further.

You can additionally configure the `kube-prometheus-stack.alertmanager.config.receivers[].email_configs[].to` field to configure which email address receives the alerting emails. Apart from our test alarm, our alert fires when more than 5 guesses (as in clicking the button to get a response form the model) within 30s are made for 30s straight. In a real production environment, we acknowledge that these limits should be more relaxed, but for easier testing and the purposes of our project we decided this was ideal.

### Sticky Sessions
After installation, you can check which version the request was assigned to based on the "You are in the stable version" or the "You are in the canary version" texts in the sms page. Multiple requests made to the page will result in the same version. PLease clear your cookies if you want to test another version.

### Continuous Experimentation
Please refer to the [detailed experimentation docs](../docs/continuous-experimentation.md)

### Rate Limiting
More than 10 requests made in 1 minute to the application will be limited and will result in a 429. This parameter can be configured via `ratelimit.smsPageRpm` in `values.yaml`.

To test this functionality, please make sure that `ratelimit.enable=true`.
You can either manually test it by refreshing the page after booting up the application, or via using the script below:
```
for i in {1..15}; do                                                
  code=$(curl -s -o /dev/null -w "%{http_code}" \
    http://team22.local) 
  echo "Request $i → $code"
done
```
Disclaimer: Depending on your setup, if the requests take too long, the number in which the limiting occurs can slightly shift because of the 1-minute sliding window. If this is the case, it can be clearer to test it from a browser.

As can be seen, requests after the 10th are globally limited. The configurations can be found under templates/rate-limit.
- `rate-limiting-config.yml` -> the ConfigMap where we specify the constraint
- `rate-limiting-envoy-filter-http.yaml` -> the EnvoyFilter used to establish connection to the rate-limiting service
- `rate-limiting-envoy-filter-path.yaml` -> the EnvoyFilter used to configure what is sent to the service and when the rule is applied
- `rate-limiting-service.yaml` -> the resources used to keep track of the amount of requests, gotten from [Istio docs](https://istio.io/latest/docs/tasks/policy-enforcement/rate-limit/).