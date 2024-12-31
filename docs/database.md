# Connecting to and Accessing the Database  

You can define and interact with your chosen database using one or more of the following methods:  

1. **Schema Access**  
2. **CRUD Access**  
3. **Query Access**  

Each method has unique characteristics, so you can choose the one that best suits your needs.  
Here's a quick comparison:  

| Method        | Verbosity | Dynamic | Combinatorial |  
|---------------|-----------|---------|---------------|  
| **Schema Access** | 🟢 Low    | 🔴 No    | 🔴 No          |  
| **CRUD Access**   | 🟡 Medium | 🟢 Yes   | 🔴 No          |  
| **Query Access**  | 🔴 High   | 🟢 Yes   | 🟢 Yes         |  

Now, let’s explore how to use each method effectively.  

---

## 1. Schema Access  

With **Schema Access**, the database and table are defined in the schema.
- The `crud` method is automatically called, but you can explicitly invoke it if needed.  
- If no table is specified (`null`), the default table name will match the entity name (*User* in this example).  

### Example:  
```php
class User extends Entity {}

User::schema($database,'table_name',[
  'id',
  'name',
  'email'
]);

// Retrieve the first record
$user=User::one();
```  

---

## 2. CRUD Access  

In **CRUD Access**, you specify the database dynamically using the `crud` method.  

### Example:  
```php
class User extends Entity {}

User::schema(columns:[
  'id',
  'name',
  'email'
]);

// Define the database dynamically
$user = User::crud($database)->one();
```  

You can also specify the table dynamically for the current query:  

### Example:  
```php
$user = User::crud($database, 'table_name')->one();
```  

---

## 3. Query Access  

With **Query Access**, you gain full control by working directly with the [`Query`](query.md) object.  
This approach is the most dynamic and flexible.  

### Example:  
```php
class User extends Entity {}

User::schema(columns:[
  'id',
  'name',
  'email'
]);

// Retrieve and customize the query object
$query = User::crud()->query->one();
$user = $query->run($database)->result;
```  

---

# What’s Next?  

- [**Learn About Queries**](query.md)
- [**Learn About Entities**](entity.md)
