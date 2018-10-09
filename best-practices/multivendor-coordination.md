# Multivendor coordination


The CX Cloud enables a way to do API development with multiple vendors where each vendor has only access to the API they are managing. There are two best practises described here: one for situation where all development is done in a single AWS account and second where vendors are developing in their own AWS accounts and APIs are managed in a central core AWS account. The second way offers proper separation of concerns and will allow vendors to only change resources related to their own API development. It also allows for a parallel deployment cycle and faster lead time for changes to production.

## Single AWS account

In the single AWS account model only one account is used. Different resources \(e.g. APIs and API implementations\) can be managed by different vendors. This allows flexibly to implement different kinds of deployment processes and solutions, but also has added risks where vendors are able to modify other vendor's resources by accident. Also different infra management tools and methods by different vendors can make the system complicated. A governance process is needed to manage risks.

It is still recommended to have separate APIs for each vendor so that deployments to APIs can be done in parallel.

In this model, it is possible to gain full separation of access to different resources in the AWS account, but it required complicated IAM profile development and in some AWS resources it is impossible to allow users to create resources and then edit them also. This is a current technical limitation of AWS IAM definitions.

## Vendor accounts, shared API gateway

By using an API gateway and IAM profiles, it is possible to do multivendor API development where each vendor has fully parallel deployment cycle to the API gateway and API implementation and also has no access to other APIs by other vendors. They are also able to choose their API implementation technologies as they wish.

There is a core AWS account maintained by a core team that contains the separate API gateway APIs. For each vendor, the core team creates an API in the API gateway and a corresponding IAM user that has access only to that API. This can be managed automatically by e.g. Terraform so that it is easy to add new vendors.

Each vendor has an AWS account that they can manage and organise as they wish. By using the IAM user given by the core team, they can manage the API \(including configuration and deployments\) they are responsible for. CX Cloud recommendation for managing AWS infra is Terraform.

The API is then configured by the vendor to use either Lambda or HTTP proxy and use the API implementation in their own account.

Benefits:

* Simple process of how to get started, not much pre-work is needed by the core team
* Isolation of the most volatile parts of the system to each vendor's own AWS account
* separate APIs means parallel deployments to API
* vendor management of both API and implementation allows them to sync deployments
* freedom of technology choices \(e.g. infra management tools etc\) for each vendor



