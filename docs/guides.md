# Guides

## Writing Tests

We provide a plugin for testing publisher/subscriber mechanism, eliminating the need for mocks.

1. Install the plugin.
1. Create a test fixture.
1. Write tests.

### 1. Install the plugin

```sh
npm i -D @runnel/metric-plugin
```

### 2. Create a Test Fixture

This involves creating a separate event bus that interacts with the main event bus in your application.

```ts
// src/test-fixture/setup-reporter.ts
import { createPlugin } from "@runnel/metric-plugin"; // The new dependency
import { validator } from "@runnel/validator";
import deepEqual from "deep-equal";
import { createEventBus } from "runneljs";

export function setupReporter() {
  const { register, observer } = createPlugin(deepEqual); // Creates a plugin and an observer
  // Instantiate another event bus.
  const { registerTopic } = createEventBus({
    deepEqual,
    payloadValidator: validator,
  });
  register();
  return { observer }; // Expose the observer
}
```

### 3. Write Tests

Below is a test suite for a simple counter application. The application code is available at <a href="/getting-started/example-react/" target="_blank">Example in React</a>.

```ts
// src/App.test.tsx
import { fireEvent, render, screen, waitFor } from "@testing-library/react";
import App from "./App";
import { beforeAll, describe, expect, test, afterAll } from "@jest/globals";
import { setupReporter } from "./test-fixture/setup-reporter";

const { observer } = setupReporter();

describe("App", () => {
  let report: any = {};
  beforeAll(() => {
    // Observe the event bus
    observer.subscribe((data) => {
      report = data;
    });
  });

  afterAll(() => {
    report = {};
  });

  test("counter increments when button is clicked", async () => {
    render(<App />);
    const button = screen.getByText("count is 0");
    fireEvent.click(button);

    await waitFor(() => {
      expect(screen.getByText("count is 1")).toBeTruthy();
    });
  });

  test("verifies expected publish/subscribe events", () => {
    expect(report).toEqual({
      count: { // "count" topic
        onCreateSubscribe: 1, // One "subscribe" action in the code
        onCreatePublish: 1, // One "publish" action in the code
        subscribe: 1, // Payloads received by subscribers
        publish: 1 // Payloads published
      },
    });
  });
});
```
