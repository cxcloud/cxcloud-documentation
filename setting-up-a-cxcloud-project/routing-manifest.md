# Routing Manifest

Routing manifest is needed to make multiple services public on the same domain.

For example, let's assume you have created 3 services and you want them all to be available on the same domain:

* `newsite.example.com/` should load `frontend` service
* `newsite.example.com/api/commerce/` should load `service-commerce` service
* `newsite.example.com/api/content` should load `service-content` service

You have to create a routing manifest to achieve this.

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
    - path: /
      serviceName: frontend
      servicePort: 80
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Please note that `path` should match the path you defined when generating the service with CLI.   

Now navigate to the `routing` directory and run `cxcloud deploy`:

```bash
$ cd infra/routing
$ cxcloud deploy
```

This command will "Deploy" your routing manifest to your Kubernetes cluster.

{% hint style="info" %}
If you are interested to know what does this command do, please read the [manual routing setup guide](../guidelines-for-custom-services/manually-defining-routing.md).
{% endhint %}

