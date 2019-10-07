# Provision Infrastructure

Cx Cloud modules and deployments is developed for Kubernetes, hence, CX Cloud can run on any existing Kubernetes cluster.

The CX Cloud demo setup will install the infra with Kubernetes, running on AWS. When the setup is up and running it can be used for the [CX Cloud Demo app](https://github.com/cxcloud/demo-cxcloud-monorepo-angular) or any other CX Cloud related project.

## Getting stated with CX Cloud Demo

### Installation steps

There are few steps that has to be done in order to install the infrastructure. Follow the instructions from the following steps:

1) [Provision the infra](https://github.com/cxcloud/demo-cxcloud-k8s/blob/master/terraform/README.md).
2) [Install OpenVPN](https://github.com/cxcloud/demo-cxcloud-k8s/blob/master/openvpn-ansible/README.md) in order to access services in the private subnets on AWS.
3) [Install Kubernetes](https://github.com/cxcloud/demo-cxcloud-k8s/blob/master/kubernetes/README.md).

### Configuring a domain for your online service

After you have generated the Kubernetes infrastructure, you would want to assign a domain to your future online service \(or services\)

The assumed starting state is that you have access to manage "example.com" DNS. CX Cloud is to be configured to utilize this domain to explore its possibilities. As there might already be a site running in www.example.com, the target state with CX Cloud would look like this:

* `newsite.example.com` \(customer front-end\)
* `newsite.example.com/api/service-commerce/v1/` \(commerce service API\)
* `newsite.example.com/api/service-content/v1/` \(content service API\)
* `newsite.example.com/api/service-search/v1/` \(search service API\)
* `newsite.example.com/api/service-auth/v1/` \(auth service API\)
* `*.dev.newsite.example.com` \(Pull Request environments\)

We have used this assumption in our documentation and configuration examples. Most of these configurations are done with a routing manifest later and at this stage we only need to point the domain to Kubernetes load balancer like this:

* First figure out your Load Balancer\(s\) URL. The URL\(s\) are displayed after generating the infrastructure. But you can also find them using:

```text
kubectl get ingress --all-namespaces
```

* Then you have to point your own domain name to this load balancer URL. To do this:
  * If you manage your domain using AWS' route53 service, update the settings in [terraform.tfvars](example_vars/terraform.tfvars) and re-run the `terraform apply` command in order to update the domain name aliases. Follow the instructions from [Install Kubernetes](https://github.com/cxcloud/demo-cxcloud-k8s/blob/master/kubernetes/README.md).
  * If you manage your domain using a provider that's not AWS, navigate to your domain's management panel and create an `CNAME` record for `newsite.example.com` pointing to the load balancer URL.
    * [GoDaddy guide](https://fi.godaddy.com/help/add-a-cname-record-19236)
    * [Namecheap guide](https://www.namecheap.com/support/knowledgebase/article.aspx/9646/2237/how-can-i-set-up-a-cname-record-for-my-domain)
    * [Name.com guide](https://www.name.com/support/articles/115004895548-Adding-a-CNAME-Record)
