# Socarrat Events

The Socarrat event library contains the base for your event dispatchers. It aims to be **quick** and **simple**, as well as **flexible** and **extensible** at the same time.

## Installation

You can install this package through [Composer](https://getcomposer.org/) by running `composer require socarrat/events`. The library will be available under the `Socarrat\Events` namespace.

## Concept

The thing this package is all about, are _events_.

Imagine you're on a little boat at open sea, and you have no idea where you are. Luckily, you have some [flares](https://en.wikipedia.org/wiki/Flare#Military_use). They have been seen by a nearby ship, and you're rescued.

This getting-lost is the mehaphor for an _event_: some kind of situation arises. When you lightened the flares, you fired or _dispatched_ the event. Thanks to hooks or _event listeners_ that have been set, the smoke has been noticed. The action (rescue) is executed when the _callback_ of the event listener (the person who had saw you calling for help) is executed.

## Usage

### Creating an event

In order to create an event, you need to extend the abstract `Event` class. It's important that you override the `$listeners` property.

For example:

```php
<?php
use Socarrat\Events\Event;

class YourCustomEvent extends Event {
	static protected array $listeners;
}
```

Nothing more is needed! Of course, you can extend the event to have custom methods and properties.

### Listening to the event

To register an event listener, you have to call the `on` method. The closure is called every time the event is dispatched.

The first parameter is the sort order. Hooks with a lower order are called earlier. This method returns the index that has been assigned to the hook. It may be higher than the index you provided. In that case, the previous index/es were alreaty occupied.

```php
YourCustomEvent::on(1, function() {
	echo "Hi!";
});
```

### Dispatching the event

Call the `dispatch` method to dispatch an event:

```php
YourCustomEvent::dispatch();
```

The parameters you pass to this method are passed to all event listeners, so that you can provide context-specific information.

## API

### `abstract class Socarrat\Events\Event`

#### `static protected $listeners`

An array of event listeners (closures) to call when the event is dispatched.

It's important that you override this in your own implementation!

#### `static function getName(): string`

Returns the name of the event.

#### `static function on(int $priority, $callback): int`

Registers an event listener for the current event. Returns the index that has been assigned to the listener.

| Parameter name | Type      | Default value | Description                                      |
|----------------|-----------|---------------|--------------------------------------------------|
| `$priority`    | `int`     | -             | The priority of this listener.                   |
| `$callback`    | `closure` | -             | The function to execute when the event is fired. |

#### `static function dispatch(...$callbackData)`

Dispatches the event. This executes all registered callbacks in order, passing optional callback data to them.

**Warning:** this is a blocking function; it will stop execution until all callbacks have finished. Callbacks are executed synchronously.

| Parameter name        | Type    | Default value | Description                              |
|-----------------------|---------|---------------|------------------------------------------|
| `...$callbackData`    | `mixed` | -             | Data to pass to the callbacks. Optional. |

## Copyright

(c) 2023 Romein van Buren. Licensed under the MIT license.

For the full copyright and license information, please view the [`license.md`](./license.md) file that was distributed with this source code.
