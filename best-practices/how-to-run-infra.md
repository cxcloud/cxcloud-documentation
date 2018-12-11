# Managing your infra and Kubernetes

With CX Cloud CLI, you can set-up your Kubernetes cluster and then move forward with services and frontend's.

But to manage your infra and Kubernetes cluster, there are a couple of tools you can use. There is no "one tool to manage everything" but use case specific, widely used tools. Essentially CX Cloud CLI, [Kops](https://github.com/kubernetes/kops), [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) and [Terraform](https://www.terraform.io/%20). 

## Tools to manage your CX Cloud infra

| Task | CX Cloud CLI | Kops | kubectl | Terraform |
| :--- | :---: | :---: | :---: | :---: |
| Create K8s cluster | x | x |  |  |
| View K8s pods |  |  | x |  |
| Delete K8s pods |  |  | x |  |
| Delete K8s cluster |  | x |  |  |
| Add API GW |  |  |  | x |
| Manage AWS capacity |  |  |  | x |
| ? |  |  |  |  |
| ? |  |  |  |  |
| ? |  |  |  |  |

## Notes about tasks

#### Create K8s cluster

One uses CX Cloud CLI to both generate the cluster and then generate and deploy services to it. One can also use kops manually to create the cluster. 



#### View & Delete pods / services

One uses kubectl for these tasks. Some examples: 

See what deployments you have  
"kubectl get deployments -n applications" \(assuming namespace is applications\)  
  
Delete selected deployment  
"kubectl delete deployment SELECTED\_DEPLOYMENT -n applications"  
  
See what services you have  
"kubectl get services -n applications"  
  
Delete selected service  
"kubectl delete service SELECTED\_SERVICE -n applications"



#### Delete K8s cluster

One uses kops to delete the whole cluster. 



#### Add API Gateway

One uses Terraform to add API GW to the tech stack.  



#### Manage AWS / Azure capacity 

One uses Terraform to manage the infrastructure powering K8s cluster. 



## Other useful tools 

There are also other tools that a DevOps engineer running CX Cloud might find useful. For example [Kubernetes Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/).

