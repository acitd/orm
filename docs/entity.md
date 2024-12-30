# Entity

## Defining an Entity
Each entity is defined by extending the `Entity` class and using the `schema` method.

```php
use acitd\Orm\Entity;

class User extends Entity {}

User::schema($database, 'table', [
    'id',
    'name',
    'email'
]);
```

This is a very simple example. For a detailed guide, visit [Schema Documentation](schema.md).

## Using Entities
To **create** or **read** an entity, you can use the `crud` method.  
When [accessing the database statically](database.md#1-schema-access), the `crud` method is called automatically.

To **update** or **delete**, you can use the entity's instance methods.

### Create
To create a new entity in the database, use the `create` method (from `crud`):

```php
User::create([
    'name' => 'Alex',
    'email' => 'alex@domain.com'
]);
```

Alternatively, use the `save` method:

```php
$user = new User;
$user->name = 'Alex';
$user->email = 'alex@domain.com';
$user->save();
```

### Read
To retrieve an entity from the database, use the `one` method (from `crud`):

```php
# Retrieve the first entity
$user = User::one();
```

Once an entity is retrieved, its data can be accessed via its properties:

```php
echo $user->id;
echo '<br>';
echo $user->name;
```

### Update
To update an entity, modify its properties and call the `update` method:

```php
$user->name = 'Alex';
$user->email = 'alex@domain.com';
$user->update();
```

Alternatively, use the `save` method, which behaves the same way in this case:

```php
$user->save();
```

*ACORM* optimizes updates to only modify changed values.

### Delete
To delete an entity, use the `delete` method:

```php
$user->delete();
```
---

## What's Next
- [**Learn About Schema**](schema.md)
- [**Learn About CRUD**](crud.md)

