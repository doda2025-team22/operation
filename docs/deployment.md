# First Draft

## Kubernetes

This project uses Kubernetes as the primary control plane for application workloads. Kubernetes is responsible for managing the lifecycle of application components, this includes handling unforseen crashes and user load balancing.
The deployment primarily relies on core Kubernetes abstractions including Deployments, Pods, Services and an Ingress Gateway, as well as ConfigMaps and Secrets.

Each application component is deployed using a Kubernetes Deployment. A Deployment manages one or more identical pod replicas, ensuring that the application instances are automatically restarted in case of failure and can be scaled to handle increased loads.
Kubernetes Services are used to expose each Deployment internaly within the cluster. A Service provides a stable virtual IP adress and DNS name, allowing pod from different Deployments to communicate without needing to know the concrete pod IP's. This enables reliable inter-service communication even as pods are dynamically created, destroyed or rescheduled across nodes.

External access to the application is provided via an ingress gateway, which acts as the single entry point into the cluster for HTTP requests. Incoming requrests are routed to the frontend Service, which then distributes traffic across the availavle frontend pod replicas. From the frontend components, the internal service-to-service communication proceeds via Kubernete Services to downstream components such as the model-service.

#### Still missing

- ConfigMaps
- Secrets
- Possibly Namespaces
- ServiceAccount

## Istio

Istio is deployed as an extension on top of the existing Kubernetes setup and augments the cluster with more advanced traffic routing capabilities. All inconming traffic enters the service mesh through the Istio Ingress Gateway, which serves as the controlled entry point for external HTTP requests. From the gateway, the requests are forwarded to the frontend service, defined in Kubernetes. In contrast to a standard Kubernetes Ingress, the Istio ingress Gateway is mesh-aware and and integrates directly with Istio's traffic management configuration.
The primary purpose of Istio in this project is the use of VirtualServices andn DestinationRules to implement dynamic traffic routing for experimentation and continuous delivery. DestinationRules are used to define logical subsets of a service, representing different versions of the application. VirtualServices then specify how traffic is routed between these subsets.
The deployment implements two forms of dynamic traffic routing. The first is a weighted traffic split, where 90% of incoming requests are routed to the stable version of the application and model-service, while the remaining 10% are routed to a canary version. This routing behaviour is defined in a VirtualServie and applied on a per-request basis.
The second routing mechanism, which has a higher priority than the first, is a header-based routing protocol. This enables sticky access to a specific application version. By including a predefined HTTP header in the request, traffic is routed ot a corresponding service subset.

#### Still missing

- rate limiting (?)
- grafana/dashboards (?)

# Second Draft

## Deployment Overview

This project is designed to fully create, provision and deploy a simple spam detection machine learning algorithm and its assiciated user interface onto a virtaul machine cluster. As we are omitting the creation and provisioning of the two virtual machines, this documentation will focus on the deployment, that runs on kubernetes for basic communication and uses an istio service mesh as an advanced form of traffic management and observation.

## Kubernetes Architecture

```code
For Graphical representation of the Kubernetes architecture:

- use the Kubernetes dashboard to show the live resource status
- use Kiali for the service mesh
- use kubectl-graph for teh CLI tool relationship diagram
```

This project relies on Kubernetes to form its base cluster network. To this end it contains various deployments, names a seperate deployment for each of the versions for the app's, and another for each of the model-service's. These deployments create identical replicas of pods that allow the application to run. We create multiple replicas of the same version of each application, to ensure that in the case of a pod breaking, the other pods can take the requests, while kubernetes automatically recreates and reservices this pod, it allows allows for request distribution, minimising the strain on any individual component of the system.

**Missing Metallb deployment, do i mention it?**

Kubernetes uses another component called a Service to ensure pod to pod communication. This is done to ensure that when a pod is destroyed or fails, the service reroutes any of the requests to the other pods, while also ensuring that when the new pod is back, it is reachable again, through the service. In this case as well, we have 1 service for every defined deployment for the application, this means that we have a structure of 1 service -> 1 deployment -> n pod replicas.

Next, the application needed an entry point. This is where we made use of the ingress gateway, which allows us to access the frontend application through an HTTP protocol. The ingress gateway receives and redirects all http traffic from user requests to the requested/required service, which then handles the rest of the logic, such as service to service communications to ensure that the frontend can make a request to the machine learning model.

**Need to write about ConfigMaps, Secrets and ServiceAccounts as well as the namespace but i need to read about this a bit more and double check the project implementations**

## Istio Service Mesh Architecture

Istio works as an extension to our kubernetes setup providing more fine-grained control over traffic management. This is done through the 'Istio Service Mesh', which comes into existence through the addition of something known as a sidecar. These sidecars are either attached to the entire project, or in our case to each individual deployment that needs to use the istio service mesh.

Istio has a seperate entry point that will be used as a substitute to the ingress gateway, known as the Istio Ingress Gateway. We do this becasue it is a 'mesh-aware' entry point that allows the integration of the more fine-grained routing rules such as a 90/10 split canary release, or the implementation of sticky sessiosn.
These traffic management resources are defined in the VirtualService and the DestinationRule. This is done by providing subsets in the destination rule, that ensures that when the app receives a version, the model-service will use the same version. The choice of the individual version is decided in the VirtualService, in our case through a split of 3 decisions. Istio first checks for a header, namely whether we pass a 'stable' or 'canary' value to a specific header key, in which case we get redirected to the specific version that is defined in the same VirtualService. If there is no header, or no matching header, we fall back to the third decision rule, a 90/10 split. This redirect 90% of the requests to a subset of the stable versions of the release, and the remaining 10% to a subset of the canary release, which could contain experiments, new features or anything else the developers want some of the users to experience.

**Need to look into Rate limiting, Observations and Monitoring**
