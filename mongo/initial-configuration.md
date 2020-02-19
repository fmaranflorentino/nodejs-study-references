- `docker run --name mongobarber -p 27017:27017 -d -t mongo`

- `yarn add mongoose`

```js
import mongoose from "mongoose";

this.mongoConnection = mongoose.connect("mongodb://localhost:27017/gobarber", {
  useNewUrlParser: true,
  useFindAndModify: true,
  useUnifiedTopology: true
});
```
