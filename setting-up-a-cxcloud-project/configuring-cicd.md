# Configuring CI/CD pipeline

When using CX Cloud CLI, it is fairly easy for one to both create and then deploy CX Cloud infra, selected services and demo frontends to AWS. This is typically enough for testing and exploration needs. We also have [tools and guidelines](../best-practices/how-to-run-infra.md) for DevOps engineer to manually manage your environment. 

But when preparing for customer project development phase, creating a CI/CD pipeline between your Github \(or the Git host of your choice eg. Bitbucket\) and various environments in AWS \(or selected cloud provider eg. Azure\) is the next thing to do. As this area can be very dependent on customer preferences and existing set up, you probably will customize your pipeline. Regardless, we have created this basic set up.

## Development workflow

The CX Cloud CI/CD pipeline has been designed with the following development workflow in mind.

![Development workflow](../.gitbook/assets/dev-workflow.png)

1. A new feature branch is created, the pipeline will checkout the code and run unit tests and perform code quality analysis. This step will be repeated every time a new commit is pushed to the branch..

2. When the feature is ready for review and testing a pull request \(PR\) is created. The pipeline will automatically run tests and code quality analysis. The pipeline will fail in case the tests fails or if the quality analysis doesn't pass the specified quality level. The pipeline will continue with building the micro services and deploy them into a Kubernetes namespace specific for the PR. The pipeline will send notifications to communication channels and publish the web url as a comment to the PR. Now the PR is ready for code review and quality assurance. New commits pushed to the PR will only analyze and deploy the modified projects.

3. When the pull request has passed all steps, it's possible to merge to master. When new code is pushed to master the pipeline will run almost the same steps as for pull requests except this time the pipeline will deploy to the staging environment \(or Kubernetes namespace staging\).

4. The process has been designed so that master is alway deployable to production. To deploy to production a release tag has to be made. The pipeline will automatically recognize all git tags and any of the tags can be with one click in Jenkins build and deployed to production.

### Requirements for the pipeline

- Kubernetes cluster
- Jenkins with preinstalled plugins
- Monorepo architecture of the micro services

### How to use the pipeline

The pipeline has been developed for Jenkins. A customized Jenkins image can be build from our [Jenkins repository](https://github.com/cxcloud/jenkins). More detailed instructions below.

The Pipeline requires that the development strategy uses a monorepo in order to deploy all services. The directory structure for the monorepo should look like:

```console
.
├── packages
|   ├── service 1
|   |   ├── .cxcloud.yaml
|   |   └── ...
|   ├── service 2
|   |   ├── .cxcloud.yaml
|   |   └── ...
|   └── service n
|       ├── .cxcloud.yaml
|       └── ...
├── .cxcloud.yaml
├── Jenkinsfile
└── ...
```

#### Repository root

In the root of the repository should be two files, `Jenkinsfile` and `.cxcloud.yaml`. The pipeline code is in the Jenkinsfile and the `.cxcloud.yaml` file contains the Kubernetes routing for the microservices.

Example of the `.cxcloud.yaml` file in the root of the repository:

```sh
namespace: $GIT_BRANCH
routing:
  domain: $GIT_BRANCH.dev.cxcloud.com
  ingressClass: $INGRESS_CLASS
  lbCert: $LB_CERT
  scheme: $SCHEME
  ssl: false
  rules:
    - path: /graphql
      serviceName: package-graphql-proxy
      servicePort: 80
    - path: /images
      serviceName: package-image-proxy
      servicePort: 80
    - path: /
      serviceName: package-frontend
      servicePort: 80
```

#### Repository services

Every microservice should preferable be in the packages folder but there is no naming convention for the services. Every microservice has to have a `.cxcloud.yaml` in the folder containing the Kuberntes deployments.

Kubernetes secrets should be stored in the namespace they are intended for. However, secrets intended for the PR environments / namespaces should be stored in the applications namespace. The pipeline will copy them from the applications namespace into the specific PR namespace.

Example of the content in the `.cxcloud.yaml` file for a microservice:

```sh
namespace: $GIT_BRANCH
deployment:
  name: $APP_NAME
  image:
    name: $APP_NAME
    repository: $ECR_REPOSITORY
    version: $APP_VERSION
  containerPort: 3000
  port: 80
  replicas: $MIN_REPLICAS
  cpuRequest: $CPU_REQUEST
  nodeSelector:
    kops.k8s.io/instancegroup: $INSTANCE_GROUP
  autoscaling:
    minReplicas: $MIN_REPLICAS
    maxReplicas: $MAX_REPLICAS
    targetAverageUtilization: 75
  env:
    - name: NODE_ENV
      value: production
    - name: CONTENTFUL_MANAGEMENT_API_TOKEN
      valueFrom:
        secretKeyRef:
          name: cxcloud-secret
          key: CONTENTFUL_MANAGEMENT_API_TOKEN
    - name: CONTENTFUL_ENVIRONMENT_ID
      valueFrom:
        secretKeyRef:
          name: cxcloud-secret
          key: CONTENTFUL_ENVIRONMENT_ID
    - name: CONTENTFUL_SPACE_ID
      valueFrom:
        secretKeyRef:
          name: cxcloud-secret
          key: CONTENTFUL_SPACE_ID
```

## Jenkins
