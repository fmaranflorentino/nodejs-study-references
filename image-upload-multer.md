## Creating a simple fileupload with multerjs

- `yarn add multer`

<p>In your configuration folder create a multer.js file, with the configuration below:</p>

```js
import multer from 'multer';
import crypto from 'crypto';
import { extname, resolve } from 'path';

export default {
  storage: multer.diskStorage({
    destination: resolve(__dirname, '..', '..', 'tmp', 'uploads'),
    filename: (req, file, cb) => {
      crypto.randomBytes(16, (err, res) => {
        if (err) return cb(err);

        return cb(null, res.toString('hex') + extname(file.originalname));
      });
    },
  }),
};
```

<p>In your routes file import the following configs:</p>

```js
import multer from 'multer';
import multerConfig from './config/multer';
```

<p>After that, create e const referincing multer:</p>

```js
const upload = multer(multerConfig);
```

<p>To test your multer configuration you can create a simple route:</p>

```js
routes.post('/file', upload.single('file'), (req, res) => {
  return res.json({ ok: true });
});
```
