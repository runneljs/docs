# Installation & Basic Usage

## Preparation

`runnel` requires **two dependencies** to be installed.

### Dependency #1: Deep Equal

The first dependency is used to compare two objects and requires the following interface:

```ts
interface DeepEqual {
  (a: object, b, object): boolean;
}
```

[deep-equal](https://www.npmjs.com/package/deep-equal) and [lodash.isequal](https://www.npmjs.com/package/lodash.isequal) are good libraries for this.

### Dependency #2: JSON Schema Validator

The second dependency validates payloads against a JSON Schema. The interface is as follows:

```ts
interface Validator {
  (jsonSchema: object): (payload?: unknown) => boolean;
}
```

You can use the package [@runnel/validator](https://www.npmjs.com/package/@runnel/validator) for this purpose.

Please read [Make Your Own Validator](/libraries/make-your-own/#validator) if you need a customized validator.

## Installation

Below is an installation example using [deep-equal](https://www.npmjs.com/package/deep-equal).

```sh
npm install runneljs @runnel/validator deep-equal
```

## Usage

1. Initialize an event bus with `createEventBus` which returns `registerTopic`.

```ts
import deepEqual from "deep-equal";
import { createEventBus } from "runneljs";
import { validator } from "@runnel/validator";

const { registerTopic } = createEventBus({
  deepEqual,
  payloadValidator: validator,
});
```

2. Create an event topic.

```ts
const exampleTopic = registerTopic<string>("message", {
  type: "string",
});
```

3. Publish and subscribe to events using the topic.

```ts
exampleTopic.publish("Hello World");
```

```ts
exampleTopic.subscribe((payload: string) => alert(`Received [${payload}]`));
```
