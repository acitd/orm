# Database access
When we want to execute a query from the entity class there are 3 ways to choose the database.

## 1. Schema Access
In this case the the choosen database is defined in the schema.
```php
class User extends Entity{}
User::schema($database,'User',[
  'id',
  'name',
  'email'
]);

# get one (first one)
$user=User::one();
```
#### PROS
- Easiest to write.
- In most cases we can skip the `crud` method since it's called automatically.
- We don't need the database on the current scope.
- We don't need to write `->run($database)->result` each time.

#### CONS
- Less flexibility.
- The database is static, therefore is one per Entity.
- We don't have access to the query, therefore we can't perform multiple executions at once.

## 2. Crud Access
In this case the choosen database is defined in the `crud` method.
```php
$user=Country::crud($database)->one();
```
#### PROS
- Flexibility to change database dynamically.
- We don't need to write `->run($database)->result` each time.

#### CONS
- More verbodose.
- We don't have access to the query, therefore we can't perform multiple executions at once.

## 3. Query Access
In this case we user the `query` property to collect the `Query` object and then using it.

```php
$user=Country::crud()->query->one()->run($database)->result;
```

#### PROS
- Flexibility to change database dynamically.
- We have access to the query, therefore we can perform multiple executions at once.

#### CONS
- Most verbodose.
- We have to write `->run($database)->result` each time.
