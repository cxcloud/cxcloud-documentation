# Generating Infrastructure

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
3. Generates Terraform files for setting up the Kubernetes cluster
4. Runs and applies the Terraform setup
5. Waits for the Kubernetes cluster to become available
6. Configures access controls on the Kubernetes cluster
7. Installs Tiller, nginx-ingress and cert-manager on the cluster
8. Setups of staging and production LetsEncrypt issuers 

