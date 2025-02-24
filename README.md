# Star-Events

## Events Emitter Usage

```typescript
import { createEventEmitter } from './utils/createEventEmitter';

// Create an event emitter instance
const emitter = createEventEmitter();

// Subscribe to events
emitter.on('userLoggedIn', (user) => {
  console.log('User logged in:', user);
});

// Subscribe to all events using wildcard
emitter.on('*', (data) => {
  console.log('Any event occurred:', data);
});

// Emit events
emitter.emit('userLoggedIn', { id: 1, name: 'John' });

// Unsubscribe from events
const callback = (data) => console.log(data);
emitter.on('notification', callback);
emitter.off('notification', callback);

// Remove all callbacks for an event
emitter.offAll('notification');
```

## Custom Logging

```typescript
import { createLogger } from './utils/logger';
import { createEventEmitter } from './utils/createEventEmitter';

// Create a logger with custom settings
const logger = createLogger({
  LOGGER_DEBUG: true, // Enable debug logs
  LOGGER_LOG: true    // Enable warning and error logs
});

// Create an event emitter with custom logger
const emitter = createEventEmitter({ logger });

// Now all events will be logged according to your settings
emitter.emit('test', 'Hello, World!');
// [2023-07-20T10:00:00.000Z] [LOG] Emitting event "test" with params: Hello, World!
```

## Error Handling

The event emitter automatically catches and logs errors in event handlers:

```typescript
const emitter = createEventEmitter();

// This handler will throw an error
emitter.on('buggy', () => {
  throw new Error('Something went wrong!');
});

// The error will be caught and logged, other handlers will still execute
emitter.emit('buggy');
```

## Type Safety

TypeScript interfaces ensure type safety:

```typescript
interface UserEvents {
  userLoggedIn: { id: number; name: string };
  userLoggedOut: { id: number };
}

// Create a type-safe event emitter
const emitter = createEventEmitter<UserEvents>();

// TypeScript will ensure correct event names and payload types
emitter.on('userLoggedIn', (user) => {
  console.log(user.name); // TypeScript knows `name` exists
});

// This would cause a type error
emitter.emit('userLoggedIn', { id: 1 }); // Error: missing 'name' property
```
