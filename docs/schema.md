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
	'height'=>Cast::decimal(2),
	'data'=>Cast::json(),
	'date'=>Cast::datetime(),
	'country_id',
	'favorite_numbers'=>Cast::int_list('|'),
	'location'=>Cast::point()
]);
```

# Columns
The `columns` argument receives an assoc array with all db columns and their casts.  
The casts define how the column values will be read or writen from/to the database. 
You can create your own custom casts but usually the native ones are sufficient.

### Default casts
|method|SQL Type|description|
|-|-|-|
|Cast::bool()|tinyint(1)|For boolens values.|
|Cast::decimal(𝘯𝘶𝘮𝘣𝘦𝘳)|decimal(10,𝘯𝘶𝘮𝘣𝘦𝘳)|For float values.|
|Cast::json()|longtext|For `stdObject` or `array`.|
|Cast::datetime()|datetime|For `DateTime` object.|
|Cast::int_list(𝘥𝘪𝘷𝘪𝘥𝘦𝘳)|longtext|For `array` of integers.|
|Cast::str_list(𝘥𝘪𝘷𝘪𝘥𝘦𝘳)|longtext|For `array` of strings.|
|Cast::point()|point [st_geometryfromtext('point(0 0)')]|For `array` of 2D coordinates.|
