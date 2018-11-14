# Manually Defining Routing

Now that you have created your Kubernetes infrastructure and deployed your services to it, you can make them public using a routing profile:

* First navigate to your `infra` folder and create a new directory called `routing`:

```bash
$ cd infra
$ mkdir routing
$ cd routing 
```

* Create a file named `cert.yaml` with the following content:

{% code-tabs %}
{% code-tabs-item title="cert.yaml" %}
```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: my-app-certificate
spec:
  secretName: my-app-certificate
  dnsNames:
  - newsite.example.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - newsite.example.com
  issuerRef:
    name: letsencrypt-production
    kind: ClusterIssuer

```
{% endcode-tabs-item %}
{% endcode-tabs %}

Be sure to replace `my-app-certificate` with a more appropriate name and `newsite.example.com` with a domain that you have configured to be used with your cluster.

* Now create a file named `routing.yaml` with the following content:

{% code-tabs %}
{% code-tabs-item title="routing.yaml" %}
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: my-app-routing
  namespace: applications
  annotations:
    kubernetes.io/ingress.class: "nginx"
    certmanager.k8s.io/cluster-issuer: letsencrypt-production
spec:
  tls:
  - hosts:
    - newsite.example.com
    secretName: my-app-certificate
  rules:
  - host: newsite.example.com
    http:
      paths:
      - path: /api/commerce
        backend:
          serviceName: service-commerce
          servicePort: 4003
      - path: /
        backend:
          serviceName: frontend
          servicePort: 80

```
{% endcode-tabs-item %}
{% endcode-tabs %}

Again, replace `my-app-certificate`, `my-app-routing` and `newsite.example.com` with your own values.

In the `spec.rules` section, you can specify routing information for each domain name that you want. In this example:

1. `newsite.example.com/` loads `frontend` service on port `80`
2. `newsite.example.com/api/commerce` loads `service-commerce` service on port `4003` 

* After you have done setting the values, run the the following command to apply your changes \(make sure you are still in `routing` directory\)

```text
$ kubectl apply -f ./
```

Your domain will be configured and after a few minutes an SSL certificate will be issued using The LetsEncrypt service.

