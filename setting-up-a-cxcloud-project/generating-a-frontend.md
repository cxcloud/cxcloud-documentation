# Generating a Frontend Demo

Cool, You now have infra and core services up and running! For front-end, CX Cloud CLI allows you to generate a front-end demo to test the concept with end-to-end functionality. Currently, we provide Angular 7 demo. React is coming soon.

## Front-end generation

You start by creating a new repository for the service in Github, named for example `frontend-demo`. Then clone it to your project folder and start front-end demo generation by running the following commands:

```bash
$ git clone git@github.com:myorganization/frontend-demo.git
$ cd frontend-demo
$ cxcloud generate demo
```

You will go through a set of simple questions and receive an end-to-end working demo.

## Configuration

B default, the generated demo uses our live test API including end points with pre-filled sets of data for Commerce, Content and Search services. 

In order to enable the frontend demo to work with the APIs you have generated and deployed, configuration files need to be modified. Configuration files can be found in this project directory `/src/environments/`. 

To connect with new APIs, change the value of `apiUrl` for each service \(notice that environments files should be modified depending on the used environment\).

```text
export const environment = {
  production: false,
  siteName: 'CXCloud DEV',
  commerce: {
    apiUrl: 'https://<YOUR_URL>/api/v1',
    indexName: 'dev_COMMERCE'
  },
  content: {
    apiUrl: 'https://<YOUR_URL>/api/v1/content',
    indexName: 'dev_CONTENT'
  },
  auth: {
    apiUrl: 'https://<YOUR_URL>/api/v1/auth'
  },
  search: {
    apiUrl: 'https://<YOUR_URL>/api/v1/search'
  }
};
```

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

## Routing

After deployment, you can make your service available to the world using a [Routing Manifest](routing-manifest.md).

## Commit

As a final step, commit changes to your Github repository by running the following commands:

```bash
$ git add .
$ git commit -m "initial commit"
$ git push
```

