# Entity
Each entity is defined by extending the `Entity` class and calling the `schema` method.
Here is an example.
```php
class User extends Entity{}
User::schema($database,'𝘵𝘢𝘣𝘭𝘦',[
  'id',
  'name',
  'email'
]);
```
## Collect an entity
To collect an entity from the database you use the `crud` method.  
In case you access the [database statically](database.md#schema-access) the `crud` method is called automatically.
```php
# get one (first one)
$user=User::one();
```
