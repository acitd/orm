# ORM
This is the doc of ACITD ORM.

# Database
This how we connect to the database.
```php
$database=new Database(new PDO('mysql:host=localhost;dbname=𝘥𝘢𝘵𝘢𝘣𝘢𝘴𝘦','𝘶𝘴𝘦𝘳','𝘱𝘢𝘴𝘴𝘸𝘰𝘳𝘥'));
```
# Query
This is the core object of sql queries.  
Usually we don't have to use it directly, but it's good to know how it works.

### Composition
If we want a value to be escaped (for security), we just separate the value with commas.  
In this example we select 3 users with id higher than 5 and we make sure that the number `5` is escaped properly.
```php
$query=new Query('select * from User where id>',5,'limit 3');
```
We can also divide our SQL code between multiple `Query` objects and then merge it with the `merge` method.  
This example does exactly the same as the previous example. 
```php
$query=new Query('select * from User where id>',5);
$query2=new Query('limit 3');
$query->merge($query2);
```

### Mutation
After the execution of the above query it will return a result of a PDOStatement.  
We usually prefer to work directly with data, so we can modify the result with the `then` method.
```php
$query->then(fn($response)=>$response->result->fetchAll(PDO::FETCH_ASSOC));
```

### Execution
To execute the query we use the `run` method.
```php
$response=$query->run($database);

$response->result;      # query result
$response->stmt;        # PDOStatement
$response->database;    # Database
```

### Multiple queries
We can also combine multiple queries into one execution.  
To do so we use the `add` method.
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
