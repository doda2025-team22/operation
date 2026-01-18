# A3 Review Document

## How to review?
The 'Completed?' field is annoted with '✓' for Sufficient and above to indicate that the requirement has been met, and 'X' for Poor and below in order to indicate that the assignment does NOT contain Insufficient/Poor characteristics. The description field provides a description of why the team thinks the requirement has been met, alongside how one can check these requirements.

## Rubric Checklist
| Rubric item level | Rubric item | Completed? | Link | Description |
|-------------------|-------------|-------|------|-------------|
| **Kubernetes Usage** |  |  |  |  |
| Insufficient | | |  | The deployment does contain deployments/services, and deploying to kubernetes does not fail. |
| | The deployment does not contain a Deployment or a Service or deploying to Kubernetes fails. | [X] |  |  |
| Poor | |  |  | Can be accessed without a node port/service tunnel (with a LoadBalancer) |
| | The deployment is successful, but can only be accessed through a node port or a service tunnel. | [X] |  |  |
| Sufficient | | | | For all sufficient reqs, simply following the helm commands in the usage README works: The app CAN be deployed to a kubernetes cluster, does have working deployments/services(deployment.yaml, service.yaml), is accessed through an ingress (app-ingress.yaml), and simply running helm install (after helm dependecy) is enough to install everything. |
| | The application can be deployed to a Kubernetes cluster. | [✓] |  |  |
| | The relevant deployment files contain at least a working Deployment and a working Service. | [✓] |  |  |
| | The app is accessed through an Ingress. (The cluster provisioning creates a suitable IngressController.) | [✓] |  |  |
| | All aspects are installed through the central Helm chart. | [✓] |  |  |
| Good | |  |  | Model service name is configurable from the helm chart values.yaml, which can be set while installing the helm chart. You can find seperate use cases for a ConfigMap and Secret each: with ConfigMap being defined in configmap.yaml and then being used from that file in deployment.yaml via valueFrom/configMapKeyRef, and Secret in secret.yaml. Since the goal is to showcase our knowledge of the use of these, a seperate valueFrom/configMapKeyRef clause for Secret was not utilized.|
| | The deployed application defines the location of the model service through an environment variable. | [✓] |  |  |
| | The model service can be relocated just by changing the Kubernetes config. (For example, changing the name of the service or the port on which it is running.) | [~] |  | Model service name is not hard coded anymore, but changing ports does not work. Port is hardcoded inside 'serve_model.py' so we most likely need a new release to address this issue. |
| | The deployed application successfully uses a ConfigMap and a Secret. (Define one to show that you know how, even though your application might not need one.) | [✓] |  |  |
| | The hostname/URL of the app can be changed through the value.xml of the Helm chart. (To support grading, I want to be able to pick the URLs that I use to access the stable/pre-release versions.) | [✓] |  |  |
| Excellent | |  |  |  |
| | All VMs mount the same shared VirtualBox folder as /mnt/shared into the VM. | [X] |  |  |
| | The deployed application mounts this path as a hostPath Volume into at least one Deployment. | [X] |  |  |
| **App Monitoring** |  |  |  |  |
| Insufficient | |  |  | The app defines three metrics (Gauge, Counter, Histogram) that Prometheus picks up automatically. |
| | The app defines less than three metrics or Prometheus does not automatically collect the values. | [X] |  |  |
| Poor | |  |  | There are indeed examples of both Gauge and Counter metrics for the frontend. We also have our own endpoint instead of utillizing a framework. |
| | The app metrics are lacking an example for either Gauge or Counter. | [X] |  |  |
| | The app metrics are exposed through a framework (create the /metrics endpoints yourself) | [X] |  |  |
| Sufficient | |  |  | The first 4 points can be observed by running 'kubectl get servicemonitor -n team22' and 'kubectl describe servicemonitor app-service -n team22', and the last point can be observed by running 'curl http://team22.local/sms/metrics'.|
| | The app has 3+ app-specific and UI-related metrics for reasoning about user behavior or the perception of the app from the perspective of users. | [✓] |  |  |
| | The metrics are automatically discovered and collected by Prometheus through ServiceMonitor. | [✓] |  |  |
| | The content of the /metrics endpoint is created by yourself (not by a library). | [✓] |  |  |
| | All aspects are installed through the central Helm chart. | [✓] |  |  |
| | The /metrics endpoint of the frontend-service is reachable through the Ingress. (This is a measure to support grading of setups with separate containers for the web UI and the API gateway.) | [✓] |  |  |
| Good | |  |  | There are indeed examples of Gauge,  Counter, and (app-specific) Histogram metrics with each containing an example. This can be observed from the output of running: 'curl http://team22.local/sms/metrics'|
| |The exposed metrics include a Gauge , a Counter , and an app-specific Histogram metric. | [✓] |  |  |
| | Each metric type has at least one example, in which the metric is broken down with labels. | [✓] |  |  |
| Excellent | |  |  | Can be observed in 'test-alert.yaml', in case of Alert we do indeed get an email. The email's credentials (or any other credentials for that matter of fact) are not hardcoded into the helm chart. |
| | An AlertManager is configured with at least one non-trivial PrometheusRule. | [✓] |  |  |
| | A corresponding Alert is raised in any type of channel (e.g., via email). | [✓] |  |  |
| | The deployment files and the source code must not contain credentials (e.g., SMTP passwords).(Instead, require pre-deployed Secret s and use them through environment variables.) | [✓] |  |  |
| **Grafana** |  |  |  |  |
| Insufficient | |  |  | There are 2 dashboards defined (inside /grafana-dashboards) that are nontrivial. |
| | No dashboard is defined or it is trivial. | [X] |  |  |
| Poor | |  |  | The dashboards are complete, and can be imported without errors. |
| | A serious dashboard attempt exists, but it is incomplete or can only be imported with errors. | [X] |  |  |
| Sufficient | |  |  | There are 2 dashboards which are defined in JSON files, with one of them supporting the experimental decision of A4 inside '/grafana-dashboards' |
| | A basic Grafana dashboard exists that illustrates all app-specific metrics. | [✓] |  |  |
| | A second Grafana dashboard exists that supports the experimental decision of A4. | [✓] |  |  |
| | Both dashboards are defined in a JSON file and can be manually imported in the Web UI. | [✓] |  |  |
| | The operations repository contains a README.md that explains the manual installation (if required). | [✓] |  | Not required |
| Good | | |  | The basic dashboard has Gauges (spam_guess_gauge, total_guess_counter), time series (Predictions / min, Average SMS length) and a bar chart (SMS length histogram), while the experimental dashboard has stat (Average latency per version), a heatmap (Model Request Histogram v2), and a timeseries (Request latency percentiles). rate() and histogram_quantile() used, and time.from and time.to are utilized.|
| | The basic dashboard contains at least two diﬀerent visualizations that are not just time series. (For example, a visual Gauge representation or a bar chart for Histograms.) | [✓] |  |  |
| | The basic dashboard employs variable timeframe selectors to parameterize the queries. | [✓] |  |  |
| | The basic dashboard applies functions (like rate or avg ) to enhance the plots. | [✓] |  |  |
| Excellent | | |  | The dashboards are automatically installed through the helm chart, via  grafana-dashboard.yaml and grafana-ingress.yaml |
| | Both Grafana dashboards are automatically installed by the central Helm chart. | [✓] |  |  |