This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app --typescript`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).
# NextJs Aws Serverless Deploy

---
- [Description](#description)
- [Installation](#installation)
- [Env Example](#env-example)
- [Package Json Scripts](#package-json-scripts)
- [Testing](#testing)
- [Linting](#linting)
- [Git Hooks](#git-hooks)
- [Deployment](#deployment)

---


# Description
### ([top](#nextjs-aws-serverless-deploy))


This Next Js starter repo is configured with:
* [NextJs 12](https://nextjs.org/blog/next-12) (using swc Minify)
* [Typescript](https://www.typescriptlang.org/)
* [ESLint](https://eslint.org/) Linter
* [Jest](https://jestjs.io/) Testing framework
* [Husky](https://typicode.github.io/husky/#/) Git Hooks
* [AWS CDK CI/CD Pipeline](https://docs.aws.amazon.com/cdk/v1/guide/cdk_pipeline.html) (Infrastructure as code).
* [Serverless NextJs CDK Construct](https://serverless-nextjs.com/docs/cdkconstruct/) (experimental)

---


# Installation
### ([top](#nextjs-aws-serverless-deploy))


First, install required packages:

```bash
yarn install
```

> **Note**: It is important to use `yarn` version 1.22.* to install dependencies because of the `yarn.lock` file. Using npm would likely result in errors.
>
> [_Installing Yarn_](https://classic.yarnpkg.com/lang/en/docs/install/#install-via-npm)

### Starting Dev server
```bash
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `pages/index.tsx`. The page auto-updates as you edit the file.

[API routes](https://nextjs.org/docs/api-routes/introduction) can be accessed on [http://localhost:3000/api/hello](http://localhost:3000/api/hello). This endpoint can be edited in `pages/api/hello.ts`.

The `pages/api` directory is mapped to `/api/*`. Files in this directory are treated as [API routes](https://nextjs.org/docs/api-routes/introduction) instead of React pages.

### Learn More About Next.js Framework 

Take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js/) - your feedback and contributions are welcome!

---


# Env Example
### ([top](#nextjs-aws-serverless-deploy))


The AWS CDK deployment depends on variables set in the `.env` file. Below are the required variables and example usages.

**These values are examples and should _NOT_ be used and will _NOT WORK_ in your deployed app**
```dotenv
# The AWS Account ID you want to create the deployment for.
AWS_ACCOUNT_ID=123456789012

# Preffered AWS Region for the Pipeline
AWS_REGION_DEFAULT=us-east-1

# Connection Arn for the Source Repository
# Docs for how to Setup: https://docs.aws.amazon.com/dtconsole/latest/userguide/connections-create.html
AWS_REPO_SOURCE_CONNECTION_ARN=arn:aws:codestar-connections:us-east-1:123456789012:connection/62ce35c0-6800-11ec-90d6-0242ac120003

# A string that encodes owner and repository separated by a slash (e.g. 'owner/repo').
STAGING_REPO_STRING=myUser/someRepoOfMine

# Branch name the will trigger CI/CD Pipeline
STAGING_SOURCE_BRANCH=develop
```

The next set of variables are optional. Not setting the domain variables will mean your app will only be available at the Secure CloudFront Url created automatically, for example: `http://d111111abcdef8.cloudfront.net`. You can then attach that value to a DNS record manually. Either in AWS or with another Registrar.

```dotenv
# Custom Domain name for Staging infrastructure. This will be used to create an A record in Route 53.
# You must have an existing hosted zone setup, see below for details.
STAGING_DOMAIN=staging.rea-dev.com
```
Requesting a Public Cert with AWS: https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request-public.html
```dotenv
# SSl Cretificate Arn for your custom Staging Domain
STAGING_DOMAIN_SSL_CERT_ARN=arn:aws:acm:us-east-1:123456789012:certificate/62ce35c0-6800-11ec-90d6-0242ac120003
```
Creating a Hosted Zone in AWS: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/CreatingHostedZone.html
```dotenv
# AWS Route 53 Hosted Zone ID
STAGING_HOSTED_ZONE_ID=SHETE57DONT9DWERK6U

# AWS Route 53 Host Zone Name, will most likly be your domain name, without any subdomain.
STAGING_DOMAIN_ZONE_NAME=mydomain.com
```

---


# Package Json Scripts
### ([top](#nextjs-aws-serverless-deploy))


This starter comes with predefined scripts intended to make life easier on the developer.
Each script is defined in the `package.json` and can be executed by `yarn`.

For example running `yarn format` will reformat your code per the ESlint settings, using `next lint --fix` under the hood.

Below are explanations for each script you'll find in the `package.json`.

#### Start development build of next js
>```json
>  "dev": "next dev"
>```
>Start a local/development build of your app. Useful while developing your application.

#### Create Production Build
>**_This will not be used when deployed, see [Deployment](#deployment) section for more details._**
>
>**_Helpful when testing issues with the CI/CD Pipeline_**
>```json
>  "build": "next build"
>```
>Create a standard NextJs build, see [CLI build](https://nextjs.org/docs/api-reference/cli#build) for more details.

#### Start production Build
>```json
>  "start": "next start"
>```
>Start a production build compiled by running `yarn build`.

#### Test Compiling Typescript
>```json
>  "type-check": "tsc --pretty --noEmit"
>```
>Compile the project code using [Typescript Compiler (TSC)](https://www.typescriptlang.org/docs/handbook/compiler-options.html), but **_do not_** make any changes to the code. Uses configuration options in `tsconfig.json`.

#### Check Linting per ESLint
>```json
>  "lint": "next lint"
>```
>Only check for potential changes based on settings in `eslint.json` & `eslintignore`. See the [Linting](#linting) section for more details.

#### Reformat code per ESLint
>```json
>  "format": "next lint --fix"
>```
>Reformat the project code based on settings in `eslint.json` & `eslintignore`. See the [Linting](#linting) section for more details.

#### Run Jest Tests
>```json
>  "test": "jest"
>```
>Execute the Jest tests defined in `test/` directory. See the [Testing](#testing) section for more details.

#### Update Jest Snapshots
>```json
>  "test-update": "jest --update-snapshot"
>```
>Update the Jest Snapshot test files used for comparison. See the [Testing](#testing) section for more details.

#### Run Jest Coverage Tests
>```json
>  "test-coverage": "jest --coverage"
>```
>See [Jest CLI coverage Flage](https://jestjs.io/docs/cli#--coverageboolean) for more details.

#### Check Linting, Types, & run all Jest Tests.
>```json
>  "test-all": "yarn lint && yarn type-check && yarn test"
>```
>Runs all Tests  for ESLint, TSC, and Jest. See [Linting](#Linting), [TSC](https://www.typescriptlang.org/docs/handbook/compiler-options.html), [Testing](#testing) for more details

#### Install Husky Git Hooks
>This should only be used when setting up the project for the first time.
>```json
>  "prepare": "husky install"
>```
>See the [Git Hooks](#git-hooks) section for more details.

#### Aws Cdk Deployments
>Read [Deployment](#deployment) section before using these commands.
>```json
>  "cdk": "cdk",
>  "deploy": "ts-node deploy/bin.ts"
>```
>Executing `yarn cdk ls` will build your app per the CDK specifications setup in the `deploy/` directory and list **Stack Names** once complete. Example output:
>```shell
>info  - Loaded env from /home/rae004/projects/nextjs-serverless-deploy/.env.local
>info  - Loaded env from /home/rae004/projects/nextjs-serverless-deploy/.env
>info  - Checking validity of types...
>info  - Creating an optimized production build...
>info  - Compiled successfully
>info  - Collecting page data...
>info  - Generating static pages (0/3)
>info  - Generating static pages (3/3)
>info  - Finalizing page optimization...
>
>Page                                       Size     First Load JS
>┌ ○ /                                      5.04 kB        76.7 kB
>├   └ css/b282b959608f58d4.css             718 B
>├   /_app                                  0 B            71.7 kB
>├ ○ /404                                   180 B          71.8 kB
>└ λ /api/hello                             0 B            71.7 kB
>+ First Load JS shared by all              71.7 kB
>  ├ chunks/framework-3ccec772d1fa8b6b.js   42.1 kB
>  ├ chunks/main-404bdd3acbdd01ab.js        28.3 kB
>  ├ chunks/pages/_app-fc81d34f352835e7.js  496 B
>  ├ chunks/webpack-27593bf8d60abcde.js     757 B
>  └ css/2974ebc8daa97b04.css               209 B
>
>λ  (Lambda)  server-side renders at runtime (uses getInitialProps or getServerSideProps)
>○  (Static)  automatically rendered as static HTML (uses no initial props)
>
>Done in 17.03s.
>nextjs-serverless-starter-staging-pipeline
>nextjs-serverless-starter-staging-pipeline/staging/serverless-rae-dev-next-js
>```
>
>See the [Deployment](#Deployment) section for more details.


# Testing
### ([top](#nextjs-aws-serverless-deploy))


This starter already has [Jest](https://jestjs.io/docs/) configured to use with  [NextJs](https://nextjs.org/docs/testing#setting-up-jest-with-the-rust-compiler). Run `yarn test` to execute the test files in the `test/` directory.

One simple [snapshot](https://jestjs.io/docs/snapshot-testing) test is configured by default, you can easily add more tests based on your apps specifications. This test will render the Home component and compare it against the saved snapshot.

```js
describe('Render Index Home Correctly', () => {
    let component: RenderResult;
    beforeEach(() => {
        component = render(<Home />, {});
    });

    it('should render the home page correctly', () => {
        expect(component).toMatchSnapshot();
    });
});
```
>**Note**: Snapshot files are stored in `<root>/test/jest/__snapshots__` and should not be modified directly.

Jest configuration options are set in `jest.config.js` in the root project directory. 

>* [Jest Framework Documentation](https://jestjs.io/docs/getting-started)
>* [Using Jest With NextJs](https://nextjs.org/docs/testing#creating-your-tests)
>* [Adding Cypress e2e testing](https://nextjs.org/docs/testing#cypress)

---


# Linting
### ([top](#nextjs-aws-serverless-deploy))


NextJs create app helper install ESLint by default. This starter extends  default settings, adds additional settings, plus configures the ESLint Prettier plugin for code styling.

>**Default Extended Rules Sets**:
>* eslint:recommended
>* plugin:react/recommended
>* plugin:@typescript-eslint/recommended
>* plugin:@next/next/recommended
>* prettier

>**Default Plugins Used**:
>* @typescript-eslint
>* prettier
>* react
>* react-hooks
>* jest
>* import

ESLint configuration options are set in `.eslintrc.json`.

>* [NextJs Config](https://nextjs.org/docs/basic-features/eslint#eslint-config)
>* [ESLint Rules](https://eslint.org/docs/rules/)
>* [Prettier Plugin Options](https://github.com/prettier/eslint-plugin-prettier#options)


# Git Hooks
### ([top](#nextjs-aws-serverless-deploy))


This starter comes with Git Hooks already configured using [Husky](https://www.npmjs.com/package/husky) package.

Commands ran before committing to repo.
> pre-commit: `yarn run test-all`

Commands ran before pushing to repo.
> pre-push: `yarn run type-check`

The `package.json` has a script called `prepare` intended to help [initialize Husky](https://typicode.github.io/husky/#/?id=manual). If you need to re-install or re-configure Husky, run `yarn prepare` to generate new Husky files.

See documentation for [Creating additional Git Hooks](https://typicode.github.io/husky/#/?id=create-a-hook).

# Deployment
### ([top](#nextjs-aws-serverless-deploy))

The AWS CDK allows you to deploy infrastructure as code. This starter deploys a NextJs application to AWS Lambda and S3, utilizing the Serverless NextJs CDK Construct. This does require an AWS account. You will also need to install and configure the AWS CLI in order to create the CI/CD Pipeline for the first time.