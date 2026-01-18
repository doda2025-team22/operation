## Problem and Extension Solution

For Continuous Integration (CI) and Continuous Deployment (CD), the project is setup to version, package and release a mavent artifact package and in some cases a docker container based on one of two conditions a manual trigger by the developers, or a automatic trigger through a push that contains a change to the pom.xml.

From there, the current state of the project is to have a manual trigger of helm upgrade to attach these newly released versions to the running cluster, this method is both ineficient and can introduce various misalignments between developers. We can mitigate this and introduce a safer semi-automated deployment through the introduction of a gitops tool such as [ArgoCD](https://argo-cd.readthedocs.io/en/stable/). ArgoCD used the Git repository as the source of truth for defining the current application state. This implies that whenever a new stable release or canary release has been pushed to the git repository, ArgoCD sees this and notifies on the dashboard that the current deployment is 'OutOfSync'.

### ArgoCD

Introducing ArgoCD to the project should be a relatively streamlined process, by following either the documentation or this [medium article](https://medium.com/@veerababu.narni232/a-complete-overview-of-argocd-with-a-practical-example-f4a9a8488cf9). The tasks for this extension would be to implement, apply and observe the argoCD tool. Some specific steps include creating the namespace for argocd and installing the controlpane through the appropriate extensions, natually this should be done in the vagrant setup to make this an automated process in the future. Writing an appropriate manifest file that tracks both app and model-service will be the second hurdle for this task, however following this [medium source](https://medium.com/@ricardolam3/argocd-multiple-sources-application-16a799918da5) should get you quite far.

### Improvement

As mentioned above, the current project setup does not have a fully automated CI/CD pipeline. Furthermore, without any git notification, team members will often use different clusters and project versions, especially when the project scale and team size increases. By adding ArgoCD to the project, many of these issues will be circumvented through the highlighting of an inconsistent version with the currently deployed cluster and the git's source of truth.

## Experiment

Since ArgoCD in our case mainly functions as a dashboard/CD tool, there is no major experimentation that can be done to measure the resulting effects however, we can start the experimentation through developer observations. Both medium articles listed provide instructions on how to deploy argoCD as a visualCLI tool, therefore a simple measure of successful implementation is to see whether: (a) ArgoCD's visual CLI can be deployed and inspected. (b) the Kubernetes cluser is corectly visualised on the application dashboard.

Further experimentation can be conducted through a small benchmark test comparing a developers time from CI activation to sucessful version deployment compared to ArgoCD's speed. However ArgoCD is not necessarily meant for speed so this metric can only serve as a guideline on whether efficiency, when active, may increase or drop.

One of the other major components of ArgoCD is system health monitoring, this can be checked and tested by selectively disabling or pausing kubernetes components such as a pod or deployment. If done correctly, ArgoCD should pick up on the disabled pod and display it on the dashboard showing a correct connection to the main Kubernetes cluster. Naturally this can also be tested with the benchmark above, as the step involving argoCD will require an OutOfSync status to be shown telling us that the two remote repositories are correctly added through the manifest (if tested seperately).

## Reflection

Naturally this extension proposal is just a proposal and may not be fully correct in terms of implementation steps or possible benefits. Although the sources show that it should be possible correct implementation may be more challenging than expected especially since we are now using vagrant and ansible to run the commands shown in the medium sources which adds that seperate layer of complexity.
