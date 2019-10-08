# Getting access to an already running Infrastructure

In most real world cases, you haven't probably generated and set up the Kubernetes infrastructure yourself. But you might need to access it to deploy services or change settings.

To do that, you should have access to the AWS account that the infrastructure is running in, and export the `AWS_PROFILE` variable like before:

```bash
$ export AWS_PROFILE=my-cxcloud-dev
```

Now run the following script to download the configuration from the running cluster to your system:

```bash
$ kops export kubecfg somecluster.k8s.local --state=s3://somecluster.k8s.local
```

This command will download the configuration to your system and sets up a Kubernetes `context` for easier use. Now you can run any `kubectl` or `cxcloud` command to interact with your cluster.

## Kubernetes Contexts

In case you have access to more than one Kubernetes cluster, you can use `kubectl config` tool and switch between different contexts.

To see a list of your available contexts run:

```bash
$ kubectl config get-contexts
```

This command will print a list of configured contexts on your system and the currently in use context.

To switch between contexts, run:

```bash
$ kubectl config use-context mycluster.k8s.local
```

