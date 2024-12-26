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

## SQL Composition
If we want a value to be escaped (for security), we just separate the value with commas.  
In this example we select 3 users with id higher than 5 and we make sure that the number `5` is escaped properly.
```php
$query=new Query('select * from User where id>',5,'limit 3');
```
