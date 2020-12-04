# Frontend Accelerators

Make sure you have [installed all the requirements](../getting-started/prepare-your-environment.md) on your computer and have exported the proper AWS profile:

```text
$ export AWS_PROFILE=my-cxcloud-dev
```

## Create Github repo and clone

You start by creating a new repository for the service in Github, named for example `cx-frontend`. Then clone it to your project folder:

```bash
$ git clone git@github.com:myorganization/cx-frontend.git
```

## Getting started with Angular

### Install Angular CLI

```bash
npm install -g @angular/cli
```

You can verify that the Angular CLI has been installed correctly by checking its version using: `ng --version`.

### Generate skeleton

In the parent directory of the `cx-frontend` folder the `git clone` command above generated, create a workspace with an application project using the Angular CLI command in :

```bash
ng new cx-frontend
```

When prompted, select Yes for Angular routing. And preferably SCSS, SASS or LESS for the stylesheet format. The CLI will then generate the basic skeleton for your application, and install the packages necessary for the application to run.

### Local test

Switch to the generated directory `cd cx-frontend`.

Run `ng serve` to compile the project and start a web server. By default the web server is launched on port 4200.

You can access the project by typing [http://localhost:4200/](http://localhost:4200/) in a browser. You should be able to see a splash screen with the Angular logo on it.

### Add configuration

* Environment setup

For the frontend to work with the APIs, you need to configure the environment files found in at the project directory `/src/environments/`. Refer to [Configuring Frontend](https://docs.cxcloud.com/setting-up-a-cxcloud-project/generating-a-frontend#configuration) for more information.

* Make sure your project has a `Dockerfile` \(this is just an example\):

{% code-tabs %}
{% code-tabs-item title="Dockerfile" %}
```text
### STAGE 1: Build ###

# We label our stage as 'builder'
FROM node:10-alpine as builder

COPY package.json package-lock.json ./

RUN npm set progress=false && npm config set depth 0 && npm cache clean --force

## Storing node modules on a separate layer will prevent unnecessary npm installs at each build
RUN npm i && mkdir /ng-app && cp -R ./node_modules ./ng-app

WORKDIR /ng-app

COPY . .

## Build the angular app in production mode and store the artifacts in dist folder
RUN ENVIRONMENT=docker npm run build
COPY scripts/replace.sh dist/config/

### STAGE 2: Setup ###

FROM nginx:1.13.9

## Copy our default nginx config
COPY scripts/nginx.conf /etc/nginx/conf.d/default.conf
COPY scripts/nginx.sh /run.sh

## Remove default nginx website
RUN rm -rf /usr/share/nginx/html/*

## From 'builder' stage copy over the artifacts in dist folder to default nginx public folder
COPY --from=builder /ng-app/dist /usr/share/nginx/html

CMD ["bash", "/run.sh"]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* Create a file named `.cxcloud.yaml` in the root of your project:

{% code-tabs %}
{% code-tabs-item title=".cxcloud.yaml" %}
```yaml
deployment:
  name: $APP_NAME
  image:
    name: $APP_NAME
    repository: YOUR_AWS_ECR_REPOSITORY_URL_HERE
    version: $APP_VERSION
  containerPort: 80
  replicas: 2
  env:
    - name: NODE_ENV
      value: production
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Add Docker and Nginx scripts

In the root folder of the frontend application, create a folder called _scripts_, and inside it create the following files.

* `extract-config.ts`

```text
import * as fs from 'fs';
import * as path from 'path';
import * as mkdirp from 'mkdirp';

const DIR = path.join(__dirname, '../dist/config');
const ENVS = ['production', 'development'];

mkdirp.sync(DIR);

function escape(str) {
  if (typeof str !== 'string') {
    return str;
  }
  return str.replace(/[\/$'"]/g, '\\$&');
}

ENVS.forEach(env => {
  const { environment } = require(`../src/environments/environment.${env}.ts`);
  const output = Object.keys(environment)
    .map(key => `${key} ${JSON.stringify(escape(environment[key]))}`)
    .join('\n');
  fs.writeFileSync(path.join(DIR, `${env}.cfg`), `${output}\n`);
});
```

* `nginx.conf`

```text
server {

  listen 80;

  sendfile on;

  default_type application/octet-stream;


  gzip on;
  gzip_http_version 1.1;
  gzip_disable      "MSIE [1-6]\.";
  gzip_min_length   256;
  gzip_vary         on;
  gzip_proxied      expired no-cache no-store private auth;
  gzip_types        text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/xml+rss text/javascript;
  gzip_comp_level   9;


  root /usr/share/nginx/html;


  location / {
    try_files $uri $uri/ /index.html =404;
  }

}
```

* `nginx.sh`

```text
#!/bin/bash

chmod +x /usr/share/nginx/html/config/replace.sh
/usr/share/nginx/html/config/replace.sh

nginx -g 'daemon off;'
```

* `replace.sh`

```text
#!/bin/bash

DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
CONFIG=$DIR/$NODE_ENV.cfg
BUNDLE=$DIR/../main.*.js

echo $BUNDLE

if [ ! -f $CONFIG ]; then
  echo "NODE_ENV is not set. Exiting."
  exit 1
fi

while read var value
do
  echo $var=$value
  sed -i "s|$var:\"__PLACEHOLDER__\"|$var:$value|g" $BUNDLE
done < $CONFIG
```

### Deployment

Run the following command to deploy your frontend application:

```bash
$ cxcloud deploy
```

### Routing

Adapt the routing manifest to use the right service name for the frontend. Refer to [Routing Manifest](../setting-up-a-cxcloud-project/routing-manifest.md).

For example, you want to have the following rule:

```text
- path: /
  serviceName: cx-frontend
  servicePort: 80
`
```

### Commit

As a final step, commit changes to your Github repository by running the following commands:

```bash
$ git add .
$ git commit -m "initial commit"
$ git push
```

### Tools recommendations

Install [Prettier](https://prettier.io/docs/en/install.html) using: `npm install --save-dev --save-exact prettier`

* Prettier is a code formatter that enforces style consistency. you can configure prettier using .prettierrc file, written in YAML or JSON in the project root, with optional extensions: .yaml/.yml/.json. An example of a prettier configuration file:

```text
{
  "tabWidth": 2,
  "singleQuote": true,
  "printWidth": 80
}
```

Install [Tslint](https://github.com/palantir/tslint). Using `npm install -g tslint typescript`

* TsLint is a linter for the TypeScript language.

Use TypeScript. Angular itself is written in Typescript.

* Typescript adds optional static typing to Javascript.

Install [Husky](https://github.com/typicode/husky) using `npm install husky --save-dev`

* Husky prevents bad commits by adding pre-commit hooks in package.json.

  The configuration below runs prettier and a `git add` before before commiting to Git.

```text
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{js,json,css,md}": ["prettier --write", "git add"]
  }
}
```

Note that for this configuration to work you need to install [lint-staged](https://github.com/okonet/lint-staged/) using `install --save-dev lint-staged`

Install [Augury](https://augury.rangle.io/)

* Augury is a Developer Tool extension for debugging and profiling Angular applications in Chrome and Firefox. 

Use [NgRx](https://ngrx.io/)

* NgRx is a reactive state management inspired by Redux. 

Use [Internalization](https://angular.io/guide/i18n).

* i18n makes your application available in multiple languages. [ngx-translate](https://www.npmjs.com/package/@ngx-translate/core) is a widely used Angular internationalization library.

### Angular specific styleguide

Angular has [documented](https://angular.io/guide/styleguide) a set of best practices to follow when developer Angular applications. It is highly recommended that you read the styleguide before starting the frontend development to be in line with the syntax, conventions, and application structure.

## Getting started with React

In our React demo, we used [Next.js](https://github.com/zeit/next.js/), a React framework that gives developers the possibility to create server-rendered React apps providing bunch of other nice features as well.

### Running locally

Switch to project directory and install the dependencies by running:
```bash
npm install
```
...then simply run following command to start local server:
```bash
npm run dev
```
...you can now view the demo in your browser:
```bash
http://localhost:3000
```
You can read more about nextjs through their [documentation](https://nextjs.org/docs)

### Style guide

Styles are handled through [styled-components](https://www.styled-components.com/)

You can find the theme constants such as colors, screen sizes for media queries in ```common/theme.js``` file.
