# DODA Group 22

## Repos
operation: https://github.com/doda2025-team22/operation/releases/tag/a3
app: https://github.com/doda2025-team22/app/releases/tag/a3
frontend: https://github.com/doda2025-team22/model-service/releases/tag/a3
lib: https://github.com/doda2025-team22/lib-version/releases/tag/a3

## Comments for A1
To our knowledge, we implemented everything from the assignment description.

## Comments for A2
Other than the last task with is optional for this deadline, everything has been implemented. We included an additional playbook called `master.yaml` to call the other playbooks on the `Vagrantfile`. This is only done for convinience and cleaner code for the `Vagrantfile` and does not add additional complexities. Some of the `ansible.builtin.command` runs are also without checks present, this will be fixed on the final submission.

## Comments for A3
Due to the lack of the monitoring and alert implementation, the grafana dashboards are also not implemented. The only thing done is to package the applicatiuon into a k8n compose file and do the deployment of the app with monitoring and ingress. 
