# Frontend accelerators

## Getting started with Angular 

{% hint style="info" %}
You need to have Node.js and NPM installed. Follow [this guide](prepare-your-environment.md#install-node-js-and-npm) for installing them.
{% endhint %}

### test 

```bash
npm install -g @angular/cli
```

You can verify that the Angular CLI has been installed correctly by checking its version using: `ng -v`.

Create a workspace with an application project using the Angular CLI command:

```bash
ng new <appname>
```

When promopted, select Yes for Angular routing. And preferably SCSS, SASS or LESS for the stylesheet format.

The CLI will generate the basic skeleton for your application, and install the packages necessary for the application to run.

Switch to the generated directoy `cd <appname>`. 

Run `ng serve` to compile the project and start a web server. By default the web server is launched on port 4200. 

You can access the project by typing http://localhost:4200/ in a browser. You should be able to see a splash screen with the Angular logo on it. 

For the frontend to work with the APIs, you need to configure the environment files found in at the project directory `/src/environments/`. Refer to [Configuring Frontend](https://docs.cxcloud.com/setting-up-a-cxcloud-project/generating-a-frontend#configuration) for more information. 


### Tools recommendations

Install [Prettier](https://prettier.io/docs/en/install.html) using: `npm install --save-dev --save-exact prettier`
* Prettier is a code formatter that enforces style consistency.

Install [Tslint](https://github.com/palantir/tslint). Using `npm install -g tslint typescript`
* TsLint is a linter for the TypeScript language.

Use TypeScript. Angular itself is written in Typescript. 
* Typescript adds optional static typing to Javascript.

Install [Husky](https://github.com/typicode/husky) using `npm install husky --save-dev`
* Husky prevents bad commits by adding pre-commit hooks in package.json.

Install [Angular Material](https://material.angular.io/) using: `npm install --save @angular/material @angular/cdk @angular/animations`
* Material design provides components and tools that support the best practices of UI design.

Install [Augury](https://augury.rangle.io/) 
* Augury is a Developer Tool extension for debugging and profiling Angular applications in Chrome and Firefox. 

Use [NgRx](https://ngrx.io/) 
* NgRx is a reactive state management inspired by Redux. 

### Angular specific styleguide

Angular has [documented](https://angular.io/guide/styleguide) a set of best practices to follow when developer Angular applications. It is highly recommended that you read the styleguide before starting the frontend development to be in line with the syntax, conventions, and application structure. 
