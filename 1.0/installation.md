## Installing Parable

Parable can be installed using [Composer](https://getcomposer.org). Simply run the following:

```shell
$ composer require devvoh/parable
```

After this, Parable is ready to use. You have a choice of how to implement
Parable. You can either use the default structure as provided by Parable,
or come up with your own and use the components separately. The components
outside of the `\Parable\Framework` namespace are intended to function by
themselves and do not require the use of the default structure.

If you do want to use the default structure, which is a very loosely-defined
structure anyway, you can get started by running the following command:

```shell
$ vendor/bin/parable init-structure
```

By default the 'public' directory is just called `public`. If you want to
use a different directory than that (say, `http_docs`), you can do that by adding the `--homedir`
option:

```shell
$ vendor/bin/parable init-structure --homedir=http_docs
```

Parable was built for Apache 2.x and works out of the box for that. If you want
to use a different server package, such as nginx, you'll have to do some
setup for the rewrites yourself.

There's also a way to serve Parable without a server package installed,
which is useful for development and testing. This is done using the built-in
php web server:

```shell
$ cd vendor/devvoh/parable
$ make server
```

This effectively runs the following command:

```shell
$ php -t ../../.. -S 127.0.0.1:5678 php-server.php
```

It'll then run on `http://127.0.0.1:5678`.

The `vendor/devvoh/parable/php-server.php` contains some logic to fill in
some of the gaps between the built-in web server and Apache.
