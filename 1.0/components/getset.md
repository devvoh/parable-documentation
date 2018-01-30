## GetSet

`GetSet` is a data container system that is used by many internal Parable classes. It's used for HTTP request data such as GET, POST, etc., but also for setting session messages with `\Parable\Framework\SessionMessage` and passing data internally through `\Parable\GetSet\Internal`. Some of them use PHP Superglobals, and some only use local resources.

## GetSet types

Class|Type|Purpose
-----|----|-------
`\Parable\GetSet\Cookie`|superglobal|Work with cookies,
`\Parable\GetSet\Env`|superglobal|Work with environment variables.
`\Parable\GetSet\Files`|superglobal|Work with file uploads.
`\Parable\GetSet\Get`|superglobal|Work with GET values.
`\Parable\GetSet\InputStream`|local resource|Work with values in the HTTP request body, as happens for PUT and PATCH requests.
`\Parable\GetSet\Internal`|local resource|Make it easy to pass data from one point of an application to another (i.e. controller > view).
`\Parable\GetSet\Post`|superglobal|Work with POST values.
`\Parable\GetSet\Server`|superglobal|Work with SERVER values.
`\Parable\GetSet\Session`|superglobal|Work with the session. Also allows starting/ending/etc.
