# @runnel/metric-plugin

To verify publisher/subscriber calls, use [@runnel/metric-plugin](https://www.npmjs.com/package/@runnel/metric-plugin) which collects six types of data:

1. `schema` - The schema of the topic.
2. `onCreateTopic`: The number of the topic created.
3. `lastPayload`: The latest payload to the topic.
4. `onPostMessage`: The number of the topic published.
5. `onAddEventListener`: The number of the topic subscribed to.
6. `onRemoveEventListener`: The number of the topic unsubscribed from.

## Usage

```ts
import { createPlugin, type Metrics } from "@runnel/metric-plugin";
import { runnel } from "runneljs";

const { register, observer } = createPlugin(deepEqual);
const eventBus = runnel("my-event-bus", deepEqual, validator);
register();

...

// Example with React.useState
const [metrics, setMetrics] = useState<Metrics>();
observer.subscribe(setMetrics);
```

Find more usage example on the [Guides](../guides.md) page.

## Output Examples

### Case 1

- `topic1` with schema `{ "type": "number" }`.
- No subscribers.
- One publish event with payload `100`.

```json
{
  "topic1": {
    "schema": { "type": "number" },
    "onCreateTopic": 1,
    "lastPayload": 100,
    "onPostMessage": 1,
    "onAddEventListener": 0,
    "onRemoveEventListener": 0
  }
}
```

### Case 2

- `topic2` with schema `{ "type": "string" }`.
- One subscriber.
- No publish events.

```json
{
  "topic2": {
    "schema": { "type": "string" },
    "onCreateTopic": 1,
    "lastPayload": null,
    "onPostMessage": 0,
    "onAddEventListener": 1,
    "onRemoveEventListener": 0
  }
}
```
