# Entity
## Define an entity
Each entity is defined by extending the `Entity` class and calling the `schema` method.
```php
class User extends Entity{}
User::schema($database,'𝘵𝘢𝘣𝘭𝘦',[
  'id',
  'name',
  'email'
]);
```
The above is an extremelly simple example, go [here](schema.md) to learn more about schema.

## Use entities
To **create** or **read** a new entity from the database you use the `crud` method.  
In case you [access the database statically](database.md#schema-access) the `crud` method is called automatically.

### Create
To create a new entity to the database you can use the `create` method from the `crud`.  
```php
User::create([
  'name'=>'Alex',
  'email'=>'alex@domain.com'
]);
```
You can also use the `save` method to achive the same result like this.
```php
$user=new User;
$user->name='Alex';
$user->email='alex@domain.com';
$user->save();
```
### Read
To get an entity from the database you can use the `one` method from the `crud`.  
```php
# get one (first one)
$user=User::one();
```
Once an entity is collected you can access it's data via it's properties.
```php
echo $user->id;
echo '<br>';
echo $user->name;
```
### Update
To modify an entity you can just reassign the new values to it's properties, and then call the `update` method.
```php
$user->name='Alex';
$user->email='alex@domain.com';
$user->update();
```
The *ACORM* is smart enough to update only the new values.

Tou can also use the `save` method that in this case does the same thing.
```php
$user->save();
```

### Delete
To delete an entity you just call the `delete` method.
```php
$user->delete();
```

# What's next
[**`LEARN ABOUT SCHEMA`**](schema.md) [**`LEARN ABOUT CRUD`**](crud.md)
