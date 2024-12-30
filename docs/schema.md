# Schema Definition

The `schema` static method in the `Entity` class is used to define the database-related configuration for an entity.  
This method accepts the following arguments:

| Parameter | Type                       | Required  | Behavior                                                                                      |
|-----------|----------------------------|-----------|-----------------------------------------------------------------------------------------------|
| **database** | [Database](database.md)   | 🔴 No      | If not specified (`null`), it must be provided in the Entity's `crud` method or the `run` method of the `Query`. |
| **table**    | string                     | 🔴 No      | Defaults to the name of the Entity's class if not explicitly set (`null`).                     |
| **columns**  | array                      | 🟢 Yes     | The first column is the `AI ID` (usually the `'id'`).<br>Simple types integers and strings do not require casting. |

### Example Usage
```php
use acitd\Orm\{Entity, Cast};

class User extends Entity {}

User::schema($database, 'users', [
    'id',                          // Auto-increment ID
    'name',                        // String column
    'active' => Cast::bool(),      // Boolean column
    'height' => Cast::decimal(2),  // Decimal column with 2 decimal places
    'data' => Cast::json(),        // JSON-encoded column
    'date' => Cast::datetime(),    // Datetime column
    'country_id',                  // Foreign key
    'favorite_numbers' => Cast::int_list('|'), // List of integers, separated by '|'
    'location' => Cast::point()    // 2D coordinates (Point)
]);
```

---

# Columns

The `columns` argument accepts an associative array defining the database columns and their corresponding casts.  
Casts determine how values are interpreted when reading from or writing to the database.  

While custom casts can be created, the native casts typically cover most use cases.

### Native Casts

| Method                     | SQL Type                | Description                                                 |
|----------------------------|-------------------------|-------------------------------------------------------------|
| **Cast::bool()**           | `tinyint(1)`           | For boolean values.                                         |
| **Cast::decimal(𝘯)**       | `decimal(10,𝘯)`        | For floating-point values with `𝘯` decimal places.          |
| **Cast::json()**           | `longtext`             | For `stdClass` or `array`.                                  |
| **Cast::datetime()**       | `datetime`             | For `DateTime` objects.                                     |
| **Cast::int_list(𝘥)**      | `longtext`             | For an array of integers, delimited by `𝘥`.                 |
| **Cast::str_list(𝘥)**      | `longtext`             | For an array of strings, delimited by `𝘥`.                  |
| **Cast::point()**          | `point`                | For 2D coordinates stored as a `point` (e.g., `POINT(x y)`).|
