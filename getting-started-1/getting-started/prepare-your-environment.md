# Prepare Local Environment

## Install Git client

Currently, our CLI only supports GitHub as a versioning system. Once you have installed Git client of your preference, make sure to configure global `username` and `email` properties with values from your GitHub account like this:

```bash
git config --global user.name "username"
git config --global user.email "example@email.com"
```

## Install and Configure the AWS CLI tools

You naturally need an AWS account. For quick testing, You can create your own [AWS Free Tier account](https://aws.amazon.com/free/). Then install the AWS Cli tools using [this guide](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) and configure a profile:

```bash
$ aws configure --profile myproject
```

Check [this guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for more information on configuring AWS CLI.

Export your AWS profile to an environment variable. This variable is used by CX Cloud tools to interact with your AWS account:

```bash
$ export AWS_PROFILE=myproject
```

CX Cloud CLI supports preset AWS API keys as environment variables. This is useful for example when running [AWS Vault](https://github.com/99designs/aws-vault) for storing the AWS secrets in your operating system's secure keystore. The `AWS_PROFILE` doesn't need to be exported in case AWS Vault is used.

## Install Kubernetes and Kops

Before you start, you need to have Kubernetes and it's tools installed on your computer. Follow [this guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/) to Install those. On Mac, you can install Kubernetes tools using `homebrew`:

```bash
$ brew install kubernetes-cli
```

CXCloud CLI uses Kops to create and configure your Kubernetes cluster. You can install it on your Mac using `homebrew`:

```bash
$ brew update && brew install kops
```

Or on your Linux machine:

```bash
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops
```

Check [Kops' documentation](https://github.com/kubernetes/kops#installing) for more information.

## Install Helm

[Install the Helm client](https://docs.helm.sh/using_helm/#installing-helm) on your machine. On Mac this can be done using `homebrew`:

```bash
$ brew install kubernetes-helm
```

## Install Terraform

[Install Terraform ](https://www.terraform.io/intro/getting-started/install.html) on your machine. On Mac this can be done using `homebrew`:

```bash
$ brew install terraform
```

## Install Node.js and NPM

You need to install Node.js and NPM on your computer. It is recommended to install the latest Node.js LTS version. On macOS you can do this using `homebrew`:

```text
$ brew install node
```

For other systems, you can check [Node.js official documentation](https://nodejs.org/en/download/package-manager/).

Once you have installed Node, you can confirm that NPM is working using `npm --version`

## Install Docker

To be able to compose a deployable image you have to install Docker. Head over to [this page](https://docs.docker.com/install/#supported-platforms) and get the Docker for your system. You'll need to create a Docker account as well.

## Install Ansible

To be able to access the private services in Kubernetes an VPN solution is needed. Ansible is used to install OpenVPN for the CX Cloud Demo.

[Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) on your machine. On Mac this can be done using `homebrew`:

```text
$ brew install ansible
```

