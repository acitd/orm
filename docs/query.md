# Query
This is the core object of sql queries.  
Usually **you don't have to use it directly**, but it's good to know how it works for special cases.
```php
use acitd\Orm\Query;
```

## Composition
If you want a value to be escaped (for security), you just separate the value with commas.  
With this example you select 3 users with id higher than 5 and we make sure that the number `5` is escaped properly.
```php
$query=new Query('select * from User where id>',5,'limit 3');
```
### List of values
To pass a list of values that you desire to be escaped you can use an array.
```php
$ids=[42,43,44,45];
$query=new Query('select * from User where id in (',$ids,')');
```

### Subqueries
You can even pass a `Query` as value to use it as subquery.
```php
$subquery=new Query('select id from Country where name=','Greece');
$query=new Query('select * from User where id in (',$subquery,')');
```
### Merge queries
You can also divide your SQL code between multiple `Query` objects and then merge it with the `merge` method.  
```php
$query=new Query('select * from User where id>',5);
$query2=new Query('limit 3');
$query->merge($query2);
```

## Mutation
The execution of the above query will return a result of a PDOStatement.  
It's preferable to work directly with data, so you can modify the result with the `then` method.
```php
$query->then(fn($response)=>$response->result->fetchAll(PDO::FETCH_ASSOC));
```

## Execution
To execute the query you use the `run` method.
```php
$response=$query->run($database);

$response->result;      # query result
$response->stmt;        # PDOStatement
$response->database;    # Database
```

## Combination (multiple queries at once)
You can also combine multiple queries into one execution.  
To do so you use the `add` method.
```php
# first query
$query=new Query('select * from User limit 3');
$query->then(fn($response)=>$response->result->fetchAll(PDO::FETCH_ASSOC));

# second query
$query2=new Query('select * from Country limit 3');
$query2->then(fn($response)=>$response->result->fetchAll(PDO::FETCH_ASSOC));

# adding the second query to the first one
$query->add($query2);

# executing both queries
$response=$query->run($database);

# read the results
$response->results;       # all queries results in array
$response->results[0];    # query result of users
$response->results[1];    # query result of countries
$response->result;        # last query result (countries)
```
# What's next
[**`LEARN ABOUT ENTITY`**](entity.md) [**`LEARN ABOUT CRUD`**](crud.md)
