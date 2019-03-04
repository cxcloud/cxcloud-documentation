# Deploying Custom Services

## Deployment Basics

In addition to Core Services, CX Cloud makes it really easy for you to deploy any Docker-based service. You just need to follow a few simple steps to deploy a service:

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
    repository: YOUR_AWS_ECR_REPOSITORY_URL_HERE
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

## Deployment Manifest Options

The `.cxcloud.yaml` file has different configurable sections:

| Option | Description |
| :--- | :--- |
| `namespace` | Namespace to store deployment resources |
| `deployment.name` | The name of your service to be saved in Kubernetes |
| `deployment.image.name` | The name of your docker image |
| `deployment.image.repository` | Your AWS ECR Repository |
| `deployment.image.version` | A version to tag your docker image with |
| `deployment.containerPort` | Your Dockerfile's exposed port |
| `deployment.port` | If omitted, it will be the same as `containerPort` but if provided it will change the service's inbound port |
| `deployment.replicas` | Number of replicas to be made \(usually more than 1 just to be safe\) |
| `deployment.routing.domain` | Standalone domain pointing to the current deployment |
| `deployment.routing.path` | Which path on the domain should deployment respond to |
| `deployment.routing.ssl` | Enable SSL for the domain |
| `deployment.env` | A list of environment variables that will be attached to your deployment |

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
| `$GIT_COMMIT_ID` | Full Commit ID of latest commit. eg. `0fa68a9c4a5452567524db6da8e9304e3d7512c0` |
| `$GIT_SHORT_COMMIT_ID` | Short Commit ID of latest commit. eg. `0fa68a9` |
| `$GIT_BRANCH` | Current Git branch. eg. `master` |

For example you can set those environment variables on runtime:

```text
$ MY_BASH_ENV=frontend cxcloud deploy
```

## Deployment Environment Variables

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

## Routing

#### Routing for one service only

If you need to assign a domain to the current deployment, you can do so in the deployment manifest under the `deployment` key:

{% code-tabs %}
{% code-tabs-item title=".cxcloud.yaml" %}
```yaml
deployment:
  # ...
  routing:
    domain: service.example.com
    path: /
    ssl: false
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
You have to make sure that the `service.example.com` \(or your domain\) is pointed to your load balancer. [You can read how to do so here](../setting-up-a-cxcloud-project/generating-infrastructure.md#configuring-a-domain-for-your-online-service).
{% endhint %}

#### Routing for multiple services

[Read about routing to multiple services in it's section](../setting-up-a-cxcloud-project/routing-manifest.md).

