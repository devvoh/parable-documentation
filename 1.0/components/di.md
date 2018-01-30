## DI

Parable's Dependency Injection system is a static-based caching DI setup. However, since DI by itself is merely intended
to provide dependency injection and not necessarily caching, the following methods are available:

## `\Parable\DI\Container::get($className)`

Get a stored instance of the requested class if it exists, otherwise create it and store it. This will also use any
cached instances for dependencies and create and store them if needed.

## `\Parable\DI\Container::create($className)`

This will always create a new instance of the requested class, but will use/create stored instances of its dependencies
as needed. The requested class is not stored and any already stored instances will remain where they are.

## `\Parable\DI\Container::createAll($className)`

This will create a new instance and will create new instances of all dependencies as well. This will _not_ store any
of the newly instantiated classes. This is for completely clean DI.

## `\Parable\DI\Container::store($class, $className)`

You can also store your own version of an already instantiated class. This makes it possible to expect an interface
in a `__construct` method and before instantiating the class, storing an instance implementing that interface under
that interface's name.

## Other methods

The DI container also supports some other methods intended to help you deal with dependencies.

`isStored($className)` will return `true` if the class name is already stored.

`clear($className)` will clear any stored instance under that name.

`clearExcept($className)` will clear every stored instance _except_ the instance stored under that name.
 
`clearAll()` will clear every stored instance.