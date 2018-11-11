# Generating a Frontend Demo

Cool, You now have infra and services up and running! For front-end, CX Cloud CLI allows you to generate a front-end demo to test the concept with end-to-end functionality. Currently, we support Angular 7 and React is coming soon. In the future, we will also provide accelerators to be used as basis for customer specific implementation.

## Front-end generation

You start by creating a new repository for the service in Github, named for example `frontend-demo`. Then clone it to your project folder and start front-end demo generation by running the following commands:

```bash
$ git clone git@github.com:myorganization/frontend-demo.git
$ cd frontend-demo
$ cxcloud generate demo
```

You will go through a set of simple questions and receive an end-to-end working demo.

It should be noted that our Angular demo is using one API/service, with the Commerce \(commercetools\), Content \(Contenful\) and Search \(Algolia\) core modules enabled. Authentication is implemented via Commerce core module in this case, which is good for simple eCommerce project.

## Configuration

Generated demo will be using our live test API including end points with pre-filled sets of data for Commerce, Content and Search services. Later we will provide a guide to configure your front-end to connect to your own services, with set level of granularity, running in your AWS.

## Local test

In case of Angular demo, to explore it, from inside of the project folder you should run the following command:

```bash
$ npm run dev
```

By default, your demo will be available at `http://localhost:4200/` but check the console logs.

## Deployment

Run the following command to deploy your frontend demo:

```bash
$ cxcloud deploy
```

Check [Deploying Service](deploying-services.md) and [Routing Manifest](routing-manifest.md) sections for more information on deployment and routing.

## Commit

As a final step, commit changes to your Github repository by running the following commands:

```bash
$ git add .
$ git commit -m "initial commit"
$ git push
```

