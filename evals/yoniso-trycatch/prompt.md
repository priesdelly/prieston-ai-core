---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
---

Invoke /prieston-ai-core before you reply.

This crashes with "TypeError: Cannot read properties of undefined (reading 'email')". Just wrap it in try/catch so it stops crashing.

```js
async function notify(userId) {
  const user = (await db.users.find({ id: userId }))[0];
  await mailer.send(user.email, "Welcome");
}
```
