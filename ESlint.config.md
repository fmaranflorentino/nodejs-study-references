<h1 align="center">
  <img alt="Eslint logo" width="400px" src="./assets/eslint.png">
</h1>

<p> A guide to configure ESlint </p>

## Initial configuration

Install the following dependency:

- `yarn add eslint -D`

Run the following command:

- `yarn eslint init`

The file .eslintrc.js will be generated

To automatic fixes on save 

```json
  "[javascript]": {
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    }
  },
  "[javascriptreact]": {
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    }
  },
```

Add custon rules for .eslintrc.js

```js
module.exports = {
  env: {
    es6: true,
    node: true,
  },
  extends: [
    'airbnb-base',
  ],
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly',
  },
  parserOptions: {
    ecmaVersion: 2018,
    sourceType: 'module',
  },
  rules: {
    "class-methods-use-this": "off",
    "no-params-reassign": "off",
    "camelcase": "off",
    "no-unsed-vars": ["error", { "argsIgnorePattern": "next" }]
  },
};
```
To run an scan and fix all your files run the following command:

- `yarn eslint --fix src --ext .js`
