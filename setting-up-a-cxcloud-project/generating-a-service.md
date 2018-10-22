# Generating a Service

{% hint style="info" %}
Make sure you have generated infrastructure and exported your AWS profile before continuing. [Read the docs](generating-infrastructure.md) to find out how.
{% endhint %}

CXCloud provides support for selected services, which can be generated and added to the project using CXCloud CLI. 

You start by creating a new repository for the service in Github, named for example `service-commerce`. Then clone it to your project folder and start service generation by running the following commands:

```bash
$ git clone git@github.com:myorganization/service-commerce.git
$ cd service-commerce
$ cxcloud generate service
```

You will go through a number of questions and get to choose which services to create. At this point, CXCloud CLI won't deploy anything yet, though it might ask to perform some authentefication steps for AWS.

Some services might conflict with each other, in this case you will get a warning. Example of such conflict is Commerce and Auth services. Commerce includes own authentication service, making Auth in this case redundant.

Currently CLI supports next services:

- Commerce (Commercetools)
- Content (Contentful)
- Auth (AWS Cognito)
- Search (Algolia)

After you have generated the services, it's time to provide them with configurations. We are using Node-config for providing different configurations based on environment. Documentation for Node-config can be foud [here](https://www.npmjs.com/package/config).

Create file `\config\development.json` and add necessary configurations there. Each service should have its own object with configurations. Make sure to check the application port there as well. For production, you should create `\config\production.json` respectively. These files will override configurations from the `\config\default.json`, but keep the non-sensitive configurations there (like `defaultCurrency`) and add the environment-related keys to `development.json` or `production.json` files. 

`development.json` file will be automatically loaded when you are testing at your local environment while `production.json` will be used if you deploy the services.

{% hint style="info" %}
CXCloud does not provide any testable configurations. You must purchase licenses for selected services and set them up yourself.
{% endhint %}

{% hint style="warning" %}
If you use public repository for versioning, make sure to encrypt the `development.json` and `production.json` files (for instance, with git-crypt).
{% endhint %}

Example configurations for Commerce:

```json
"store": {
    "defaultCurrency": "EUR",
    "supportedCurrencies": ["EUR", "USD", "GBP"]
},
"commerceTools": {
    "authHost": "AUTH_HOST_DOMAIN",
    "apiHost": "API_HOST_DOMAIN",
    "projectKey": "PROJECT_KEY",
    "admin": {
        "clientId": "CLIENT_ID",
        "clientSecret": "CLIENT_SECRET"
    },
    "user": {
        "clientId": "CLIENT_ID",
        "clientSecret": "CLIENT_SECRET"
    }
}
```

Example configurations for Content:

```json
"contentful": {
    "sdkConfig": {
        "space": "SPACE",
        "accessToken": "ACCESS_TOKEN"
    }
}
```

Example configurations for Auth:

```json
"cognito": {
    "userPoolId": "USER_POOL_ID",
    "clientId": "CLIENT_ID",
    "encryptionKey": "ENCRYPTION_ID"
}
```

Example configurations for Search:

```json
"algolia": {
    "applicationId": "APPLICATION_ID",
    "apiKey": "API_KEY"
}
```

After generation is finished and configurations are set, all of selected services will be working as a single API. You should be able to see Swagger API documentation at `http://localhost:4003/api/v1/api-docs`. To do that, run the following set of commands at the root of your repository with services:

```bash
$ npm run build
$ npm run start
```

It will build your Swagger documentation and start the local server for you to be able to test your configurations. Swagger provides an interface for testing right in the browser.

After you have tested the API, if you chose `deploy to cluster` option, you can deploy your services using the following command:

```bash
$ cxcloud deploy
```
