# Schema
The Entity's `schema` static method is used to define the database info of an Entity.  
It takes the following arguments.
||type|required|behavior|
|-|-|-|-|
|**database**|[Database](database.md)|🔴 No|If is not defined (null) in schema, it has to be defined in the Entity's `crud` method or in the `run` method of the `Query`. |
|**table**|string|🔴 No|If is not defined (null) in schema, it becomes by default same as the name of the Entity's class.|
|**columns**|array|🟢 Yes|The first column is the `AI ID` (usually 'id').<br>`Integers` and `strings` do not need `Cast`.|

### Example
```php
use acitd\Orm\{Entity,Cast};

class User extends Entity{}
User::schema($database,'users',[
	'id',
	'name',
	'active'=>Cast::bool(),
	'money'=>Cast::decimal(2),
	'data'=>Cast::json(),
	'date'=>Cast::datetime(),
	'country_id',
	'teams_ids'=>Cast::list('|'),
	'location'=>Cast::point()
]);
```

# Columns
The `columns` argument receives an assoc array with all db columns and their casting.
