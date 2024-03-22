# @runnel/metric-plugin

To verify publisher/subscriber calls, use [@runnel/metric-plugin](https://www.npmjs.com/package/@runnel/metric-plugin) which collects five types of data:

1. `onCreatePublish` - Number of `topic.publish()` executions.
1. `onCreateSubscribe` - Number of `topic.subscribe()` declarations.
1. `schema` - The topic's schema. Object.
1. `publish` - Array of published payloads.
1. `subscribe` - Array of payloads received by subscribers. In cases of multiple subscribers, a payload is counted per subscriber.

## Usage

```ts
const { plugin, subscribe } = createPlugin(deepEqual);
const eventBus = createEventBus({
  deepEqual,
  payloadValidator,
  pluginMap: new Map([[window, [metricPlugin]]]), // To observe the window. If the `scope` is smaller than the specified plugin scope, the specified plugin will not function.
});

...

// Example with React.useState
const [metrics, setMetrics] = useState();
subscribe(setMetrics);
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
    "onCreatePublish": 1,
    "publish": [100],
    "onCreateSubscribe": 0,
    "subscribe": [],
    "schema": { "type": "number" }
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
    "onCreatePublish": 0,
    "publish": [],
    "onCreateSubscribe": 1,
    "subscribe": [],
    "schema": { "type": "string" }
  }
}
```
