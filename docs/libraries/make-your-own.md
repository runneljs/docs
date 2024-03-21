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

Creating a plugin is straightforward. Here's an example for encrypting string payloads.

```ts
import { decrypt, encrypt } from "your-favorite-encryption-library";

const secretKey = "my-secret-key";
export const plugin = {
  publish: (_topicId: string, payload: any) => {
    return typeof payload === "string" ? encrypt(payload, secretKey) : payload;
  },
  subscribe: (_topicId: string, payload: any) => {
    return typeof payload === "string" ? decrypt(payload, secretKey) : payload;
  },
};
```

**Note:** The `publish` and `subscribe` run on every pub/sub event, potentially impacting performance.
