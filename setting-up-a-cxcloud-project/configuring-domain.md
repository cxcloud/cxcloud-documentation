# Configuring Domains

## Target state

When configuring domains, the assumed starting state is that there is already a site working in "example.com" and you have access to manage DNS. CX Cloud is then installed to utilize this same domain to explore its possibilities. So the target state with CX Cloud would look like this:

* newsite.example.com \(customer front-end\)
* admin.newsite.example.com \("admin panel"\)
* developer.newsite.examle.com \(developer portal\)
* newsite.example.com/api/commerce/v1/ \(commerce service API\)
* newsite.example.com/api/content/v1/ \(content service API\)
* newsite.example.com/api/search/v1/ \(search service API\)
* newsite.example.com/api/auth/v1/ \(auth service API\)

We have used this assumption in our documentation and configuration examples. 


## Configuring a domain for your online service

After your have [configured and validated your infrastructure](generating-infrastructure.md), you will want to deploy a service and assign a domain to it. Let's say we want to deploy a service called `service-search` and assign a domain to it called `newsite.example.com`:

* Figure out what your Load Balancer URL is. The URL is displayed after generating the infrastructure. But you can also find it using:

```bash
kubectl get service nginx-ingress-controller -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}'
```

* Now you have to create an `ALIAS` or `CNAME` record in Route53 pointing to that hostname.
  * More information on how to create an `ALIAS` record in Route53 on AWS [here](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-elb-load-balancer.html)

