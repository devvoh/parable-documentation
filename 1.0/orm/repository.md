## ORM Repository

Repositories are intended as a way to easily work with collections straight from the database. It allows simple methods
to query for [ConditionSets](conditionsets) by abstracting away the logic of setting up a [Query](query) instance
yourself, by using [Model](model) instance to set up the query.

## Basic Usage

```php
$repository = \Parable\ORM\Repository::createForModel($model);

// array of model instances
$all = $repository->getAll();

// array of model instances
$some = $repository->getByCondition("key", "LIKE", "value@%");

// single model instance
$one = $repository->getById(1337);
```

## Conditions

It's pretty easy to get models based on certain conditions. `Repository::getByCondition()` does this with a single 
condition, where you pass a key, a comparator and a value. It is a shorthand way of querying based on a single 
condition without having to set up a [ConditionSet](conditionsets) yourself.

Parameter|Type|Info
---------|----|------
`$key`|string|The key should match the property on the model (and by extension, the column name in the database).
`$comparator`|string|Usually this will be `=`, but can also be `in`, `not in`, `like`, `not like`, `is null` and `is not null`.
`$value`|mixed|The value to match with. In case of `in` or `not in`, this should be an array. In case of `is null` or `is not null`, this is unused.

`Repository::getByConditionSet()` is actually then called by `Repository::getByCondition()` with a [ConditionSet](conditionsets)
built from the parameters you provided.

## Multiple conditions

For multiple conditions, it is necessary to use [ConditionSet](conditionsets). They're pretty straightforward,
however.

```php
$userResult = $repository->getByConditionSet(
    $repository->buildAndSet([
        ['id', '=', $id],
        ['username', '=', $username],
    ])
);
```

Using an `AndSet` allows simple conditions stringed together with `AND`. If you want to use `OR`, simply use 
`buildOrSet` instead.

## Limit and Order By

Repositories offer a way both limit (and use offset) and order by, just like the [Query](query) class. It simply sets 
these values on the query it stores. This does mean that if you're running multiple get calls on a Repository, you have 
to reset these values. This is pretty easy, however, by just calling `reset()`, which resets `onlyCount`, `returnOne`, 
the `orderBy` and the `limit`.

```php
$repository->reset();
```

And then you can continue with your next get call.