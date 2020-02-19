```js
await Notification.create({
  content: `Novo agendamento de ${user.name} para ${formattedDate}`,
  user: provider_id
});
```
