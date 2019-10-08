# Why CX Cloud?

CX Cloud is a battle-proven solution accelerator for cloud native architecture. It is based on API-first approach, microservices and cloud infrastructure providing fast time-to-market, agility in development and lack of waste via automation. 

Example solutions include web shops, customer self-service solutions, content rich web sites and API-layer for mobile apps and other clients. Solution implementation can also be part of "decomposing the monolith" program where your customer targets to create new online solutions with modern omni-channel architecture while at the same time reducing investments towards legacy platforms.

We have seen many cloud native initiatives fail to reach the high expectations due to siloed approach and lack of governance in the areas of end-to-end feasibility, common services and long term architecture. For us, CX Cloud is a tool to promote also these long-term aspects in addition to enabling fast innovation and benefitting from cloud native approach. 

It is good to remember that even if CX Cloud provides guidance and accelerators to create an end-to-end online solution including infra, services and front-end, it is not a ready-made solution. It is more of a framework and its granularity of accelerators enable you to hand pick only the ones you need. For example, you can just use the infra and CI/CD part to get your team up and running in one day and then continue with customizing the rest of the stack.

In high level, there are three main scenarios for usage.

* A green field project to create a new solution or replace existing solution
* Use CX Cloud for a "decomposing the monolith" program
* Add CX Cloud capablities to an existing microservices stack

## 1. "Greenfield"

In this scenario, the solution scope has little dependencies on the existing IT stack. You have quite big freedom in technology choices regarding both services, front-ends, infrastructure, and CI/CD configurations. As the target is quite a "stand-alone" solution stack, no separate API Gateway is necessarily needed in first phase as CX Cloud runs Kubernetes with an internal ingress loadbalancer.

For a greenfield project, you can make a clean start for your project using [Kubernetes Infra Template](https://github.com/cxcloud/demo-cxcloud-k8s) to install the infra with Kubernetes and [CX Cloud CLI](https://github.com/cxcloud/cxcloud-cli) and generate services needed for your project in hand.

## 2. "Decomposing the monolith"

Quite similar to the greenfield. But there is a need for separate API Gateway to enable multi-vendor approach \(different vendors can expose API's via one gateway\). This gateway can also be utilized to expose API's of current legacy platforms for new client/front-end solutions. CX Cloud enables generating and running AWS API Gateway outside the Kubernetes cluster but you can also configure the tool of choice \(e.g. WSO2, Azure API Management\) separately.

In this case, you can make a clean start for your project using [Kubernetes Infra Template](https://github.com/cxcloud/demo-cxcloud-k8s) to install the infra with Kubernetes and [CX Cloud CLI](https://github.com/cxcloud/cxcloud-cli) and generate services needed for your project.

## 3. Add capabilities for your existing microservice stack

In this scenario, there is already a microservices architecture in place, typically including centralized API Gateway, version control, and DevOps pipeline. Your task might be for example to add additional capabilities \(e.g. commerce, content, search, process-engine\) to the existing stack. And in this area, CX Cloud services might come in handy.

In this case, you can generate required services or service template using [CX Cloud CLI](https://github.com/cxcloud/cxcloud-cli). More information about generating core services can be found here: [Generating Core Services](https://docs.cxcloud.com/setting-up-a-cxcloud-project/generating-core-services).

