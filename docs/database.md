# Connect and get the database
To access the database in *ACORM* you have to use the `Database` object.  
This is how you create it.
```php
$database=new Database(new PDO('mysql:host=𝘭𝘰𝘤𝘢𝘭𝘩𝘰𝘴𝘵;dbname=𝘥𝘢𝘵𝘢𝘣𝘢𝘴𝘦','𝘶𝘴𝘦𝘳','𝘱𝘢𝘴𝘴𝘸𝘰𝘳𝘥'));
```
# Database and table access
To execute a query from the `Entity` class you have to define in which database and table will be performed.  
You can define the choosen database in one or more of these ways.
1. Shema access
2. Crud access
3. Query access

It's your choice what to use each time, but to make it easier for you, these are some characteristics of each one.
||verbosity|dynamic|combinatorial|
|-|-|-|-|
|Schema access|🟢 Low|🔴 No|🔴 No|
|Crud access|🟡 Mid|🟢 Yes|🔴 No|
|Query access|🔴 High|🟢 Yes|🟢 Yes|

Now let's see how you cant use them.

## Schema access
In this case the the choosen database and table is defined in the schema.  
The `crud` method is called automatically, but you can always use it if you want.  
If you pass the table as `null`, the default table name will be same as the entity name (*User* in this case).
```php
class User extends Entity{}
User::schema($database,'𝘵𝘢𝘣𝘭𝘦',[
  'id',
  'name',
  'email'
]);

# get one (first one)
$user=User::one();
```

## Crud access
In this case the choosen database is defined in the `crud` method.  
```php
class User extends Entity{}
User::schema(null,null,[
  'id',
  'name',
  'email'
]);

$user=User::crud($database)->one();
```
You can also define the table dynamically for the current query like this.
```php
$user=User::crud($database,'𝘵𝘢𝘣𝘭𝘦')->one();
```

## Query access
In this case you can use the `query` property to collect the [`Query`](query.md) object and then using it as you wish.

```php
class User extends Entity{}
User::schema(null,null,[
  'id',
  'name',
  'email'
]);

$query=Country::crud()->query->one();
$user=$query->run($database)->result;
```
# What's next
[**`LEARN ABOUT QUERY`**](query.md) [**`LEARN ABOUT ENTITY`**](entity.md)
