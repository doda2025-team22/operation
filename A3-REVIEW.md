# A3 Review Document

## How to review?
enter description here


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
| Good | |  |  |  |
| | The deployed application defines the location of the model service through an environment variable. | [~] |  | URL not fully hard coded, but service name is. (model-service) |
| | The model service can be relocated just by changing the Kubernetes config. (For example, changing the name of the service or the port on which it is running.) | [X] |  | Again, model service name is hard coded. |
| | The deployed application successfully uses a ConfigMap and a Secret. (Define one to show that you know how, even though your application might not need one.) | [~] |  | We indeed have one but don't fully "use" it, should reference configmap.yaml instead of values.yaml configmap vars. |
| | The hostname/URL of the app can be changed through the value.xml of the Helm chart. (To support grading, I want to be able to pick the URLs that I use to access the stable/pre-release versions.) | [✓] |  |  |
| Excellent | |  |  |  |
| | All VMs mount the same shared VirtualBox folder as /mnt/shared into the VM. | [X] |  |  |
| | The deployed application mounts this path as a hostPath Volume into at least one Deployment. | [X] |  |  |
| **App Monitoring** |  |  |  |  |
| Insufficient | The app defines less than three metrics or Prometheus does not automatically collect the values. | [ ] |  |  |
| Poor | The app metrics are lacking an example for either Gauge or Counter. | [ ] |  |  |
| | The app metrics are exposed through a framework (create the /metrics endpoints yourself) | [ ] |  |  |
| Sufficient | The app has 3+ app-specific and UI-related metrics for reasoning about user behavior or the perception of the app from the perspective of users. | [ ] |  |  |
| | The metrics are automatically discovered and collected by Prometheus through ServiceMonitor. | [ ] |  |  |
| | The content of the /metrics endpoint is created by yourself (not by a library). | [ ] |  |  |
| | All aspects are installed through the central Helm chart. | [ ] |  |  |
| | The /metrics endpoint of the frontend-service is reachable through the Ingress. (This is a measure to support grading of setups with separate containers for the web UI and the API gateway.) | [ ] |  |  |
| Good |The exposed metrics include a Gauge , a Counter , and an app-specific Histogram metric. | [ ] |  |  |
| | Each metric type has at least one example, in which the metric is broken down with labels. | [ ] |  |  |
| Excellent | An AlertManager is configured with at least one non-trivial PrometheusRule. | [ ] |  |  |
| | A corresponding Alert is raised in any type of channel (e.g., via email). | [ ] |  |  |
| | The deployment files and the source code must not contain credentials (e.g., SMTP passwords).(Instead, require pre-deployed Secret s and use them through environment variables.) | [ ] |  |  |
| **Grafana** |  |  |  |  |
| Insufficient | No dashboard is defined or it is trivial. | [ ] |  |  |
| Poor | A serious dashboard attempt exists, but it is incomplete or can only be imported with errors. | [ ] |  |  |
| Sufficient | A basic Grafana dashboard exists that illustrates all app-specific metrics. | [ ] |  |  |
| | A second Grafana dashboard exists that supports the experimental decision of A4. | [ ] |  |  |
| | Both dashboards are defined in a JSON file and can be manually imported in the Web UI. | [ ] |  |  |
| | The operations repository contains a README.md that explains the manual installation (if required). | [ ] |  |  |
| Good | The basic dashboard contains at least two diﬀerent visualizations that are not just time series. (For example, a visual Gauge representation or a bar chart for Histograms.) | [ ] |  |  |
| | The basic dashboard employs variable timeframe selectors to parameterize the queries. | [ ] |  |  |
| | The basic dashboard applies functions (like rate or avg ) to enhance the plots. | [ ] |  |  |
| Excellent | Both Grafana dashboards are automatically installed by the central Helm chart. | [ ] |  |  |