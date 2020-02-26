## Creating Mail notification locally with nodemailerjs

- `yarn add nodemailer`

<p>Create a main.js file on config folder.</p>

<p>You need to creater a account in mailtrap to use email feature in DEV mode.</p>

```js
export default {
  host: "smtp.mailtrap.io",
  port: "2525",
  secure: false,
  auth: {
    user: "08493398cebfae",
    pass: "0112c1369075e6"
  },
  default: {
    from: "Go baraber - FMF"
  }
};
```

<p>Create a lib folder to store external configuration </p>

<p>Create a Mail.js file </p>

```js
import nodemailer from "nodemailer";
import mailConfig from "../config/mail";

class Mail {
  contructor() {
    const { host, port, secure, auth } = mailConfig;

    this.transporter = nodemailer.createTransport({
      host,
      port,
      secure,
      auth: auth.user ? auth : null
    });
  }

  sendMail(message) {
    return this.transporter.sendMail({
      ...mailConfig.default,
      ...message
    });
  }
}

export default new Mail();
```

<p>Send email </p>

```js
import Mail from "../../lib/Mail";
```

```js
await Mail.sendMail({
  to: `Name`,
  subject: "Subject",
  text: "Message text"
});
```
