## ORM Overview

Parable's ORM uses an ActiveRecord pattern-like system, where every model corresponds to a database table, but a minimalist Repository implementation helps deal with loading existing models. 

## Basic Usage

The first thing to do is to define a model class that extends the base Model class. A good example of this is the
provided `/app/Model/User.php` from the default structure.

You define the table name and default primary key in `$tableName` and `$tableKey`, and the columns on the table itself as public properties.

```php
namespace Model;

class User extends \Parable\ORM\Model
{
    /** @var string */
    protected $tableName = 'user';
    
    /** @var string */
    protected $tableKey  = 'id';
    
    /** @var string */
    public $username;
    
    /** @var string */
    public $password;§
    
    /** @var string */
    public $email;
}
```

If you include a `$created_at` public property, the ORM will automatically set that when inserting a record. If you include an `$updated_at` property,
every update on it will update that field as well. 

Create a new `User` and save it:

```php
$user = \Model\User::create();

$user->username = "Testing Person";
$user->password = password_hash("actual-pass-here", PASSWORD_DEFAULT);
$user->email    = "555-email@test.test";

$user->save(); // will return bool on success or fail
```

After saving a new instance of a model, the `$tableKey` will be set based on the return value from the database.

## Repositories

You can choose to do it directly with the main [Repository](repository) class or with extending classes.

Directly: 

```php
$userRepo = \Parable\ORM\Repository::createForModelName(\Model\User::class);

// array of model instances
$all = $userRepo->getAll();

// array of model instances
$some = $userRepo->getByCondition("email", "LIKE", "person@%");

// single model instance
$one = $userRepo->getById(1337);
```

Extending:

```php
class UserRepository extends \Parable\ORM\Repository
{
    public function getUsersWithEmailAddresses()
    {
        return $this->getByCondition("email", "is not null");
    }
    
    public function getUsersWithoutEmailAddresses()
    {
        return $this->getByCondition("email", "is null");
    }
}
```

This would allow you to use all the basic Repository logic, but also offer far more fine-grained methods. Using 
extending classes can make it far clearer what you're working with, and can even make it easier, i.e. by implementing 
a `create()` method that calls `\Parable\ORM\Repository::createForModelName()` so you can instantly get a specific 
Repository.

Another way is by adding the repositories you need to the constructor to let DI set up the Repositories you'll need. 

## Working with Models

A [Model](model) instance is responsible for saving (whether it's to `insert` or `update`, for example) and deleting.

To save a model (new or updated), simply call `$model->save()`. It'll return a `bool` value of whether it succeeded
or not. To delete a model, call `$model->delete()`, and again it'll return a `bool`.

It's also possible to get a [Query](query) instance populated with the model's tablename and primary key.

```php
$query = $model->createQuery();
$query->where(
    $query->buildAndSet([$model->getTableKey(), '=', $model->id])
);

echo $query;
```

This would output:

```sql
SELECT * FROM `model` WHERE (`model`.`id` = '1');
```

What's used to add many types of conditions are [ConditionSets](conditionsets), a system to easily and reliably build 
nested `AND`/`OR` condition clauses.