# Deployment Documentation

## Deployment Overview

This project is designed to fully create, provision and deploy a simple spam detection machine learning algorithm and its assiciated user interface onto a virtaul machine cluster. As we are omitting the creation and provisioning of the two virtual machines, this documentation will focus on the deployment, that runs on kubernetes for basic communication and uses an istio service mesh as an advanced form of traffic management and observation.

## Kubernetes Architecture

### Deployments

This project relies on Kubernetes to form its base cluster network. To this end it contains various deployments, names a seperate deployment for each of the versions for the app's, and another for each of the model-service's. These deployments create identical replicas of pods that allow the application to run. We create multiple replicas of the same version of each application, to ensure that in the case of a pod breaking, the other pods can take the requests, while kubernetes automatically recreates and reservices this pod, it allows allows for request distribution, minimising the strain on any individual component of the system. Each deployment also defines a specific port(s) that they listen on allowing them to receive the HTTP requests from the Service. In our application, the application frontend listen on the target port 8080, while the model-service backend listens to the target port 8081.

### Services

Kubernetes Services provide stable network identities for sets of pods. Services ensure pod-to-pod communication remains stable even as pods are recreated or resceduled. Im This deployment, services are used to expose both the application frontend and the model-service internally within the cluster. Service route requests to the appropriate pod replicas using label selectors and port-forwarding, enabling transparent failover and load balancing.

### Ingress Gateways

To expose the application ot external users, an ingress gateway is required. The ingress gateway creates an Entry point to the cluster, which allows users to access the frontend application through an HTTP protocol. The ingress gateway receives and redirects all HTTP traffic from user requests to the requested/required service, which then handles the remaining logic, such as service to service communications to ensure that the frontend can make a request to the machine learning model. Bare-metal servers and Virtaul Machines do not have a reachable external IP's, without cloud-provider management, therefore to provide an externap ip address to out VM application, we use MetaLB to assing addresses to our Services.

### Configuration Maps and Secrets

Configuration data is managed using Kubernetes ConfigMaps and Secrets. ConfigMaps store non-sensitive configuration values, while Secrets are used for sensitive data such as credentials or tokens. These resources are decoupled from application code and can be shared across deployments, improving maintainability and consistency.

### Namespaces and Access Control

To support logical seperation and operational clarity, the cluster is organized using Kubernetes namespaces. Namespaces introduce logical boundaries within the cluster alloweing resources to be grouped by porpose and enabling naming reuse across different parts of the system. Namespaces however do not enforce access restrictions by default. Access control is implemented in Kubernetes Role-Based Access Control (RBAC) in combination with ServiceAccounts. RBAC defines which actions are permitted within a namespace or across the cluster, while ServiceAccounts represent identities used by applications or operational tools when interacting with the Kubernetes API. This mechanism enables fine-grained permission management and limits access to sensitive cluster resources.

### Monitoring and Observations

To observe the state and behavior of the deployment, multiple monitoring tools are used:

- The Kubernetes Dashboard provides a cluster-level overview, including pods, services, namespaces, and deployment status.
- Prometheus collects metrics related to application performance and traffic.
- Grafana visualizes these metrics through dashboards, enabling analysis of request rates, latency, error rates, and comparisons between different application versions

## Istio Service Mesh Architecture

Istio extends the Kubernetes deployment with a service mesch that provides fine-grained traffic control, observability and policy enfocemoent.

### Sidecars

Istio operates by injecting an Envoy sidecar into selected application pods. These sidecars intercept inbound and outbound traffic, enabling traffic management and telemetry without requiring changes to the application code. Isto also allows the definition of a project wide sidecar which then automatically attaches a sidecar to each defined pod, however, this wasn't used as it is a less fine-grained approach.

### Istio Ingress Gateway

Instead of using a standard Kubernetes ingress controller, this deployment uses the Istio Ingress Gateway, which is fully ingress gateway. The ingress gateway integrates with Istio's routing and policy mechanisms and serves as the new external entry point for all HTTP traffic.

### Traffic Routing and Canary Releases

Traffic management in Istio is defined using VirtualService and DestinationRule resources.

- Destination Rules define subsets corresponding to different application versions.
- VirtualServices define how traffic is routes to these subsets.
  Routing decisions are made at the ingress gateway and following a hierarchical logic, in our case:

1. If a request contains a specific cookie indicating a stable or canary version is desired, it is routed accordingly.
2. If no matching cookie is present, traffic is split dynamically using a 90/10 canary release strategy, where 90% of all requests are routed to the stable version and 10% to the canary release.
   This routing logic enable controlled experimentation while allowing for controlled and benchmarked observations using the dashboards.

### Rate Limiting Extension

As an extension to the base project, global HTTP rate limiting is implementaed at the Istio Ingress Gateway.
Rate limiting is enforced using Envoy's HTTP rete-limiting filter, configured via an EnvoyFilter resource. The filter intercepts incoming HTTP requests at the gateway and extracts request attributes, in our case the request path. These attributes are sent via gRPC to an external rate-limit service.
The rate-limit service evaluates the request against configured policies and stores request counters in a Redis backend to ensure consistency across multiple Ingress Gateway replicas. If a request exceeds the configured threshold, the gateway responds with an HTTP 429 error. As a failsafe, if the rate-limit service is unavailable, requests are immediately denied to prevent uncontrolled loads on the system.

## Request Flow throug the System

```code
For Graphical representation of the Kubernetes architecture:

- use the Kubernetes dashboard to show the live resource status
- use Kiali for the service mesh
- use kubectl-graph for teh CLI tool relationship diagram
```

(add the diagrams here.)
use the figures to explain the request flow path and show where the split decisions are made.
