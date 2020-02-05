## Initial cofiguration

Install dependencies

- `yarn add bcryptjs`

To generate a bcrypt hash you can follow the example below:

```js

import bcrypt from 'bcryptjs';

const generateHash = (password) => bcrypt.hash(password, 8);

const passwordHash = generateHash('foo123');

```