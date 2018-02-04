## ORM Query

The Query class is the heart of the ORM system. It's a query builder and builds and outputs queries that are compatible 
with MySQL and SQLite.

## Basic Usage

Query itself does nothing with the database, except for escaping values through `\Parable\ORM\Database::quote()`, which
in itself uses `PDO::quote()`. If no database instance is available, it uses soft quoting instead, adding single quotes 
where needed, unless it's disabled.

```php
$query = \Parable\ORM\Query::createInstance();

$query->setAction("delete");
$query->setTableName("users");
$query->where("id", "=", 1);

$result = $database->query($query);
```

Soft quoting can be disabled by setting the `parable.database.soft-quoting` config setting to `false` or calling
`setSoftQuoting(false)` on the database instance at any point.

## Actions

Query supports only the 4 most basic actions: `select`, `insert`, `update` and `delete`. For anything more involved,
you can either extend Query and add them, or you should do it without the use of Query.

## Table name and key

Every Query requires a `$tableName` so it knows what table to target. `$tableKey`