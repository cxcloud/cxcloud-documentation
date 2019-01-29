# Deleting deployments

After you have deployed a service using `cxcloud deploy` you can run the following command to delete the deployment and all of it's configurations:

```bash
$ cxcloud deploy --purge
```

This will undo anything that is done by running `cxcloud deploy` except it's namespace. If you wish to destroy the namespace as well, run:

```bash
$ cxcloud deploy --purge-all
```

{% hint style="warning" %}
If multiple services are using the same namespace that you have defined in your manifest, this command will purge all other services as well because it will destroy the namespace.
{% endhint %}

