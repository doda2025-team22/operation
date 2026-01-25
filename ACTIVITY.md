# Contributions

### Week 1 (Nov 10-16)

- Eduardo: Attended to lectures and went to the practical on thursday where tasks were assigned.
- Gonenc: Attended to lectures and went to the practical on thursday where tasks were assigned.
- Ada: Attended lectures and the lab.
- Emre: Addended the lab.
- Thomas: Attended the lectures and lab.

### Week 2 (Nov 17-23)

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

### Week 3 (Nov 24-30)

- Gonenc:
  Worked on all steps of 1.1, which the entire team did as well. The final PR can can be found as https://github.com/doda2025-team22/operation/pull/26 and my individual branch can be found as https://github.com/doda2025-team22/operation/tree/gt/1.1; also worked on step 20: https://github.com/doda2025-team22/operation/pull/33

- Ada:
  Also worked on all steps of 1.1, along with the entire team. The branch with my individual process can eb found in: https://github.com/doda2025-team22/lib-version/tree/at/provisioning, the changes were moved to operation later. The final PR can be found in: https://github.com/doda2025-team22/operation/pull/26. Additionally, I made some alterations to the base playbooks which can be found in: https://github.com/doda2025-team22/operation/pull/28 , https://github.com/doda2025-team22/operation/pull/30 , and https://github.com/doda2025-team22/operation/pull/29. I additionally worked on step 22 on A2, which can be found in: https://github.com/doda2025-team22/operation/pull/32

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

## Week 4 (Dec 1-7)

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

## Week 5 (Dec 8-14)

- Thomas: I started working on some local provisioning that could help speed up the vagrant provisioning. I also updated the readme in that time. It still needs some work but I made an intermediate PR for it. I also worked on the sticky sessions, they currently only work with curl and you will be defaultet to a 90/10 split on the browser or without any correct heading.

  - https://github.com/doda2025-team22/operation/pull/50
  - https://github.com/doda2025-team22/operation/pull/56

- Ada: This week I worked on the monitoring that was missing from last week. It can be found here: https://github.com/doda2025-team22/app/pull/13 . Then on the lab we worked on some troubleshooting steps for Istio, which resulted in us adding it to the documentation here:https://github.com/doda2025-team22/operation/pull/51 . I also had to adjust paths used for the metrics here: https://github.com/doda2025-team22/operation/pull/48 .

- Gonenc: This week I worked on implementing the traffic management up until sticky management. The pr for this can be found here: https://github.com/doda2025-team22/operation/pull/49

- Emre: Worked on the helm charts but nothing significant was contributed to the chart.

## Week 6 (Dec 15-21)

- Ada: Added rate limiting for the additional use case section for A4. Also made adjusted the trigger for releases on the app to make sure it is not accidentally triggered.

  - https://github.com/doda2025-team22/app/pull/15
  - https://github.com/doda2025-team22/operation/pull/59

- Emre: Worked on teh helm chart and added the dashboard and the ingress for grafana. Also fixed a bug related to the prometheus metrics, now the metrics by default are easly discoverable by the grafana dashboard. Also added the documentation for the helm chart, explaining how to use it and what are the configurable values.
  - https://github.com/doda2025-team22/operation/pull/58
  - https://github.com/doda2025-team22/operation/pull/62

- Gonenc: Worked on implementing adding a canary version for the backend in addition to the frontend:
  - https://github.com/doda2025-team22/operation/pull/60
  - Improved upon by Emre in another branch/pr
  
- Thomas: Worked on the sticky session branch agin, becasue I merged late and ended up with a bunch of conflicts. I also started working on the Report that is part of assignment 4, however that is not in a PR just yet.
  - https://github.com/doda2025-team22/operation/pull/56

## Week 7 (Jan 5-11)

- Emre: This is the activity for holiday time contrib + week 7. Worked on A1 assignment polishing. Fixed the lib-version release workflow to be now fully standartised and for the other repos make them fully tag indipendent. Also for the app repo, added a version verifyer that checks if the changed `pom.xml` is actually has a new version of the application before building the jar and the container. Also added the old fixes Gonenc was working on after they became outdated due to breaking changes to the helm chart (Holiday time contrib). Nearly finished up the experimentation. Made some changes to app related to its CI deployment. Now the new version building gets trigered when the app version is updated from SNAPSHOT to normal release number using mvn. Also fixed an bug present inside the CI. Additionaly added sticky sessions based on the work done by Thomas as after some changes to the helm package a rewrite was needed. Added cookies to ensure version served to the user between the stable and the canary release does not change. 
  - https://github.com/doda2025-team22/lib-version/pull/7
  - https://github.com/doda2025-team22/app/pull/16
  - https://github.com/doda2025-team22/app/pull/17
  - https://github.com/doda2025-team22/app/pull/18
  - https://github.com/doda2025-team22/model-service/pull/15
  - https://github.com/doda2025-team22/model-service/pull/16
  - https://github.com/doda2025-team22/operation/pull/70
  - https://github.com/doda2025-team22/operation/pull/64
  - https://github.com/doda2025-team22/operation/pull/65
  - https://github.com/doda2025-team22/app/pull/23
  - https://github.com/doda2025-team22/app/pull/24
  - https://github.com/doda2025-team22/app/pull/25

- Ada: Worked on polishing the ratelimiting, some things had to be adjusted with improvements made to the project. I also configured the alertmanager and established the smtp server connection via using secret and also makig a project email.
  - https://github.com/doda2025-team22/operation/pull/66

- Thomas: This week I spent more time on working on the project documentation, finishing a second draft with the general abstract overview of the Kubernetes and Istio setup. The I also spent some time running the clusters to try and access the dashboards to get some figures and better insight however with little luck. No worthwhile contribution for a PR yet, so i added the main commit.
  - https://github.com/doda2025-team22/operation/commit/69583e7bf0f3a63a28b35df8b4f46353fbc80fe8

- Gonenc: Solved issues/bugs in order to allow deploying the application into custom provisioned cluster:
    - https://github.com/doda2025-team22/operation/pull/69
    - The coding changes can be observed in the commit below:
      - https://github.com/doda2025-team22/operation/pull/69/commits/5936fec625c5855999d1f8f243128afe05583ce9
    - These changes were improved upon by Emre in another branch/pr.

## Week 8 (Jan 12-18)

  - Emre: Did the final fixes for A2.
    - https://github.com/doda2025-team22/operation/pull/73

  - Gonenc: Graded assignment 3, and worked on meeting all the requirements for Good on Kubernetes Usage:
    - https://github.com/doda2025-team22/operation/pull/77
    - https://github.com/doda2025-team22/operation/pull/80

  - Ada: THis week, I finished the alerting, making them rely on the metrics we have exposed before. Also, I polished A4, making sure there is a central source of documentation.
    - https://github.com/doda2025-team22/operation/pull/78
    - https://github.com/doda2025-team22/operation/pull/76

  - Thomas: Worked on f11 of a1, specifically creating the branch releases. Then I moved on to writing up the documentation for the extension.md followed by fixing the rest of the deployment documentation although i noticed that it still needs more work.
    - https://github.com/doda2025-team22/operation/pull/83
    - https://github.com/doda2025-team22/lib-version/pull/8


## Week 9 (Jan 19-25)

  - Emre: Some minor tweaks to a1 and a2 and also added .env file for maximum points for a1. Vagrant file also made a bit better
    - https://github.com/doda2025-team22/operation/pull/91

  - Thomas: This week I worked on my part of the presentation slides. I also worked on updating a1 to work with the configured ports, and match up the grading standards.
      - https://github.com/doda2025-team22/model-service/pull/18
      - https://github.com/doda2025-team22/operation/pull/90

  - Gonenc: This week, I worked on improving my previous work from https://github.com/doda2025-team22/operation/pull/80 as it fulfilled all requirements for Good, but not Excellent. Finalized A3/Kubernetes Usage to (hopefully) have met all points to get Excellent:
      - https://github.com/doda2025-team22/operation/pull/93
  
## Week 10 (Jan 26-27)
