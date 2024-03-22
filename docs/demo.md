# Demo

Please visit [our demo page](https://example.runnel.run/) and play around.

## Specs

The demo page is composed with multiple Micro Frontend Applications. Runnel is integrated in every application.

| Application | Description                 | Topics subscribing  | Topics publishing |
| ----------- | --------------------------- | ------------------- | ----------------- |
| App 1       | Rendered in the top window. | "count", "fullName" | "count"           |
| App 2       | Rendered in the top window. | "count", "fullName" | "fullName"        |
| App 3       | Rendered in an iframe.      | "count", "fullName" | "count"           |

## Catch Errors in Runtime

The idea is from [@trutoo/event-bus](https://www.npmjs.com/package/@trutoo/event-bus). App 1 and App 2 each have an intentional error that you can observe in the browser console.

| Application | Type of Error                             |
| ----------- | ----------------------------------------- |
| App 1       | Wrong payload is about to be published.   |
| App 2       | Wrong schema is set to an existing topic. |

## Metrics

Runnel in App 3 uses [@runnel/metric-plugin](https://www.npmjs.com/package/@runnel/metric-plugin). When you click a button that triggers "publish" in any application, the metrics get updated.

The plugin starts collecting data when App 3 is mounted. Thus, events happened before that don't get collected. Below is the list of events happened before App 3 gets mounted.

- Declaration of subscribe for "count" in App 1
- Declaration of subscribe for "fullName" in App 1
- Declaration of subscribe for "count" in App 2
- Declaration of subscribe for "fullName" in App 2
