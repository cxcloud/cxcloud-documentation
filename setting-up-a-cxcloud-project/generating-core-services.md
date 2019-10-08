# Generating Core Services

CX Cloud provides selected ready-made \(Node.js\) core services \(e.g. commerce, content, search, authentication\) which can be generated and added to the project using CX Cloud CLI. These can, of course, be customized later on. <!-- Also, CX Cloud enables you to deploy any custom services with any tech stack. Read more about that [here](../guides/deploying-services.md). -->

## Requirements

It is required to use a monorepo structure in order to deploy all microservices with CI/CD pipeline. More information about monorepo development strategy can be found [here](../getting-started/create-a-git-repository.md).

## Service generation

You can start by creating a new folder for a new service in monorepo `packages` folder, for example:

```console
my-monorepo
├── packages
|   ├── package-commerce
|   |   ├── .cxcloud.yaml
|   |   └── ...
└── ...
```

Then navigate inside the newly created folder and start service generation by running the following commands:

```bash
$ cd package-commerce
$ cxcloud generate service
```

You will go through a number of questions and get to choose which service to create.

To enable ready-made functionality for services, CX Cloud provides a set of core modules that can be used when generating services. To enable the desired level of granularity, one can use multiple core modules for one service or create multiple services based on the same core module. It should be noted that if generating service with multiple core modules, some services might conflict with each other. In this case, you will get a warning. Example of such conflict is Commerce and Auth services. Commerce includes own authentication service, making Auth in this case redundant.

Currently CLI supports the following core modules:

- Commerce \(commercetools\)
- Content \(Contentful\)
- Auth \(AWS Cognito\)
- Search \(Algolia\)

## Configuration

After you have generated the service, it's time to provide it with configurations.

Create a `.env` file in the root directory of the monorepo and add environment-specific variables on new lines in the form `NAME=VALUE`. For example:

```env
CONTENTFUL_API_KEY=12345
CTP_CLIENT_ID=qwerty-567
AUTH_URL=https://auth.example.com
...
```

{% hint style="warning" %}
**Do Not** commit sensitive information such as API keys to GitHub or other versioning systems. Make sure the `.env` file is included in `.gitignore` of the root folder.
{% endhint %}

In order to provide a consistent configuration interface to the services, which can be extended and overridden during further development, the configurations are stored in configuration files within the service. For these purposes, we are using `node-config`. The generated with CLI services are already prepackaged with `node-config` and basic configuration files. Among other settings, the configuration files include references to the environment variables which are loaded from `.env` file into `process.env`. Documentation for node-config can be found [here](https://www.npmjs.com/package/config).

The configurations below are used in CX Cloud Demo and can be copied and modified in default configuration file `config/default.js` of generated service.

Example configuration for Commerce:

```javascript
"store": {
    "defaultCurrency": "EUR",
    "supportedCurrencies": ["EUR", "USD", "GBP"]
},
"commerceTools": {
    "authHost": `${process.env.CTP_AUTH_URL}`,
    "apiHost": `${process.env.CTP_API_URL}`,
    "projectKey": process.env.CTP_PROJECT_KEY,
    "admin": {
        "clientId": process.env.CTP_CLIENT_ID,
        "clientSecret": process.env.CTP_CLIENT_SECRET
    },
    "user": {
        "clientId": process.env.CTP_USER_CLIENT_ID,
        "clientSecret": process.env.CTP_USER_CLIENT_SECRET
    }
}
```

Example configuration for Content:

```javascript
"contentful": {
    "sdkConfig": {
        "space": process.env.CONTENTFUL_SPACE_ID,
        "accessToken": process.env.CONTENTFUL_DELIVERY_API_TOKEN
    }
}
```

Example configuration for Auth:

```javascript
"cognito": {
    "userPoolId": process.env.USER_POOL_ID,
    "clientId": process.env.CLIENT_ID,
    "encryptionKey": process.env.ENCRYPTION_ID
}
```

Example configuration for Search:

```javascript
"algolia": {
    "applicationId": process.env.ALGOLIA_APPLICATION_ID,
    "apiKey": process.env.ALGOLIA_API_KEY
}
```

{% hint style="info" %}
CXCloud does not provide any testable configurations for 3rd party solutions. You must get test account or purchase licenses for selected services and set them up yourself.
{% endhint %}

## Local test

After generation is finished and configurations are set, the generated service will be working as a single API. To test locally, run the following set of commands at the root of your repository with the service:

```bash
$ npm install
$ npm run build
$ npm run start
```

It will install all required npm packages, build your Swagger documentation and start the local server for you to be able to test your configurations. Swagger provides an interface for testing right in the browser.

In case of `commerce` service and default settings, you should be able to see Swagger API documentation at `http://localhost:4003/api/commerce/v1/api-docs`

## Deployment

CI/CD pipeline handles the deployment of microservices within the monorepo. Therefore, CI/CD pipeline should be set up and configured prior deployment of the services. More information about CI/CD pipeline configuration and monorepo settings for deployment can be found [here](configuring-cicd.md).

{% hint style="info" %}
When applications is deployed, all sensitive information such as keys, secrets or tokens are stored as Kubernetes secret object in the intended namespace. You can find an example of using Kubernetes' secrets to configure your project [here](../guides/storing-secrets-in-kubernetes.md#example-storing-and-using-secrets-in-nodejs-and-node-config-module).
{% endhint %}

## Routing

After deployment, you can make your service available to the world using a [Routing Manifest](routing-manifest.md).

## Commit

As a final step, add git remote origin corresponding to your monorepo and commit changes to your Github repository by running the following commands:

```bash
$ git remote add origin https://github.com/user/my-monorepo.git
$ git add .
$ git commit -m "initial commit"
$ git push
```
