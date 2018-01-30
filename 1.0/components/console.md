## Console

`\Parable\Console` contains classes to build console applications with. Parable comes with an implementation of this in the form of `vendor/bin/parable`.

## Example application

You can try the following from your project's root directory:

```php
// Require the Composer autoloader
require_once(realpath(__DIR__ . "/vendor/autoload.php"));

/** @var \Parable\Console\App $app */
$app = \Parable\DI\Container::create(\Parable\Console\App::class);

$command = \Parable\DI\Container::create(\Parable\Console\Command\Help::class);

$app->addCommand($command);

$app->run();
```

And then just call it like this:

```shell
$ php test.php help
```

If you want it to run a command without providing it specifically, you can set a default command like this:

```php
// by command itself:
$app->setDefaultCommand($command);

// or simply by name:
$app->setDefaultCommandByName("help");
```

Check out the example `HelloWorld` command. Commands by themselves are pretty simple, but can be as complex as you want them to be.
By default, as you can see in the `parable` script, a Console application requires some basic setup. This is because Console is built to be used outside of a Parable framework setup.