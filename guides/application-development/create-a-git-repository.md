# Git Repository Conventions

The development strategy required to use a "monorepo" in order to deploy all services using CI/CD pipeline. The repository contains multiple packages \(microservices\) that can but do not have to be related.

## Project structure

The file structure for the monorepo should look like:

```text
my-monorepo
├── packages
|   ├── package 1
|   |   ├── .cxcloud.yaml
|   |   └── ...
|   ├── package 2
|   |   ├── .cxcloud.yaml
|   |   └── ...
|   └── package n
|       ├── .cxcloud.yaml
|       └── ...
├── .cxcloud.yaml
└── ...
```

Every microservice folder should include `.cxcloud.yaml` file with the configuration for Kubernetes.

There is no naming convention for the packages, but it is recommended to add a prefix to the microservice such as `"package-"` or `"service-"`.

In order to simplify and optimize the workflow around development with the multi-package repository, the managing tool such as [Lerna](https://lerna.js.org/) can be in use.

