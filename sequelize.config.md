<h1 align="center">
  <img alt="Sequelize logo" width="400px" src="./assets/sequelize.png">
</h1>

<p> A guide to configure e execute migrations with sequelize </p>

## Initial cofiguration

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


## Creating a Model

```js
import Sequelize, { Model } from 'sequelize';

class User extends Model {
  static init(sequelize) {
    super.init(
      {
        name: Sequelize.STRING,
        email: Sequelize.STRING,
        password_hash: Sequelize.STRING,
        provider: Sequelize.BOOLEAN,
      },
      {
        sequelize,
      }
    );
  }
}

export default User;
```

## Connecting a model and insert example

On database/ create a index.js

```js
import Sequelize from 'sequelize';

import User from '../app/models/User';

import databaseConfig from '../config/database';

const models = [User];

class Database {
  constructor() {
    this.init();
  }

  init() {
    this.connection = new Sequelize(databaseConfig);

    models.map(model => model.init(this.connection));
  }
}

export default new Database();

```

On the app.js file import the database config

```js
import './database';
```

Below a example of connection

```js
const user = await User.create({
    name: 'Flávio Florentino',
    email: 'florentino.flavio@hotmail.com',
    password_hash: '348927472',
});
```

### Adding a new column to a existing table

<p>First you need to create another migration, follow the example below:</p>

- `yarn sequelize migration:create --name=add-img-to-user`

<p>
After that, you need to cofigure your new migration, follow the configuration below:
</p>

```js
'use strict';

module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.addColumn('TABLE_NAME', 'NEW_COLUMN_NAME', {
      type: Sequelize.INTEGER,
      references: { model: 'MODEL_TO_BE_REFERENCED', key: 'id' },
      onUpdate: 'CASCADE',
      onDelete: 'SET NULL',
      allowNull: true,
    });
  },

  down: queryInterface => {
    return queryInterface.removeColumn('TABLE_NAME', 'NEW_COLUMN_NAME');
  },
};
```