# Configuring Routing Manifest

Routing manifest is needed to make multiple services available in the same domain.

For example, let's assume you have created a front-end and 3 microservices and you want them all to be available on the same domain:

* `newsite.example.com/` should load `frontend` service
* `newsite.example.com/api/service-commerce/` should load `package-commerce` service
* `newsite.example.com/api/service-content` should load `package-content` service
* `newsite.example.com/api/service-search` should load `package-search` service

You have to create a routing manifest to achieve this. \(In addition to pointing the domain in question to your Kubernetes cluster during [infra generation](https://github.com/cxcloud/cxcloud-documentation/tree/7585ecd6d3f3a8c408f0919987af56c53decff01/setting-up-a-cxcloud-project/generating-infrastructure.md#configuring-a-domain-for-your-online-service)\)

## Create manifest

Add `.cxcloud.yaml` to the root folder of your monorepo.

{% code-tabs %}
{% code-tabs-item title="my-monorepo/.cxcloud.yaml" %}
```yaml
namespace: $GIT_BRANCH
routing:
  domain: $GIT_BRANCH.dev.newsite.example.com
  ingressClass: $INGRESS_CLASS
  lbCert: $LB_CERT
  scheme: $SCHEME
  ssl: false
  rules:
    - path: /api/service-commerce
      serviceName: package-commerce
      servicePort: 4003
    - path: /api/service-content
      serviceName: package-content
      servicePort: 4003
    - path: /api/service-search
      serviceName: package-search
      servicePort: 4003
    - path: /
      serviceName: package-frontend
      servicePort: 80
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Please note that `path` should match the API prefix you defined when generating the service with CLI. By default, during service generation, CX Cloud CLI suggests service name as the API prefix.

After committing changes to Git repository, CI/CD pipeline will deploy changes automatically.

## Test

To test for example commerce service, `newsite.example.com/api/service-commerce/` should return {"health":"OK"} and `newsite.example.com/api/service-commerce/v1/api-docs` should present to you Swagger documentation.

