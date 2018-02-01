## Inits

Inits are classes that are by default defined in the `\Config\App` class by adding them to the `parable.inits` key,
and are loaded immediately after the config files are. They are intended to offer you a way of hooking into the
framework events before anything else happens. This can help set up pre-routing logic or can be used to prepend/append
header and footer views to the response (example of which below).

## How they work

Inits are simply instantiated. This means that implementing your logic in the constructor of the file is enough.
You can make Inits as complex or simple as you'd like. You can either use one Init for every event, or a single,
large class that has many sub-methods, or even loads other classes.

DI is used to instantiate Inits, do you can add dependencies to the constructor like any other class.

## Examples

To use as an example, here's a way of prepending a header and appending a footer.

```php
namespace Init;

class Main
{
    public function __construct(
        \Parable\Event\Hook $hook,
        \Parable\Framework\View $view,
        \Parable\Http\Response $response
    ) {
        // Prepend the header to the response
        $hook->into(
            \Parable\Framework\Dispatcher::HOOK_DISPATCH_TEMPLATE_BEFORE,
            function () use ($view, $response) {
                $content = $view->partial("app/View/Layout/header.phtml");
                $response->prependContent($content);
            }
        );

        // Append the footer to the response
        $hook->into(
            \Parable\Framework\Dispatcher::HOOK_DISPATCH_TEMPLATE_AFTER,
            function () use ($view, $response) {
                $content = $view->partial("app/View/Layout/footer.phtml");
                $response->appendContent($content);
            }
        );
    }
}
```

To understand the `Hook` logic, check out the page on [events](events). The events being hooked into are default
Parable events.

As soon as the Init is loaded, it'll be executed.