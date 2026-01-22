# A4 Review Document

Istio resources used for traffic management can be found under team22_chart/templates. Ingress Gateway name can be configured via `global.ingressGatewayName` along with other [fields](../team22_chart/README-Helm.md#configurable-helm-chart-values).

### Sticky Sessions
After installation, run the following command in a separate terminal:
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
    http://192.168.49.2:<port number>/sms/library-version | grep version
done
```
A stable version results is version 0.05, while a canary version results in 0.0.1.

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
## Rubric Checklist
| Rubric item level | Rubric item | Completed? | Location?                                                                  | Description                                                |
|-------------------|-------------|------------|----------------------------------------------------------------------------|------------------------------------------------------------|
| **Traffic Management** |  |            |                                                                            |                                                            |
| Insufficient | The system does not define a Gateway or a VirtualService. | [ ]        |                                                                            |                                                            |
| Poor | The app contains an attempt of using Gateway and VirtualServices, but the app is not accessible. | [ ]        |                                                                            |                                                            |
| Poor | The app contains a hard-coded name for the IngressGateway and does not work with other names. | [ ]        |                                                                            |                                                            |
| Sufficient | The project resembles the state of the in-class exercise. | [X]        |                                                                            |                                                            |
| Sufficient | The app defines a Gateway and VirtualServices. | [X]        |                                                                            |                                                            |
| Sufficient | The application is accessible through the IngressGateway (i.e., minikube tunnel ). | [X]        |                                                                            |                                                            |
| Sufficient | The IngressGateway is defined outside of the Helm chart. | [X]        |                                                                            |                                                            |
| Sufficient | All aspects are installed through the central Helm chart (see A3). | [X]        |                                                                            |                                                            |
| Good | It uses DestinationRules and weights to enable a 90/10 routing of the app service. | [X]        | templates/app-ingress.yaml                                                 |                                                            |
| Good | The old/new versions of model-service and app-service are used consistently. For example, no access from the new app-service to the old model service. | [X]        | templates/app-ingress.yaml                                                 |                                                            |
| Excellent | The project implements Sticky Sessions, i.e., requests from the same origin have a stable routing. | [X]        | templates/app-ingress.yaml                                                 | Given via DestionationRule and VirtualService definitions. |
| **Additional Use Case** |  |            |                                                                            |                                                            |
| Insufficient | No additional use case has been realized or its complexity does not compare to the given examples. | [ ]        |                                                                            |                                                            |
| Poor | An additional use case has been attempted, but it does not work. | [ ]        |                                                                            |                                                            |
| Sufficient | One of the described use cases has been partially realized, with observable effects. | [ ]        |                                                                            |                                                            |
| Sufficient | All aspects are installed through the central Helm chart (see A3). | [X]        |                                                                            |                                                            |
| Good | One of the described use cases has been realized. It generally works, but falls short in some aspects. For example, instead of an individual, user-based rate-limit, only a global rate-limit exists that affects everyone. | [X]        | templates/rate-limit                                                       | A global rate-limit is used.                               |
| Excellent | One of the described use cases has been fully realized. | [ ]        |                                                                            |                                                            |
| **Continuous Experimentation: (The documentation refers to the docs/continuous-experimentation.md file)** |  |            |                                                                            |                                                            |
| Insufficient | The experiment has severe limits, like lacking a second deployment or relevant metrics. | [ ]        |                                                                            |                                                            |
| Poor | An experiment is attempted, but lacks either sufficient documentation or implementation. | [ ]        |                                                                            |                                                            |
| Sufficient | The documentation describes the experiment. It explains the implemented changes, the expected effect that gets experimented on, and the relevant metric that is tailored to the experiment. | [X]        |                                                                            |                                                            |
| Sufficient | The experiment involves two deployed versions of at least one container image. | [X]        |                                                                            |                                                            |
| Sufficient | Both component versions are reachable through the deployed experiment. | [X]        |                                                                            |                                                            |
| Sufficient | The system implements the metric that allows exploring the concrete hypothesis. | [X]        |                                                                            |                                                            |
| Sufficient | All aspects are installed through the central Helm chart (see A3). | [X]        |                                                                            |                                                            |
| Good | Prometheus picks up the metric. | [X]        |                                                                            |                                                            |
| Good | Grafana has a dashboard to visualize the differences and support the decision process. | [X]        | docs/continuous-experimentation.md or Grafana dashboard after isntallation |                                                            |
| Good | The documentation contains a screenshot of the visualization. | [X]        | docs/continuous-experimentation.md                                         |                                                            |
| Good | The hostname of the pre-release app can be configured in the value.xml of the Helm chart. To support grading, I want to be able to pick the URLs that I use to access the stable/pre-release versions. | [X]        | values.yaml                                                                |                                                            |
| Excellent | The documentation explains the decision process for accepting or rejecting the experiment in details, ie.g., which criteria is used and how the available dashboard supports the decision. | [X]        | docs/continuous-experimentation.md                                         |                                                            |
| **Deployment Documentation: (The documentation refers to the docs/deployment.md file)** |  |            |                                                                            |                                                            |
| Insufficient | The documentation is incomprehensible or incomplete. | [ ]        |                                                                            |                                                            |
| Poor | The documentation is limited to the deployment structure or the data flow. | [ ]        |                                                                            |                                                            |
| Sufficient | The documentation describes the deployment structure, i.e., the entities and their connections. | [ ]        |                                                                            |                                                            |
| Sufficient | The documentation describes the data flow for incoming requests. Also indicate at which points dynamic routing decisions are taken (e.g., 90/10 split). | [ ]        |                                                                            |                                                            |
| Sufficient | The documentation contains visualizations that are connected to the text. | [ ]        |                                                                            |                                                            |
| Good | The documentation includes all deployed resource types and their relations. It is unnecessary to include all details for each CRD, but effects and relations should become clear. | [ ]        |                                                                            |                                                            |
| Excellent | The documentation is visually appealing and clear. A new team member could contribute in a design discussion after studying the documentation. | [X]        |                                                                            |                                                            |
| **Extension Proposal: (The documentation refers to the docs/extension.md file)** |  |            |                                                                            |                                                            |
| Insufficient | The extension is unrelated to release engineering and focuses on an implementation aspect. | [ ]        |                                                                            |                                                            |
| Poor | The extension is trivial, irrelevant for the project, or refers to an unimplemented assessment criterion. | [ ]        |                                                                            |                                                            |
| Sufficient | The documentation describes one release-engineering-related shortcoming of the project practices. | [X]        |                                                                            |                                                            |
| Sufficient | A proposed extension addresses the shortcoming and is connected to one of the assignments. For example, the training or release pipelines, contribution process, deployment, or experimentation. | [X]        | Concerned with outdated deployments.                                       |                                                            |
| Sufficient | The extension is genuine and has not already been mentioned in any of the assignment rubrics. | [X]        |                                                                            |                                                            |
| Sufficient | The documentation cites external sources that inspired the envisioned extension. | [X]        |                                                                            |                                                            |
| Good | The shortcoming is critically reflected on and its negative effects get elaborated in detail. | [X]        |                                                                            |                                                            |
| Good | The presented extension improves the described shortcoming. | [X]        |                                                                            |                                                            |
| Good | The documentation explains how an improvement could be measured objectively in an experiment. | [X]        |                                                                            |                                                            |
| Excellent | The presented extension is general in nature and applicable beyond the concrete project. | [X]        |                                                                            |                                                            |
| Excellent | The presented extension clearly overcomes the described shortcoming. | [X]        |                                                                            |                                                            |
