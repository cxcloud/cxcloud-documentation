# Deploying Services

### Deployment Basics

CX Cloud makes it really easy for you to deploy any Docker-based service. You just need to follow a few simple steps to deploy a service:

* Make sure your project has a `Dockerfile` \(this is just an example\):

{% code-tabs %}
{% code-tabs-item title="Dockerfile" %}
```text
FROM nginx
COPY html /usr/share/nginx/html
EXPOSE 80
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* Create a file named `.cxcloud.yaml` in the root of your project:

{% code-tabs %}
{% code-tabs-item title=".cxcloud.yaml" %}
```yaml
deployment:
  name: my-frontend
  image:
    name: my-frontend-image
    repository: 307365680736.dkr.ecr.eu-west-1.amazonaws.com/cluster.cxcloud.com
    version: 1.1.1
  containerPort: 80
  replicas: 2
  env:
    - name: NODE_ENV
      value: production

```
{% endcode-tabs-item %}
{% endcode-tabs %}

* Run CX Cloud deployment tool:

```bash
$ cxcloud deploy
```

Your service will be deployed to your Kubernetes cluster.

### Deployment Manifest Options

The `.cxcloud.yaml` file has different configurable sections:

| Option | Description |
| :--- | :--- |
| deployment.name | The name of your service to be saved in Kubernetes \(can be anything\) |
| deployment.image.name | The name of your docker image \(can be anything\) |
| deployment.image.repository | Your AWS ECR Repository |
| deployment.image.version | A version to tag your docker image with |
| deployment.containerPort | Your Dockerfile's exposed port |
| deployment.replicas | Number of replicas to be made \(usually more than 1 just to be safe\) |
| env | A list of environment variables that will be attached to your deployment |

You can use Bash environment variables anywhere in this file. Just prefix a string with `$` to denote it:

```yaml
deployment:
  name: $MY_BASH_ENV
```

There are a few special environment variables that are available to be used:

| Environment Variable | Description |
| :--- | :--- |
| `$APP_NAME` | The `name` field of your `package.json` \(if available\) |
| `$APP_VERSION` | The `version` field of your `package.json` \(if available\) |

For example you can set those environment variables on runtime:

```text
$ MY_BASH_ENV=frontend cxcloud deploy
```

### Deployment Environment Variables

The `deployment.env` option in the deployment manifest can be used to pass environment variables to your docker container when it's deployed to your Kubernetes cluster. This can be useful for passing things like secrets, environment config, etc.

```yaml
deployment:
  # ...
  env:
    - name: NODE_ENV
      value: production
    - name: MY_SECRET_KEY
      value: $SECRET_BASH_ENV
```

You can pass environment variables also when running the `cxcloud deploy` command:

```text
$ cxcloud deploy -e MY_SECRET_KEY=something
```

You can pass multiple environment variables using commas.

### Routing

Now that you have deployed your service, you can make it available to the world by creating a routing manifest. Read more about that in it's section.

