# Make Your Own Validator/Plugin

## Validator

You may sometimes need to create a custom validator. Below is an example using [@cfworker/json-schema](https://github.com/cfworker/cfworker/blob/main/packages/json-schema/README.md).

```ts
import { Validator } from "@cfworker/json-schema";

export function validator(jsonSchema: object): (payload?: unknown) => boolean {
  // When schema is `{}`, the payload must be undefined or null.
  if (Object.keys(jsonSchema).length === 0)
    return (payload?: unknown) =>
      typeof payload === "undefined" || payload === null;

  // Otherwise, validate the payload using the schema.
  const validator = new Validator(jsonSchema);
  return function (payload?: unknown) {
    return validator.validate(payload).valid;
  };
}
```

You can create your own validator using [@cfworker/json-schema](https://github.com/cfworker/cfworker/blob/main/packages/json-schema/README.md) or [ajv](https://www.npmjs.com/package/ajv).

## Plugin

Runnel dispatches CustomEvents to the `window.top` or `window` object. You can create your own plugin to observe these events.

### Custom Events

| Event Name                        | Triggered when...             |
| --------------------------------- | ----------------------------- |
| `runnel:on-create-topic`          | A topic is created.           |
| `runnel:on-post-message`          | A topic is published.         |
| `runnel:on-add-event-listener`    | A topic is subscribed to.     |
| `runnel:on-remove-event-listener` | A topic is unsubscribed from. |
