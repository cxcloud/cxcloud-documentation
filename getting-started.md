# Getting Started

## Getting Started

To start a simple CXCloud project, you will multiple things:

* An API Micro Service
* Infrastructure using Terraform
* A frontend

TO get started, install the CXCloud command line tools:

```bash
$ npm install -g cxcloud
```

{% hint style="info" %}
 If you don't have `node` and `npm` installed, please [follow this guide](https://nodejs.org/en/download/package-manager/).
{% endhint %}

Once you have installed the `cxcloud` command line tool, you can run it like so:

```bash
$ cxcloud --help
```

### Creating Git Repositories

The `cxcloud` command line tool requires you to create the required git repositories yourself. Create empty repositories on Github or other platforms and clone them locally, so you can run the `cxcloud` tool inside those directories.

### Generating a Micro Service

To generate your API Micro Service \(or any other Micro Service\) please run the following command:

```bash
$ cd my-project-api
$ cxcloud generate microservice
```

You will be prompted to enter your project information. Follow the terminal guide to create and run the project.

Check the [Micro Services](micro-services/) page for more information.

### Generating the Infrastructure

Similarly, run the following command and follow the instructions to create your infrastructure:

```bash
$ cd my-project-infra
$ cxcloud generate infra
```



