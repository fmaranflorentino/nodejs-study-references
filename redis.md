- `docker run --name redisbarber -p 6379:6379 -d -t redis:alpine`

- `yarn add bee-queue`

<p>Create Queue.js file at config folder.</p>

```js
import Bee from "bee-queue";
import CancellationMail from "../app/jobs/CancellationMail";
import redisConfig from "../config/redis";

const jobs = [CancellationMail];

class Queue {
  constructor() {
    this.queues = [];

    this.init();
  }

  init() {
    jobs.forEach(({ key, handle }) => {
      this.queues[key] = {
        bee: new Bee(key, {
          redis: { redisConfig }
        }),
        handle
      };
    });
  }
}

export default new Queue();
```

Create a redis.js file at the config folder

```js
export default {
  host: "127.0.0.1",
  port: 6379
};
```

Create e new job in jobs folder

```js
import Bee from "bee-queue";
import CancellationMail from "../app/jobs/CancellationMail";
import redisConfig from "../config/redis";

const jobs = [CancellationMail];

class Queue {
  constructor() {
    this.queues = [];

    this.init();
  }

  init() {
    jobs.forEach(({ key, handle }) => {
      this.queues[key] = {
        bee: new Bee(key, {
          redis: { redisConfig }
        }),
        handle
      };
    });
  }

  add(queue, job) {
    return this.queues[queue].bee.createJob(job).save();
  }

  processQueue() {
    jobs.forEach(job => {
      const { bee, handle } = this.queues[job.key];

      bee.process(handle);
    });
  }
}

export default new Queue();
```

```js
import { format, parseISO } from "date-fns";
import pt from "date-fns/locale/pt";
import Mail from "../../lib/Mail";

class CancellationMail {
  get key() {
    return "CancellationMail";
  }

  async handle({ data }) {
    const { appointment } = data;

    await Mail.sendMail({
      to: `${appointment.provider.name} <${appointment.provider.email}>`,
      subject: "Agendamento Cancelado",
      template: "cancellation",
      context: {
        provider: appointment.provider.name,
        user: appointment.user.name,
        date: format(
          parseISO(appointment.date),
          "'dia' dd 'de' MMMM', às' H:mm'h'",
          {
            locale: pt
          }
        )
      }
    });
  }
}

export default new CancellationMail();
```

<p>queue.js</p>

```js
import Queue from "./lib/Queue";

Queue.processQueue();
```

```js
await Queue.add(CancellationMail.key, { appointment });
```
