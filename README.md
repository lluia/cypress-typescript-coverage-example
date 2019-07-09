# Cypress + Typescript App + Code Coverage ❤ ️️

This example uses [@cypress/code-coverage](https://github.com/cypress-io/code-coverage) plugin for [Cypress.io](https://www.cypress.io) test runner.

This repository aims [to document](https://github.com/cypress-io/code-coverage/issues/19) how to set up **code coverage** in Cypress against a Typescript app.

The tricky part of this particular setup is to configure **Istanbul** instrument your Typescript code.

See **[Cypress code coverage](https://on.cypress.io/code-coverage)** docs for more insight on how it works.

## Running the example 🏃🏻‍

First, make sure you have **[Yarn](https://yarnpkg.com/en/docs/install#mac-stable)** installed.

To run the example Typescript app:

```
yarn start
```

To run the Cypress test suite with code coverage:

```
CODE_COVERAGE=true yarn test
```

## How does it work 🤨

After installing the handy [`@cypress/code-coverage` plugin](https://docs.cypress.io/guides/tooling/code-coverage.html#Install-the-plugin), we will need to [instrument our compiled TS code](https://docs.cypress.io/guides/tooling/code-coverage.html#Instrumenting-code) that Cypress will load to run the tests against ( _see the "Instrumenting the code" section below_ ).

Once your instrumentation is set up, we need to install a few extra dependencies to make **Istanbul** understand your **Typescript** source files:

```
yarn add -D \
  @istanbuljs/nyc-config-typescript \
  source-map-support \
  ts-node
```

and make sure that Istanbul takes advantage of it by adding this configuration in your `package.json` or in `.nycrc.json`

```json
  "nyc": {
    "extends": "@istanbuljs/nyc-config-typescript",
    "all": true
  },
```

## Instrumenting the code 🎷

#### Babel

If you're compiling TS thought Babel, the easiest way is to use `babel-plugin-istanbul` which will instrument the compiled code through Babel for us. ( _this is the approach taken on this example repo_ ):

```
yarn add -D  babel-plugin-istanbul
```

#### Istanbul

In case you're compiling TS through the TSC, you'll need to manually make Istanbul instrument your TS source files and serve that to Cypress:

```
yarn nyc instrument --compact=false src/index.tsx instrumented
```
