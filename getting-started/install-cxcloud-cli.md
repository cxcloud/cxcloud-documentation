# Install CX Cloud CLI

## Installing CXCloud CLI Tools

{% hint style="info" %}
You need to have Node.js and NPM installed. Follow [this guide](prepare-your-environment.md#install-node-js-and-npm) for installing them.
{% endhint %}

Once you have installed Node.js and you confirm that NPM is working using `npm --version` you can install CXCloud CLI:

```bash
$ npm install -g cxcloud
```

If you need to update CX Cloud CLI, run this command\`:

```text
$ npm i -g cxcloud@latest
```

After installation is complete, you can call cxcloud directly from command line:

```bash
$ cxcloud --version
```

Or if you don't want to install cxcloud globally, you can call it directly using `npx`:

```text
$ npx cxcloud --version
```

