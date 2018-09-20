# Generating Infrastructure

{% hint style="warning" %}
Windows is not supported. You can only generate the infrastructure using a Unix based OS. Eg. macOS or Linux.
{% endhint %}

{% hint style="info" %}
Make sure you have setup a domain to be used by this tool before continuing. [Read the docs](configuring-domains.md#configuring-a-domain-for-the-kubernetes-cluster) to find out how.
{% endhint %}

The first step of setting up a CXCloud project, is generating the infrastructure required for running services.

Make sure you have [installed all the requirements](../getting-started/prepare-your-environment.md) on your computer and have exported the proper AWS profile:

```bash
$ export AWS_PROFILE=my-cxcloud-dev
```

Then create a Github repository named `infra` in your organization, clone it, and run the following command:

```bash
$ git clone git@github.com:myorganization/infra.git
$ cd infra
$ cxcloud generate infra
```

After answering the questions the tool asks you, it will start bootstrapping your environment on AWS. It does the following steps automatically for you:

1. Creates an S3 bucket for storing settings and state
2. Generates Terraform files for creating container repositories and other required resources
3. Generates Kubernetes config files and creates the Kubernetes cluster
4. Waits for the Kubernetes cluster to become available
5. Configures access controls on the Kubernetes cluster
6. Installs Tiller, nginx-ingress and cert-manager on the cluster
7. Setups of staging and production LetsEncrypt issuers
8. Displays the load balancer URL

if the setup is successful, the command displays the load balancer URL which you need to use to point your domains to.

[More information on how to point a domain to this load balancer](configuring-domains.md#configuring-a-domain-for-your-service).

## Demo

{% embed data="{\"url\":\"https://www.youtube.com/watch?v=ADE\_TyaGLeo&feature=youtu.be\",\"type\":\"video\",\"title\":\"Generating Infrastructure using the CLI\",\"icon\":{\"type\":\"icon\",\"url\":\"https://www.youtube.com/yts/img/favicon\_144-vfliLAfaB.png\",\"width\":144,\"height\":144,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://i.ytimg.com/vi/ADE\_TyaGLeo/mqdefault.jpg\",\"width\":320,\"height\":180,\"aspectRatio\":0.5625},\"embed\":{\"type\":\"player\",\"url\":\"https://www.youtube.com/embed/ADE\_TyaGLeo?rel=0&showinfo=0\",\"html\":\"<div style=\\\"left: 0; width: 100%; height: 0; position: relative; padding-bottom: 56.2493%;\\\"><iframe src=\\\"https://www.youtube.com/embed/ADE\_TyaGLeo?rel=0&amp;showinfo=0\\\" style=\\\"border: 0; top: 0; left: 0; width: 100%; height: 100%; position: absolute;\\\" allowfullscreen scrolling=\\\"no\\\"></iframe></div>\",\"aspectRatio\":1.7778}}" %}

