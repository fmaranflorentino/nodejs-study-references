<h1 align="center">
  <img alt="Prettier logo" width="400px" src="./assets/prettier.png">
</h1>

## Initial configuration

Add the following dependencies:

- `yarn add prettier eslint-config-prettier eslint-plugin-prettier -D`

Add the configuration below to the .eslintrc

```js
module.exports = {
  env: {
    es6: true,
    node: true,
  },
  extends: [
    'airbnb-base', 'prettier'
  ],
  plugins: [ 'prettier' ],
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly',
  },
  parserOptions: {
    ecmaVersion: 2018,
    sourceType: 'module',
  },
  rules: {
    "prettier/prettier": "error",
    "class-methods-use-this": "off",
    "no-params-reassign": "off",
    "camelcase": "off",
    "no-unsed-vars": ["error", { "argsIgnorePattern": "import" }]
  },
};
```

Create file .prettierrc with the content below:

```json
{
  "singleQuote": true,
  "trailingComma": "es5"
}
```