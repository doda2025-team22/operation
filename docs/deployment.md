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

Kubernetes uses another component called a Service to ensure pod to pod communication. This is done to ensure that when a pod is destroyed or fails, the service reroutes any of the requests to the other pods, while also ensuring that when the new pod is back, it is reachable again, through the service. In this case as well, we have 1 service for every defined deployment for the application, this means that we have a structure of 1 service -> 1 deployment -> n pod replicas.

Next, the application needed an entry point. This is where we made use of the ingress gateway, which allows us to access the frontend application through an HTTP protocol. The ingress gateway receives and redirects all http traffic from user requests to the requested/required service, which then handles the rest of the logic, such as service to service communications to ensure that the frontend can make a request to the machine learning model. However, as bare-metal and virtaul Machines do not have a reachable external IP's. Making use of MetalLB, by providing a pool of ip's we give all of these gateways and LoadBalancers external access.

Kubernetes also provides two key-value-based configuration resources: ConfigMaps and Secrets. These resources allow non-sensitive configuration values and sensitive data (such as credentials or tokens) to be managed seperately from applications code. ConfigMaps and Secrets can be shared across multiple Deployments, providing a centralized and consistent way to configure application behavior and improving the overall maintainability of the deployment.

To support logical seperation and operational clarity, the cluster is organized using Kubernetes namespaces. Namespaces introduce logical boundaries within the cluster alloweing resources to be grouped by porpose and enabling naming reuse across different parts of the system. Namespaces however do not enforce access restrictions by default. Access control is implemented in Kubernetes Role-Based Access Control (RBAC) in combination with ServiceAccounts. RBAC defines which actions are permitted within a namespace or across the cluster, while ServiceAccounts represent identities used by applications or operational tools when interacting with the Kubernetes API. This mechanism enables fine-grained permission management and limits access to sensitive cluster resources.

## Istio Service Mesh Architecture

Istio works as an extension to our kubernetes setup providing more fine-grained control over traffic management. This is done through the 'Istio Service Mesh', which comes into existence through the addition of something known as a sidecar. These sidecars are either attached to the entire project, or in our case to each individual deployment that needs to use the istio service mesh.

Istio has a seperate entry point that will be used as a substitute to the ingress gateway, known as the Istio Ingress Gateway. We do this becasue it is a 'mesh-aware' entry point that allows the integration of the more fine-grained routing rules such as a 90/10 split canary release, or the implementation of sticky sessiosn.
These traffic management resources are defined in the VirtualService and the DestinationRule. This is done by providing subsets in the destination rule, that ensures that when the app receives a version, the model-service will use the same version. The choice of the individual version is decided in the VirtualService, in our case through a split of 3 decisions. Istio first checks for a header, namely whether we pass a 'stable' or 'canary' value to a specific header key, in which case we get redirected to the specific version that is defined in the same VirtualService. If there is no header, or no matching header, we fall back to the third decision rule, a 90/10 split. This redirect 90% of the requests to a subset of the stable versions of the release, and the remaining 10% to a subset of the canary release, which could contain experiments, new features or anything else the developers want some of the users to experience.
