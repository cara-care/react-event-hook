# react-event-hook

A library for emitting and listening to events in a React application. 

The idea for this package came from [a Tweet](https://twitter.com/pedronauck/status/1502792417761800193?s=20&t=PFC7inszDOUhRFf7se88UA) by [@pedronauck](https://github.com/pedronauck).

[![Build status](https://github.com/etienne-martin/react-event-hook/workflows/Build/badge.svg)](https://github.com/etienne-martin/react-event-hook/actions)
[![Coveralls github](https://img.shields.io/coveralls/github/etienne-martin/react-event-hook.svg)](https://coveralls.io/github/etienne-martin/react-event-hook)
[![npm monthly downloads](https://img.shields.io/npm/dm/react-event-hook.svg)](https://www.npmjs.com/package/react-event-hook)

## Installation

To use react-event-hook in your project, run:

```shell script
yarn add react-event-hook
```

## Creating events

Events can be declared using the `createEvent` function. This function only takes the name of the event as an argument. It can be whatever you want. The result will be an object containing two functions, a listener and an emitter. Their name will automatically be derived from the name that was chosen for the event.

```javascript
import { createEvent } from "react-event-hook";

const { usePingListener, emitPing } = createEvent("ping")();
const { usePongListener, emitPong } = createEvent("pong")();
```

Please note that since events are global, they should only be created once. Conflicting event names can cause problems if their associated payload differs. Make sure to call the `createEvent` function only once per event and reuse the resulting functions throughout your application.

### Cross-tab events

Events can also extend to other tabs that share the same origin by enabling the `crossTab` option. This can be used to propagate changes locally between multiple instances of an application.

```javascript
import { createEvent } from "react-event-hook";

const { useSignInListener, emitSignIn } = createEvent("sign-in")({
  crossTab: true
});
```

## Listening for events

Events can be listened to using the listener function returned by `createEvent`. Listeners come in the form of a custom React hook.

```jsx
import { useMessageListener } from "./events";

const ListenerComponent = () => {
  useMessageListener((message) => {
    console.log("Received a message:", message);
  });

  return <>...</>;
};
```

## Emitting events

Events can be emitted from anywhere in your application using the emitter function.

```jsx
import { emitMessage } from "./events";

const EmitterComponent = () => (
  <button onClick={() => emitMessage("hello")}>Send Message</button>
);
```

When a cross-tab event is emitted, its payload is first serialized using `JSON.stringify`. If a payload contains values that cannot be converted to JSON, an error will be thrown and the event won't be delivered. Cross-tab payloads can contain any of the following value types: arrays, objects, or primitives (strings, numbers, booleans, null, undefined).

## TypeScript

This library is written in TypeScript to ensure type safety. It requires TypeScript v4.1 or greater due to its use of [Key Remapping](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html#key-remapping-via-as) and [Template Literal Types](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html).

### Typing events

Events can be typed using the following syntax:

```typescript
import { createEvent } from "react-event-hook";

interface Message {
  subject: string;
  body: string;
}

const { emitMessage } = createEvent("message")<Message>();

emitMessage({
  subject: "greeting",
  body: "hello"
});
```

## Contributing

When contributing to this project, please first discuss the change you wish to make via a [GitHub issue](https://github.com/etienne-martin/react-event-hook/issues/new).

Run `yarn test` and update the tests if needed.

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/etienne-martin/react-event-hook/blob/main/LICENSE) file for details.
