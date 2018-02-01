## Events

Events come in two types -- `Hook` and `Dock`. Hooks are intended for general, all-purpose event handling, and Docks are intended for front-end use.

## Hooks

The way hooks work is pretty straightforward. You can 'hook into' an event and you can 'trigger' an event.
Payloads are passed through callback functions, and in cases where the payload is not an object, needs to be passed by reference if you wish for it to be influenced.

```php
$hook = \Parable\DI\Container::get(\Parable\Event\Hook::class);

$hook->into("this_is_an_event", function($event, &$payload) {
    $payload = "<p>" . $payload . "</p>";
});

$string = "paragraph";

$hook->trigger("this_is_an_event", $string);

echo $string // outputs '<p>paragraph</p>'
```

Obviously this is more useful to trigger certain actions like logging events, writing data to a database or even influencing the response. A common way of using Hooks is to combine them with [Inits](inits) to add a header and footer to a website.

## Docks

Docks are generally more useful for when you want to offer front-end areas where extensions can add output to. Say you're building a forum package, and you want extensions to add functionality to a Dock called "post-sidebar". Simply trigger the event in the front-end and any code docking into it will have a chance to output their own logic.

Docks therefore support templates in addition to the same values Docks offer.

```php
$this->dock->into('test_dock_into', function ($event, &$payload)) {
    echo "Hello";
}, $this->testPath->getDir('dock_template.phtml'));

$this->dock->trigger('test_dock_into');
```

This will load and parse the `dock_template.phtml` file, which can contain php and any other Parable-related logic you want.

## Built-in Parable events

There are a number of events Parable itself will trigger. The `\Parable\Framework\App` class will throw the following:

- `\Parable\Framework\App::HOOK_HTTP_404`
- `\Parable\Framework\App::HOOK_HTTP_200`
- `\Parable\Framework\App::HOOK_INIT_DATABASE_BEFORE`
- `\Parable\Framework\App::HOOK_INIT_DATABASE_AFTER`
- `\Parable\Framework\App::HOOK_LOAD_INITS_AFTER`
- `\Parable\Framework\App::HOOK_LOAD_ROUTES_BEFORE`
- `\Parable\Framework\App::HOOK_LOAD_ROUTES_NO_ROUTES_FOUND`
- `\Parable\Framework\App::HOOK_LOAD_ROUTES_AFTER`
- `\Parable\Framework\App::HOOK_RESPONSE_SEND`
- `\Parable\Framework\App::HOOK_ROUTE_MATCH_BEFORE`
- `\Parable\Framework\App::HOOK_ROUTE_MATCH_AFTER`
- `\Parable\Framework\App::HOOK_SESSION_START_BEFORE`
- `\Parable\Framework\App::HOOK_SESSION_START_AFTER`

And the `\Parable\Framework\Dispatcher` class the following:

- `\Parable\Framework\Dispatcher::HOOK_DISPATCH_BEFORE`
- `\Parable\Framework\Dispatcher::HOOK_DISPATCH_AFTER`
- `\Parable\Framework\Dispatcher::HOOK_DISPATCH_TEMPLATE_BEFORE`
- `\Parable\Framework\Dispatcher::HOOK_DISPATCH_TEMPLATE_AFTER`

## Custom events

You can, of course, trigger any kind of event anywhere, so you can hook or dock into them as well.

```php
$hook->into("I just want to have an event all of my own!", function($event, &$payload) {
    echo "Triggered!";
});
$hook->trigger("I just want to have an event all of my own!");
```

Will echo `Triggered!`.