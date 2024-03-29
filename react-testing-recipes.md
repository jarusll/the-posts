---
title: React Testing Recipes
author: ["Suraj Yadav"]
date: 2023-02-11
tags: ["recipe"]
draft: false
layout: ../../layouts/RequestResponse.astro
---

## You want to setup jest
### A minimal config

*jest.config.js*
```javascript
module.exports = {
 transform: {
   "^.+\\.jsx?$": `<rootDir>/jest-preprocess.js`,
 },
 moduleNameMapper: {
   '^.+\\.(css|style|less|sass|scss)$': 'identity-obj-proxy',
 },
 transformIgnorePatterns: [ '!node_modules/',],
 testEnvironmentOptions: {
   url: `http://localhost`,
 },
}
```
#### Further Reading
- Jest configuration docs
  - https://jestjs.io/docs/configuration
- `identity-obj-proxy` docs
  - https://github.com/keyz/identity-obj-proxy


*jest.preprocess.js*
```javascript
const babelOptions = {
  // your babel config goes here
};

module.exports = require('babel-jest').default.createTransformer(babelOptions);
```
#### Further Reading
- Babel docs
  - https://babeljs.io/docs/en/presets/
  - https://babeljs.io/docs/en/plugins

### You want coverage reports
*jest.config.js*
```javascript
module.exports = {
...
 collectCoverage: true,
 coverageDirectory: 'coverage',
 reporters: ['default', ['jest-junit', { outputDirectory: 'reports' }]],
...
}
```
#### Further reading
- Reporters
  - https://jestjs.io/docs/configuration#reporters-arraymodulename--modulename-options

### You want to enable/add path alias support
*jest.config.js*
```javascript
const { pathsToModuleNameMapper } = require('ts-jest');
const { compilerOptions } = require('./jsconfig.json'); // or tsconfig.json

const paths = pathsToModuleNameMapper(compilerOptions.paths, {
  prefix: '<rootDir>/',
});

module.exports = {
...
    moduleNameMapper: {
        ...paths,
        // other path aliases you want to add manually
    }
    ...
}
```

### Your project uses other file extensions ie svg/gql/...
You need transformers
Here's how you would add transformers for `.gql` & `.svg` files. You will need to add them as dev dependencies.

*jest.config.js*
```javascript
module.exports = {
...
  transform: {
    '\\.(gql|graphql)$': '@graphql-tools/jest-transform',
    '^.+\\.svg$': 'jest-transformer-svg',
    ...
  },
...}
```
#### Further Reading
- Transformers
  - https://jestjs.io/docs/code-transformation

### Setup file
- Why you need this?
  - When you have Provider wrappers in your app ie `SnackbarProvider`, `ThemeProviders`, you might need to setup a custom render function which automatically wraps your Component to be tested with the necessary wrappers.

*jest.setup.js*
```javascript
import '@testing-library/jest-dom';
import '@testing-library/jest-dom/extend-expect'; // needed for assertions
import React from 'react';
import { render } from '@testing-library/react';
// import all your Providers here

const AllTheProviders = ({ children }) => {
  return <Provider>{children}</Provider>;
};

const customRender = (ui, options) =>
  render(ui, { wrapper: AllTheProviders, ...options });

export * from '@testing-library/react';

export { customRender as render };
```

## You want to run jest
```bash
npx jest
```

Add it as a script to `package.json`
*package.json*
```json
{
  "scripts": {
    "test": "jest"
  }
}
```
### Further Reading
- Jest CLI options
  - https://jestjs.io/docs/cli

## You want to see how tests are written?
- [Codevolution](https://www.youtube.com/@Codevolution)
  - [React Testing Tutorial](https://www.youtube.com/watch?v=T2sv8jXoP4s&list=PLC3y8-rFHvwirqe1KHFCHJ0RqNuN61SJd)
- [The Net Ninja](https://www.youtube.com/@NetNinja) & [Laith Academy](https://www.youtube.com/channel/UCyLNhHSiEVkVwPSFKxJAfSA)
  - [React Testing Library Tutorial](https://www.youtube.com/watch?v=7dTTFW7yACQ&list=PL4cUxeGkcC9gm4_-5UsNmLqMosM-dzuvQ)


## While testing
### You want to test Apollo graphql
- Here's how to set it up
  - https://www.apollographql.com/docs/react/development-testing/testing/
- Here's a really neat way to set it up along with preliminary debugging
  - https://www.swarmia.com/blog/debugging-apollo-graphql-mockedprovider/
- Is there any way to make this easier?
  - I pass my `mocks` as second argument to the `render` function
  - I have a utility to create the mock request-response entity
  - Here's how it looks like
    ```javascript
    const {...} = render(<Component {...props}/>, MakeResponses({
      'query-name': {
        params: // query params go here,
        data: // response goes here
      },
      ...
    }))
    ```

### You want to simulate browser events - Click/Change/...
- https://www.pluralsight.com/guides/simulate-browser-events-react-react-testing-library

### You are facing `SOME_ENV_VAR` not defined
You can define global variables by
*jest.config.js*
```javascript
module.exports = {
  ...,
  globals: {
    ENV_VAR_NAME: VALUE
  },
  ...
}
```

#### Further Reading
- Globals Object
  - https://jestjs.io/docs/configuration#globals-object

## You want to mock

### You want to mock a npm dependency
If the dependency is a commonjs module, you can do
```javascript
jest.mock('dependency', () => ({
  default: jest.fn(),
}));
```
Assuming the dependency is exported as default

- For named exports, you can do

```javascript
jest.mock('dependency', () => ({
  namedExportedFunction: jest.fn(),
}));
```

- If the dependency is es6 module, you can do

```javascript
jest.mock('dependency', () => ({
  __esModule: true,
  default: jest.fn(),
  namedExportedFunction: jest.fn(),
}));
```

#### Further Reading
- Jest Mocks
  - https://jestjs.io/docs/mock-functions
  - https://medium.com/@rickhanlonii/understanding-jest-mocks-f0046c68e53c

### You want to mock a Component
For example,
You want to mock Route to bypass authentication.
Assuming you have the following folder structure and Route is the default export
```
...
|-Route
  |- index.js
```
You can add the Route mock at `Route/__mocks__/index.js`

*Route/__mocks__/index.js*
```javascript
function MockRoute({children}) {
  return (
    <>{children}</>
  );
};
```

Route can be automatically mocked by placing the following snippet in your test file
```javascript
jest.mock('path/to/Route');
```

### You want to mock a Context
AuthProvider has the following state
```javascript
const AuthContext = React.createContext({
  user: null,
  login: () => {},
  logout: () => {},
});
```

You can mock AuthProvider by
*AuthProvider/__mocks__/index.js*
```javascript
const AuthContext = React.createContext();
const AuthState = {
  user: null,
  login: jest.fn(),
  logout: jest.fn(),
};

export default function AuthProvider(){
  return (<AuthContext.Provider value={{
    user: null,
    login: () => {},
    logout: () => {},
  }}>
    {children}
  </AuthContext.Provider>);
}
```
