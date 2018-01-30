## Configs

Parable's own `\Parable\Framework\Config` class is more of a Config manager than a Config itself. As such, it is an implementation of the [GetSet](getset) component. It does not, in and of itself, contain any configuration values, until they're added from actual Config implementations.

Actually working with Config classes is done by adding them by calling `\Parable\Framework\Config::addConfig()` with a Config class or by by letting Parable take care of that for you.

## Automatically by Parable

Parable by default, attempts to load `\Config\App`, which implements the `Config` interface (an extension of the `Gettable` interface, which demands nothing more than a public `get()` method that returns an array of data).

The example `\Config\App` in the default structure shows how to add additional Config files.

```php
namespace Config;

class App implements \Parable\Framework\Interfaces\Config
{
    public function get()
    {
        return [
            "parable" => [
                "configs" => [
                    \Config\Custom::class
                ],
            ],
        ];
    }
}
```

The Config files found in that location in the array will be automatically added to `\Parable\Framework\Config` and its values will be available.

The order in which the Config files are added to the array matters. Earlier values _will_ be overwritten by later-set values.

## Manually adding Configs

This isn't much harder, except that any values won't be available until after you've added them to the `\Parable\Framework\Config` pool. If you add more Config files in an [Init](inits) class, then their values will be available before the Database connection is set up.

Setting them later can be useful if you need database access to decide what configuration values to set, however.

Here's an example of doing it with an Init class:

```php
namespace Init;

class loadConfigs
{
    public function __construct()
    {
        $config = \Parable\DI\Container::get(\Parable\Framework\Config::class);
        
        $configToLoad = \Parable\DI\Container::create(\Config\Custom::class);
        $config->addConfig($configToLoad);
    }
}
```