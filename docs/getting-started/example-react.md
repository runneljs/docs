# Example in React

When you have two micro-frontend applications each contain the following files, each app has a button. Whichever the button you click, the button label of both apps get updated.

## `event-bus.ts`

```ts
import deepEqual from "deep-equal";
import { createEventBus } from "runneljs";
import { validator } from "@runnel/validator";

const { registerTopic } = createEventBus({
  deepEqual,
  payloadValidator: validator,
});
export { registerTopic };
```

## `App.tsx`

```ts
import { useEffect, useState } from "react";
import { registerTopic } from "./path-to/event-bus";

export default function App() {
  const [count, setCount] = useState(0);
  const countTopic = registerTopic<number>("count", {
    type: "number",
  });
  useEffect(() => {
    const unsubscribe = countTopic.subscribe(setCount);
    return () => unsubscribe();
  }, []);

  const clickHandler = () => {
    countTopic.publish(count + 1);
  };

  return (
    <>
      <h1>App</h1>
      <div className="card">
        <button onClick={clickHandler}>count is {count}</button>
      </div>
    </>
  );
}
```
