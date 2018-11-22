# Storing Secrets in Kubernetes

Kubernetes has the capability to store secrets and make them available to your services. You can store different secrets and use them through your CX Cloud deployment environment variables. To store a secret in `applications` namespace \(it's the default namespace for CX Cloud apps\) run the following command:

```bash
$ kubectl create secret generic prod-db-secret \
    --from-literal=username=produser \
    --from-literal=password=Y4nys7f11 \
    --namespace=applications
```

{% hint style="info" %}
You have to specify which namespace you want your secret to be created in, because only deployments in that namespace will be able to access the secrets.
{% endhint %}

Now when you create a deployment, you can reference your secret:

{% code-tabs %}
{% code-tabs-item title=".cxcloud.yaml" %}
```yaml
deployment:
  name: my-backend
  image:
    name: my-backend-image
    repository: YOUR_AWS_ECR_REPOSITORY_URL_HERE
    version: 1.1.1
  containerPort: 8080
  replicas: 2
  env:
    - name: NODE_ENV
      value: production
    - name: PROD_DB_USERNAME
      valueFrom:
        secretKeyRef:
          name: prod-db-secret
          key: username
    - name: PROD_DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: prod-db-secret
          key: password
```
{% endcode-tabs-item %}
{% endcode-tabs %}

When the service is deployed, your secrets will become available under the environment variables you have specified. For example in NodeJS, you can access them like so:

```javascript
console.log(process.env.PROD_DB_USERNAME); // produser
console.log(process.env.PROD_DB_PASSWORD); // Y4nys7f11
```

For more information about Kubernetes secrets, [visit this article](https://kubernetes.io/docs/concepts/configuration/secret/).

#### Example: Storing and using secrets in NodeJS and \`node-config\` module

One of the services that you can generate using the CX Cloud CLI is **CommerceTools** service, which requires some configurations \(like API key, etc\). We can take advantage of Kubernetes secrets explained above to store these information without having to publish them to GitHub.

First, generate a service using the CLI and choose CommerceTools, [as explained in it's section](../setting-up-a-cxcloud-project/generating-core-services.md).

Then, according to `node-config`'s [documentation](https://github.com/lorenwest/node-config/wiki/Environment-Variables#custom-environment-variables), create a file named `custom-environment-variables.json` in the `config` folder of the generated service with the following content:

```javascript
{
  "commerceTools": {
    "projectKey": "COMMERCETOOLS_PROJECT_KEY",
    "admin": {
      "clientId": "COMMERCETOOLS_ADMIN_CLIENT_ID",
      "clientSecret": "COMMERCETOOLS_ADMIN_CLIENT_SECRET"
    },
    "user": {
      "clientId": "COMMERCETOOLS_USER_CLIENT_ID",
      "clientSecret": "COMMERCETOOLS_USER_CLIENT_SECRET"
    }
  }
}
```

This file will tell `node-config` to look for those environment variables and map them to certain keys. For example the configuration key `commerceTools.admin.clientId` will map to `COMMRCETOOLS_ADMIN_CLIENT_ID` and so on. 

Now we have to store those data in Kubernetes and make them available to our service using the specified environment variables. To do that, first let's create a secret in `applications` namespace:

```bash
$ kubectl create secret generic prod-commercetools \
    --from-literal=projectKey=xxxxxxx \
    --from-literal=adminClientId=xxxxxxx \
    --from-literal=adminClientSecret=xxxxxxx \
    --from-literal=userClientId=xxxxxxx \
    --from-literal=userClientSecret=xxxxxxx \
    --namespace=applications
```

Replace the `xxxxxxx` above with your actual data. A secret will be created.

Now modify your `.cxcloud.yaml` file and add the proper environment variables referencing the secret you just created \(new values are added from line 12\):

{% code-tabs %}
{% code-tabs-item title=".cxcloud.yaml" %}
```yaml
deployment:
  name: $APP_NAME
  image:
    name: $APP_NAME
    repository: xxxxxx.dkr.ecr.eu-west-1.amazonaws.com/newsite.example.com
    version: $APP_VERSION
  containerPort: 4003
  replicas: 2
  env:
    - name: NODE_ENV
      value: production
    - name: COMMERCETOOLS_PROJECT_KEY
      valueFrom:
        secretKeyRef:
          name: prod-commercetools
          key: projectKey
    - name: COMMERCETOOLS_ADMIN_CLIENT_ID
      valueFrom:
        secretKeyRef:
          name: prod-commercetools
          key: adminClientId
    - name: COMMERCETOOLS_ADMIN_CLIENT_SECRET
      valueFrom:
        secretKeyRef:
          name: prod-commercetools
          key: adminClientSecret
    - name: COMMERCETOOLS_USER_CLIENT_ID
      valueFrom:
        secretKeyRef:
          name: prod-commercetools
          key: userClientId
    - name: COMMERCETOOLS_USER_CLIENT_SECRET
      valueFrom:
        secretKeyRef:
          name: prod-commercetools
          key: userClientSecret

```
{% endcode-tabs-item %}
{% endcode-tabs %}

When done, increase the version in your `package.json` file and run:

```bash
$ cxcloud deploy
```



