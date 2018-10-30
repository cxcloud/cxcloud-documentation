# Generating a Front-end

CX Cloud CLI allows you to test the concept with end-to-end functionality by generating a front-end demo. Currently, we support Angular 7 and React is coming soon. 

Generated demo will be using our test API end point with pre-filled sets of data for Commerce and Content modules. You can use these projects as a reference for your own development.

Create a new folder for your demo project and go inside of that folder. As you should have CX Cloud CLI installed by now, use the following command to start generating:

```bash
$ cxcloud generate demo
```

You will go through a set of simple questions and receive an end-to-end working demo. 


In case of Angular demo, to explore it, from inside of the project folder you should run the following command:

```bash
$ npm run dev
```

By default, your demo will be available at `http://localhost:4200/ ` but check the console logs.

Angular demo is using the following core modules:

- Commerce (Commercetools)
- Content (Contentful)
- Search (Algolia)

Authentication is implemented via Commerce module in this case, which is good for simple eCommerce project. 


