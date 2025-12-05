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

This will allow you to acces the services with the URL's:
- http://team22.192-168-56-90.sslip.io
- http://team22-dev.192-168-56-90.sslip.io
- http://prometheus.192-168-56-90.sslip.io
- http://prometheus.192-168-56-90.sslip.io


