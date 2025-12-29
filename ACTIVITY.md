# Contributions

### Week 1

- Eduardo: Attended to lectures and went to the practical on thursday where tasks were assigned.
- Gonenc: Attended to lectures and went to the practical on thursday where tasks were assigned.
- Ada: Attended lectures and the lab.
- Emre: Addended the lab.
- Thomas: Attended the lectures and lab.

### Week 2

- Eduardo:
  Worked on task F8 for github actions https://github.com/doda2025-team22/model-service/pull/3 and https://github.com/doda2025-team22/app/pull/5
  Worked on task F7 for docker-compose https://github.com/doda2025-team22/operation/pull/13 and https://github.com/doda2025-team22/operation/pull/16

- Gonenc:
  Worked on task F9 for github actions/workflow and F10 for non-hardcoded models https://github.com/doda2025-team22/model-service/pull/5 and https://github.com/doda2025-team22/model-service/pull/7

- Ada:
  Worked on F1, creating a version-aware library that uses a resource file for keeping track of the version and then adding functionality to the app to use that library and also use it as a dependnecy.
  Creating the library: https://github.com/doda2025-team22/lib-version/pull/3
  Adding dependency + endpoint: https://github.com/doda2025-team22/app/pull/6

- Emre:
  Created the Github Organisation and setup the repositories. Created the dockerfiles for app and model-service repositories and helped Eduardo with the Github actions to build the containers. Also fixed some bugs related to Gonenc's work in the volume mounting.

  - https://github.com/doda2025-team22/model-service/pull/1
  - https://github.com/doda2025-team22/model-service/pull/4
  - https://github.com/doda2025-team22/model-service/pull/9
  - https://github.com/doda2025-team22/model-service/pull/10
  - https://github.com/doda2025-team22/app/pull/3
  - https://github.com/doda2025-team22/app/pull/9
  - https://github.com/doda2025-team22/app/pull/7

- Thomas:
  Worked on lib-version release workflows. Created three seperate workflows corresponding with the assignments F2 and F11
  - https://github.com/doda2025-team22/lib-version/pull/3
  - https://github.com/doda2025-team22/lib-version/pull/4
  - https://github.com/doda2025-team22/lib-version/pull/5

### Week 3

- Gonenc:
  Worked on all steps of 1.1, which the entire team did as well. The final PR can can be found as https://github.com/doda2025-team22/operation/pull/26 and my individual branch can be found as https://github.com/doda2025-team22/operation/tree/gt/1.1; also worked on step 20: https://github.com/doda2025-team22/operation/pull/33

- Ada:
  Also worked on all steps of 1.1, along with the entire team. The branch with my individual process can eb found in: https://github.com/doda2025-team22/lib-version/tree/at/provisioning, the changes were moved to operation later. The final PR can be found in: https://github.com/doda2025-team22/operation/pull/26. Additionally, I made some alterations to the base playbooks which can be found in: https://github.com/doda2025-team22/operation/pull/28 , https://github.com/doda2025-team22/operation/pull/30 ,  and https://github.com/doda2025-team22/operation/pull/29. I additionally worked on step 22 on A2, which can be found in: https://github.com/doda2025-team22/operation/pull/32

- Emre:
  Worked on 1.2 and 1.3 for A2 and integrated Gonenc's 1.1 for the rest of the assingments. Also helped Gonenc do Metallb, Ada with nginx ingress and implemented k8n dashboard. For last weeks assingment changed the repos app and model-service to make them version tag indipendent and also implemented some fixes to make it a higher grade version. Also fixed an issue related to versioning of app jar file.
  - https://github.com/doda2025-team22/operation/pull/36
  - https://github.com/doda2025-team22/operation/pull/26
  - https://github.com/doda2025-team22/app/pull/12
  - https://github.com/doda2025-team22/app/pull/11
  - https://github.com/doda2025-team22/app/pull/10
  - https://github.com/doda2025-team22/model-service/pull/12
  - https://github.com/doda2025-team22/model-service/pull/11

  Thomas: 
    I worked on steps 1.1 alongside the team. I started work on steps 22 and 23 but encountered system errors preventing me from finishing them.
    - https://github.com/doda2025-team22/operation/tree/ftb/a2-1.1


## Week 4

- Thomas: 
    This week i was tasked with the migration of the docker compose files to a kubernetes setup. I also took the time to work on an error that was still present in A1. 
    - https://github.com/doda2025-team22/operation/pull/38 
    - https://github.com/doda2025-team22/lib-version/pull/6 

- Gonenc:
  Worked on creating a helm chart and then putting the kubernetes migration into the helm chart, as well as implementing (minimal) ConfigMag and Secret playbooks:
  - https://github.com/doda2025-team22/operation/pull/39
  - https://github.com/doda2025-team22/operation/pull/40

- Emre:
  Worked limitedly on the grafana dashboard but due to lack of implementation of monitoring only deployed it to an ip adress. Also fixed an issue from the A2 where vagrantfile was incorrecly provisioning the nodes. Also added a basic version of istio to the k8n cluster. Also did networking for the ingress controller so it will use the clusters default instead of hardcoding a ingress.
  - https://github.com/doda2025-team22/operation/pull/44

- Ada:
  This week I spent a great deal of time getting the charts runnign specifically on my laptop and faced a lot of issues because of memory constraints. I also worked on the prometheus section of A3.
  - https://github.com/doda2025-team22/operation/pull/41


## Week 5 

- Thomas: I started working on some local provisioning that could help speed up the vagrant provisioning. I also updated the readme in that time. It still needs some work but I made an intermediate PR for it. I also worked on the sticky sessions, they currently only work with curl and you will be defaultet to a 90/10 split on the browser or without any correct heading.
  - https://github.com/doda2025-team22/operation/pull/50
  - https://github.com/doda2025-team22/operation/pull/56
 
- Ada: This week I worked on the monitoring that was missing from last week. It can be found here: https://github.com/doda2025-team22/app/pull/13 . Then on the lab we worked on some troubleshooting steps for Istio, which resulted in us adding it to the documentation here:https://github.com/doda2025-team22/operation/pull/51 . I also had to adjust paths used for the metrics here: https://github.com/doda2025-team22/operation/pull/48 .

- Gonenc: This week I worked on implementing the traffic management up until sticky management. The pr for this can be found here: https://github.com/doda2025-team22/operation/pull/49

- Emre: Worked on the helm charts but nothing significant was contributed to the chart.

## Week 6

- Ada: Added rate limiting for the additional use case section for A4. Also made adjusted the trigger for releases on the app to make sure it is not accidentally triggered.
  - https://github.com/doda2025-team22/app/pull/15
  - https://github.com/doda2025-team22/operation/pull/59

- Emre: Worked on teh helm chart and added the dashboard and the ingress for grafana. Also fixed a bug related to the prometheus metrics, now the metrics by default are easly discoverable by the grafana dashboard. Also added the documentation for the helm chart, explaining how to use it and what are the configurable values.
  - https://github.com/doda2025-team22/operation/pull/58

## Week 7

- Emre: Worked on streamlining and improving the A2 assignments. Most of the work went into streamlining the main deployment of the cluster and the vagrantfile. Also added some documentation to the readme.
  - https://github.com/doda2025-team22/operation/pull/62