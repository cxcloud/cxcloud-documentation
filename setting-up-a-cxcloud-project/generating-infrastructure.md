# Generating Infrastructure

{% hint style="warning" %}
Windows is not supported. You can only generate the infrastructure using a Unix based OS. Eg. macOS or Linux.
{% endhint %}

Make sure you have [installed all the requirements](../getting-started/prepare-your-environment.md) on your computer and have exported the proper AWS profile:

```bash
$ export AWS_PROFILE=my-cxcloud-dev
```

## Infra generation

The first step of setting up a CX Cloud project, is generating the infrastructure required for running services. Create a Github repository named `infra` in your organization. Then clone it to your project folder and start infra generation by running the following commands:

```bash
$ git clone git@github.com:myorganization/infra.git
$ cd infra
$ cxcloud generate infra
```

{% hint style="info" %}
If you have enabled Multi-Factor Authentication on your AWS account or you are assuming another role which does, you might be asked to enter your MFA token \(using your Authenticator app\) in this step.
{% endhint %}

After answering the questions the tool asks you, it will start bootstrapping your environment on AWS. It does the following steps automatically for you:

1. Creates an S3 bucket for storing settings and state
2. Generates Terraform files for creating container repositories and other required resources
3. Generates Kubernetes config files and creates the Kubernetes cluster
4. Waits for the Kubernetes cluster to become available
5. Configures access controls on the Kubernetes cluster
6. Installs Tiller, nginx-ingress and cert-manager on the cluster
7. Setups of staging and production LetsEncrypt issuers
8. Displays the load balancer URL

If the setup is successful, the command displays the load balancer URL which you need to use to point your domains to.

## Configuring a domain for your online service <a id="configuring-a-domain-for-your-online-service"></a>

After you have generated infra, you want to assign a domain to your future online service.  

The assumed starting state is that you have access to manage  "example.com" DNS. CX Cloud is to be configured to utilize this domain to explore its possibilities. As there might already be a site running in www.example.com, the target state with CX Cloud would look like this:

* newsite.example.com \(customer front-end\)
* newsite.example.com/api/commerce/v1/ \(commerce service API\)
* newsite.example.com/api/content/v1/ \(content service API\)
* newsite.example.com/api/search/v1/ \(search service API\)
* newsite.example.com/api/auth/v1/ \(auth service API\)
* admin.newsite.example.com \("admin panel"\)
* developer.newsite.examle.com \(developer portal\)

We have used this assumption in our documentation and configuration examples. Most of these configurations are done with a routing manifest later and at this stage we only need to point the domain to Kubernetes load balancer like this:

* Figure out what your Load Balancer URL is. The URL is displayed after generating the infrastructure. But you can also find it using:

```text
kubectl get service nginx-ingress-controller -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}'
```

* Now you have to create an `ALIAS` or `CNAME` record in Route53 pointing to that hostname.
  * More information on how to create an `ALIAS` record in Route53 on AWS [here](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-elb-load-balancer.html)​

## Commit

As a final step, commit changes to your Github repository by running the following commands:

```bash
$ git add .
$ git commit -m "initial "
$ git push
```

## Demo

{% embed url="https://www.youtube.com/watch?v=ADE\_TyaGLeo&feature=youtu.be" caption="" %}

