## Usage
The helm chart uses the default ingress provider. To be able to access the apps without setting hosts yourself use sslip.io. In the example below we are using `nginx` with it pointing to ip `192.168.56.90`. Thus in this situation the global domain becomes `192-168-56-90.sslip.io`. Adjust it during deployment to fit your use case.

1. Install dependencies:
```bash
helm dependency build ./team22_chart
```

2. Install the helm chart
```bash
helm upgrade --install team22 ./team22_chart \
  --namespace team22 \
  --create-namespace \
  --set global.domain=192-168-56-90.sslip.io \
  --set global.stableSubdomain=team22 \
  --set global.prereleaseSubdomain=team22-dev \
  --set monitoring.enable=true
```

This will allow you to access the services with the URL's:
- http://team22.192-168-56-90.sslip.io
- http://team22-dev.192-168-56-90.sslip.io
- http://prometheus.192-168-56-90.sslip.io
- http://prometheus.192-168-56-90.sslip.io

## Tips for testing with Minikube
- If you are using a Minikube cluster and having problems, the following points might be helpful:
- Make sure ingress addon is enabled in your cluster.
- Install istio in your cluster using ``` istioctl install --set profile=demo -y```
- If you are having connectivity issues, consider adding the following lines to your hosts file:
```127.0.0.1 team22.192-168-56-90.sslip.io```
```127.0.0.1 team22-dev.192-168-56-90.sslip.io```
- Do not forget to ``sudo minkube tunnel`` before accessing the app through the links.
- If you see the error ``no stable upstream branch``, please wait a bit or refresh.

## For testing with Linux / In case of other errors
First run ```minikube service list```.
In the output, if you receive urls for istio-system, copy the port number of the url associated with http/80 (the target port). 

Use the link http://team22.192-168-56-90.sslip.io:{port number you just got}/sms/ or http://team22.192-168-56-90.sslip.io:{port number you just got}/sms/library-version.