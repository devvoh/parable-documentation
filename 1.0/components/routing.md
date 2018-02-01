## Routing

Routing in Parable is always concrete. It can be done through helper functions on `\Parable\Framework\App` or directly
on `\Parable\Routing\Router`. Generally using `App` is the more convenient way. 

```php
$this->app->get(
    "/",
    [\Controller\Home::class, "index"],
    "index"
);
```

In this case, the array passed (`[\Controller\Home::class, "index"]`) is considered a callable. You can also throw
a closure in here. Parable will detect how to interpret it and make sure it's called once the route is matched.

You can see in `\Parable\Framework\App::get()` how the route ends up added to the `Router`.

## Routes with parameters

Obviously you'll want to be able to add routes that take parameters. This is pretty easy.

```php
$this->app->get(
    "/post/{id}",
    [\Controller\Home::class, "post"],
    "post"
);
```

In this case, requesting `https://host.ext/post/12` will call `\Controller\Home::post($id)` with `$id` set to `12`. All
values passed as parameters are by default passed as string values, since they come straight from the url.

## Dynamic routes

In case you want to allow `https://host.ext/here-is-a-post-title-that-should-load`, routing might not be your solution.
In most cases, if you want to do dynamic routing, the best way to do so is to use [inits](inits) in combination with
[events](events) and hook into the event `\Parable\Framework\App::HOOK_ROUTE_MATCH_AFTER`. That event gets the `$route`
as payload.

If it's `null`, normal flow would continue to trigger the 404 event and send the response. In this case, you could
apply your own logic and dynamically load what needs to be loaded.

If it's an actual `Route` object, then the current url was matched and you can let it pass as-is.

This functionality, however, would allow you to intercept a route and adjust it before letting it go on.