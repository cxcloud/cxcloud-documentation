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

{% hint style="info" %}
CX Cloud CLI will by default set temporary AWS API keys as environment variables. However, it is also possible to run CLI with preset API keys as environment variables. This is useful for example when running [AWS Vault](https://github.com/99designs/aws-vault) for storing the secrets in your operating system's secure keystore. The `AWS_PROFILE` environment variable doesn't need to be exported in case AWS Vault is used.
{% endhint %}

{% hint style="success" %}
The CLI tool will ask you what domain you want to set as your cluster's domain. For most cases, you can use a "gossip-based" cluster configuration in which case you don't need an actual domain for your cluster. So enter the domain using the following format:

cluster-name**.k8s.local**

For example, you might want to name your cluster `mygreatcluster` then enter the domain name `mygreatcluster.k8s.local`

\(If you actually want to set a real domain for your cluster, you have to follow [this guide](../guides/domains-for-kubernetes.md) to delegate your domain to AWS's Route53 service and then use it.\)
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

## Configuring a domain for your online service   <a id="configuring-a-domain-for-your-online-service"></a>

After you have generated the Kubernetes infrastructure, you would want to assign a domain to your future online service \(or services\)

The assumed starting state is that you have access to manage "example.com" DNS. CX Cloud is to be configured to utilize this domain to explore its possibilities. As there might already be a site running in www.example.com, the target state with CX Cloud would look like this:

* `newsite.example.com` \(customer front-end\)
* `newsite.example.com/api/service-commerce/v1/` \(commerce service API\)
* `newsite.example.com/api/service-content/v1/` \(content service API\)
* `newsite.example.com/api/service-search/v1/` \(search service API\)
* `newsite.example.com/api/service-auth/v1/` \(auth service API\)
* `admin.newsite.example.com` \("admin panel"\)
* `developer.newsite.examle.com` \(developer portal\)

We have used this assumption in our documentation and configuration examples. Most of these configurations are done with a routing manifest later and at this stage we only need to point the domain to Kubernetes load balancer like this:

* First figure out what your Load Balancer URL is. The URL is displayed after generating the infrastructure. But you can also find it using:

```text
kubectl get service nginx-ingress-controller -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}'
```

* Then you have to point your own domain name to this load balancer URL. To do this:
  * If you manage your domain using a provider that's not AWS, navigate to your domain's management panel and create an `CNAME` record for `newsite.example.com` pointing to the load balancer URL.
    * [GoDaddy guide](https://fi.godaddy.com/help/add-a-cname-record-19236)
    * [Namecheap guide](https://www.namecheap.com/support/knowledgebase/article.aspx/9646/2237/how-can-i-set-up-a-cname-record-for-my-domain)
    * [Name.com guide](https://www.name.com/support/articles/115004895548-Adding-a-CNAME-Record)
  * If you manage your domain using AWS's route53 service, create an `ALIAS` record for `newsite.example.com` in Route53 pointing to that hostname.
    * More information on how to create an `ALIAS` record in Route53 on AWS [here](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-elb-load-balancer.html)​

If you want all the subdomains under a certain domain or subdomain to be available to you later during service creation, you can also create a `CNAME` or `ALIAS` record for `*.newsite.example.com`

## Commit

As a final step, commit changes to your Github repository by running the following commands:

```bash
$ git add .
$ git commit -m "initial "
$ git push
```

## Demo

{% embed url="https://www.youtube.com/watch?v=ADE\_TyaGLeo&feature=youtu.be" caption="" %}

