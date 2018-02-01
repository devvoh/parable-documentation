## Parable Framework

Using the Parable Framework with the default structure is pretty straightforward.

To install the default structure, if you haven't yet, follow the instructions in [Installation](../installation).

The default structure offers a skeleton setup with all elements in place as an example.

## Introduction

Parable has a number of files/classes it'll look for and load if they are present. The most important of these will be `\Config\App` in `app/Config/App.php`. This is where all the setup is done for Parable, including further [config](../components/configs) files, [routing](../components/routing) files, [init](../components/inits) files, and [console commands](../components/console#commands).

## Flow of Parable

1. Parable starts by instancing the `\Parable\Framework\App` class.
2. It then loads `\Config\App` if it exists. If there are any additional config files defined in the config array, it'll load those too, in order of definition.
3. It then loads any [inits](../components/inits) defined in `parable.inits` and initializes them. Init functionality should be defined in the `__construct()` and from this point on, it's possible to hook into [events](../components/events).
4. Routes will be loaded, if any routing classes are defined in `parable.routes`, and all routes will be loaded.
5. If configured, the database connection is created. From this point on, events triggered will have database-access.
6. Parable attempts to match the current url to a route.
    - Match found: dispatch the Route.
    - Match not found: 404, stop handling.
    
If matched, the Dispatcher takes over at this point, and it will handle the Route as it's configured.

## Parable's default `\Config\App` structure

```php
namespace Config;

class App implements \Parable\Framework\Interfaces\Config
{
    public function get()
    {
        return [
            "parable" => [
                "app" => [
                    "title" => "Parable",
                    "homedir" => "public",
                ],
                "debug" => true,
                "session" => [
                    "auto-enable" => true,
                ],
                "database" => [
                    "type" => \Parable\ORM\Database::TYPE_MYSQL,
                    "location" => "localhost",
                    "username" => "username",
                    "password" => "password",
                    "database" => "database",
                ],
                "configs" => [
                    \Config\Custom::class
                ],
                "commands" => [
                    \Command\HelloWorld::class,
                ],
                "inits" => [
                    \Init\Example::class,
                ],
                "routes" => [
                    \Routing\App::class,
                ],
            ],
        ];
    }
}
```

## Parable config values

The following configuration values are automatically picked up by Parable:

Parameter|Type|Info
---------|----|------
`parable.app.homedir`|string|Where the publicly accessible files and `index.php` can be found.
`parable.database.type`|string|`\Parable\ORM\Database::TYPE_MYSQL` or `TYPE_SQLITE`
`parable.database.location`|string|For `TYPE_SQLITE` the location of the database file. For `TYPE_MYSQL` the host.
`parable.database.username`|string|Only for `TYPE_MYSQL`.
`parable.database.password`|string|Only for `TYPE_MYSQL`.
`parable.database.database`|string|Only for `TYPE_MYSQL`.
`parable.debug`|bool|Enable displaying errors, default `false`.
`parable.session.auto-enable`|string|Enable sessions by default or not. Default `true`.
`parable.configs`|array|A list of additional configs to load.
`parable.commands`|array|A list of commands to load.
`parable.inits`|array|A list of inits to load.
`parable.routes`|array|A list of routes files to load.
`parable.timezone`|string|A timezone string in the form of `Europe/Amsterdam` will be set as default.

To get a config value from the Config class, call `$config->get("parable.app");`. The dot-notation (so `parable.app.title`) is a shorthand for getting to nested values. So when this documentation says `parable.app.title`, that's what we're referring to.

None of the values in the default structure's Config are required. They're handy, but they're not required. Instead of setting the Database config in here, you could simply create a new instance yourself and setting the config on that instead.

What's most useful is the additional configs, commands, inits and routes.