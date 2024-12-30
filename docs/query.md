# Query
The `Query` class is the foundation of SQL queries.  
While you typically don't need to interact with it directly, understanding its functionality can be helpful for handling special cases.

```php
use acitd\Orm\Query;
```

## Composition
### Escaping Values
To ensure values are properly escaped for security, separate them with commas.  
For example, the following query selects three users with an `id` greater than `5` while ensuring the number `5` is safely escaped:

```php
$query = new Query('SELECT * FROM User WHERE id >', 5, 'LIMIT 3');
```

### Lists of Values
You can pass an array to escape multiple values at once.  
For example:

```php
$ids = [42, 43, 44, 45];
$query = new Query('SELECT * FROM User WHERE id IN (', $ids, ')');
```

### Subqueries
You can use another `Query` object as a subquery.  
For instance:

```php
$subquery = new Query('SELECT id FROM Country WHERE name =', 'Greece');
$query = new Query('SELECT * FROM User WHERE id IN (', $subquery, ')');
```

### Merging Queries
Split your SQL code into multiple `Query` objects and combine them using the `merge` method:

```php
$query1 = new Query('SELECT * FROM User WHERE id >', 5);
$query2 = new Query('LIMIT 3');
$query1->merge($query2);
```

## Mutation
When executed, a `Query` returns a `PDOStatement`.  
To simplify data handling, you can use the `then` method to process the result directly:

```php
$query->then(fn($response) => $response->result->fetchAll(PDO::FETCH_ASSOC));
```

## Execution
Run your query using the `run` method.  
This returns a response object with the following properties:

```php
$response = $query->run($database);

$response->result;      // Query result
$response->stmt;        // PDOStatement
$response->database;    // Database instance
```

## Combining Multiple Queries
You can execute multiple queries together using the `add` method.  
Here's an example:

```php
# First query
$query1 = new Query('SELECT * FROM User LIMIT 3');
$query1->then(fn($response) => $response->result->fetchAll(PDO::FETCH_ASSOC));

# Second query
$query2 = new Query('SELECT * FROM Country LIMIT 3');
$query2->then(fn($response) => $response->result->fetchAll(PDO::FETCH_ASSOC));

# Adding the second query to the first one
$query->add($query2);

# Executing both queries
$response=$query->run($database);

# Read the results
$response->results;       # all queries results in array
$response->results[0];    # query result of users
$response->results[1];    # query result of countries
$response->result;        # last query result (countries)

```
# What's next
[**`LEARN ABOUT QUERY`**](query.md) [**`LEARN ABOUT ENTITY`**](entity.md)
