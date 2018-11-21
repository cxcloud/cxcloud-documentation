# Routing Manifest

Routing manifest is needed to make multiple services public on the same domain.

For example, let's assume you have created a front-end and 3 microservices and you want them all to be available on the same domain:

* `newsite.example.com/` should load `frontend` service
* `newsite.example.com/api/commerce/` should load `service-commerce` service
* `newsite.example.com/api/content` should load `service-content` service
* `newsite.example.com/api/search` should load `service-search` service

You have to create a routing manifest to achieve this. \(In addition to pointing the domain in question to your Kubernetes cluster during [infra generation](generating-infrastructure.md#configuring-a-domain-for-your-online-service)\)

## Create manifest

In your `infra` directory, create a directory named `routing` and place a file called `.cxcloud.yaml` inside it:

{% code-tabs %}
{% code-tabs-item title="infra/routing/.cxcloud.yaml" %}
```yaml
routing:
  domain: newsite.example.com
  ssl: true
  rules:
    - path: /api/commerce
      serviceName: service-commerce
      servicePort: 4003
    - path: /api/content
      serviceName: service-content
      servicePort: 4003
    - path: /api/search
      serviceName: service-search
      servicePort: 4003
    - path: /
      serviceName: frontend
      servicePort: 80
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Please note that `path` should match the API prefix you defined when generating the service with CLI.

## Deployment

Now navigate to the `routing` directory and run `cxcloud deploy`:

```bash
$ cd infra/routing
$ cxcloud deploy
```

This command will "deploy" your routing manifest to your Kubernetes cluster. It shoudn't take more than a few seconds for the changes to be applied. 

## Test

To test for example commerce service,  `newsite.example.com/api/commerce/` should return {"health":"OK"} and `newsite.example.com/api/commerce/v1/api-docs` should present you Swagger. 

{% hint style="info" %}
If you are interested to know what does this command do, please read the [manual routing setup guide](../guides/manually-defining-routing.md).
{% endhint %}



