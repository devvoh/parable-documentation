## Simple Routing

Simple Routing describes a design pattern which uses direct interaction with `\Parable\Framework\App` to quickly define simple routes that use either a closure or a controller-action array (`["ControllerName", "action"]`). Since the routes are all defined in the same location, this is most useful for applications that don't require many routes.

If you expect your application to grow significantly, perhaps consider using [Parable Framework](./parable-framework) instead.

## How to use

```php
require_once __DIR__ . '/vendor/autoload.php';

$app = \Parable\DI\Container::create(\Parable\Framework\App::class);

$app->get('/hello/{name}', function ($name) use ($app) {
    return "Hello, {$name}!";
});

$app->run();
```

The idea behind quickroutes is the sometimes you don't need a whole MVC circus but you just want some simple routes that return some value. All methods that Parable supports have equivalent quick route methods on `App`: `$app->get(...)`, `$app->post(...)`, `$app->put(...)`, `$app->patch(...)`, `$app->delete(...)`, `$app->options(...)` and can, through DI, access any component from Parable.

There are also two other methods:
- `$app->any(...)`, which accepts any aforementioned request method.
- `$app->multiple(...)`, which accepts only the methods passed to it.

## Parameters

The `multiple()` method accepts one additional parameter as its first parameter. The remaining parameters are the same as they are for all other methods.

Parameter|Type|Info
---------|----|------
`$methods`|array|The methods the route will accept, i.e. `["GET", "POST"]`


The parameters for all but `multiple()` are as follows:

Parameter|Type|Info
---------|----|------
`$url`|string|The url the route matches with. Can contain parameters. Ex. `/profile/{id}`. Parameters are passed to the callable in order.
`$callable`|callable|The callable code that will be executed if the url is positively matched.
`$name`|string\|null|An optional value, which makes it possible to use `Toolkit::getFullRouteUrlByName()` and similar functions in `\Parable\Routing`. 
`$templatePath`|string\|null|An optional value, which can be used to add a template (`.phtml`) file to a route. This file will be included after the code is called.