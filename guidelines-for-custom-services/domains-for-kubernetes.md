# Domains for Kubernetes

## Configuring a domain for the Kubernetes Cluster

{% hint style="info" %}
The DNS configuration for Kubernetes is optional. Instead, a gossip-based cluster can be easily created. The only requirement to trigger this is to have the cluster name end with `.k8s.local`. If a gossip-based cluster is created then you can skip this section.
{% endhint %}

In order to build a Kubernetes cluster with `cxcloud`, we need to prepare somewhere to build the required DNS records. There are three scenarios below and you should choose the one that most closely matches your AWS situation:

### Scenario 1a: A Domain purchased/hosted via AWS

If you bought your domain with AWS, then you should already have a hosted zone in Route53. If you plan to use this domain then no more work is needed.

### Scenario 1b: A subdomain under a domain purchased/hosted via AWS

In this scenario you want to contain all kubernetes records under a subdomain of a domain you host in Route53. This requires creating a second hosted zone in route53, and then setting up route delegation to the new zone.

In this example you own `example.com` and your records for Kubernetes would look like `etcd-us-east-1c.internal.clustername.subdomain.example.com`

This is copying the NS servers of your **SUBDOMAIN** up to the **PARENT** domain in Route53. To do this you should:

* Create the subdomain, and note your **SUBDOMAIN** name servers \(If you have already done this you can also [get the values](https://github.com/kubernetes/kops/blob/master/docs/ns.md)\)

```bash
# Note: This example assumes you have jq installed locally.
ID=$(uuidgen) && aws route53 create-hosted-zone --name subdomain.example.com --caller-reference $ID | \
    jq .DelegationSet.NameServers
```

* Note your **PARENT** hosted zone id

```bash
# Note: This example assumes you have jq installed locally.
aws route53 list-hosted-zones | jq '.HostedZones[] | select(.Name=="example.com.") | .Id'
```

* Create a new JSON file with your values \(`subdomain.json`\)

```javascript
{
  "Comment": "Create a subdomain NS record in the parent domain",
  "Changes": [
    {
      "Action": "CREATE",
      "ResourceRecordSet": {
        "Name": "subdomain.example.com",
        "Type": "NS",
        "TTL": 300,
        "ResourceRecords": [
          {
            "Value": "ns-1.awsdns-1.co.uk"
          },
          {
            "Value": "ns-2.awsdns-2.org"
          },
          {
            "Value": "ns-3.awsdns-3.com"
          },
          {
            "Value": "ns-4.awsdns-4.net"
          }
        ]
      }
    }
  ]
}
```

* Apply the **SUBDOMAIN** NS records to the **PARENT** hosted zone.

```bash
aws route53 change-resource-record-sets \
 --hosted-zone-id <parent-zone-id> \
 --change-batch file://subdomain.json
```

Now traffic to `*.subdomain.example.com` will be routed to the correct subdomain hosted zone in Route53.

### Scenario 2: Setting up Route53 for a domain purchased with another registrar

If you bought your domain elsewhere, and would like to dedicate the entire domain to AWS you should follow the guide [here](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-transfer-to-route-53.html)

### Scenario 3: Subdomain for clusters in route53, leaving the domain at another registrar

If you bought your domain elsewhere, but **only want to use a subdomain in AWS Route53** you must modify your registrar's NS \(NameServer\) records. We'll create a hosted zone in Route53, and then migrate the subdomain's NS records to your other registrar.

You might need to grab [jq](https://github.com/stedolan/jq/wiki/Installation) for some of these instructions.

* Create the subdomain, and note your name servers \(If you have already done this you can also [get the values](https://github.com/kubernetes/kops/blob/master/docs/ns.md)\)

```text
ID=$(uuidgen) && aws route53 create-hosted-zone --name subdomain.example.com --caller-reference $ID | jq .DelegationSet.NameServers
```

* You will now go to your registrar's page and log in. You will need to create a new **SUBDOMAIN**, and use the 4 NS records received from the above command for the new **SUBDOMAIN**. This **MUST** be done in order to use your cluster. Do **NOT** change your top level NS record, or you might take your site offline.
* Information on adding NS records with [Godaddy.com](https://www.godaddy.com/help/set-custom-nameservers-for-domains-registered-with-godaddy-12317)
* Information on adding NS records with [Google Cloud Platform](https://cloud.google.com/dns/update-name-servers)

