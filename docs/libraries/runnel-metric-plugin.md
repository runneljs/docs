# @runnel/metric-plugin

To verify publisher/subscriber calls, use [@runnel/metric-plugin](https://www.npmjs.com/package/@runnel/metric-plugin) which collects five types of data:

1. `onPublishCreated` - Number of `topic.publish()` executions.
1. `onSubscribeCreated` - Number of `topic.subscribe()` declarations.
1. `onPublish` - The latest payload to the topic.
1. `onSubscribe` - The latest payload from the topic.

## Usage

```ts
import {createPlugin, type Metrics} from "@runnel/metric-plugin";
import {createEventBus} from "runneljs";

const { register, observer } = createPlugin(deepEqual);
const eventBus = createEventBus({
  deepEqual,
  payloadValidator,
});
register();

...

// Example with React.useState
const [metrics, setMetrics] = useState();
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
    "onPublishCreated": 1,
    "onPublish": 100,
    "onSubscribeCreated": 0,
    "onSubscribe": null
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
    "onPublishCreated": 0,
    "onPublish": null,
    "onSubscribeCreated": 1,
    "onSubscribe": null
  }
}
```
