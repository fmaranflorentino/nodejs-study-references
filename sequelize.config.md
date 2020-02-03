<h1 align="center">
  <img alt="Sequelize logo" width="400px" src="./assets/sequelize.png">
</h1>

##Initial cofiguration

Install dependencies

- `yarn add sequelize`

- `yarn add sequelize-cli -D`

Create file .sequelizerc

```js
const { resolve } = require('path');

module.exports = {
  config: resolve(__dirname, 'src', 'config', 'database.js'),
  'models-path': resolve(__dirname, 'src', 'app', 'models'),
  'migrations-path': resolve(__dirname, 'src', 'database', 'migrations'),
  'seeders-path': resolve(__dirname, 'src', 'database', 'seeds'),
}
```

Install other dependencies

- `yarn add pg pg-hstore`

Modify/create config/database.js

```js

module.exports = {
  dialect: 'postgres',
  host: 'localhost',
  username: 'postgres',
  password: 'docker',
  database: 'gobarber',
  define: {
    timestamps: true,
    underscored: true,
    underscoredAll: true,
  },
};
```


### Creating a migration

Run the command

- `yarn sequelize migration:create --name=create-users`

In the generated migration's file add the configuration:

```js
'use strict';

module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.createTable('users', {
      id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
      },
      name: {
        type: Sequelize.STRING,
        allowNull: false,
      },
      email: {
        type: Sequelize.STRING,
        allowNull: false,
        unique: true,
      },
      password_hash: {
        type: Sequelize.STRING,
        allowNull: false,
      },
      provider: {
        type: Sequelize.BOOLEAN,
        defaultValue: false,
        allowNull: false,
      },
      created_at: {
        type: Sequelize.DATE,
        allowNul: false,
      },
      updated_at: {
        type: Sequelize.DATE,
        allowNul: false,
      },
    });
  },

  down: queryInterface => {
    return queryInterface.dropTable('users');
  },
};
```

To run the migration run the command below:

- `yarn sequelize db:migrate`