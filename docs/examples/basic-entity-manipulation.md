# Create an user from a country

## SQL
```sql
CREATE TABLE `Country` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(60) NOT NULL DEFAULT 'None',
  `code` varchar(2) NOT NULL DEFAULT 'NO',
  PRIMARY KEY (`id`)
)
CREATE TABLE `User` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(30) NOT NULL,
  `image_id` bigint(20) DEFAULT NULL,
  `active` tinyint(1) NOT NULL DEFAULT 0,
  `height` decimal(10,2) NOT NULL,
  `data` longtext CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NOT NULL CHECK (json_valid(`data`)),
  `date` datetime NOT NULL DEFAULT current_timestamp(),
  `favorite_numbers` longtext NOT NULL,
  `country_id` int(11) DEFAULT NULL,
  `location` point NOT NULL DEFAULT st_geometryfromtext('point(0 0)'),
  PRIMARY KEY (`id`),
  KEY `country` (`country_id`),
  CONSTRAINT `User_ibfk_1` FOREIGN KEY (`country_id`) REFERENCES `Country` (`id`)
);
```
## PHP
```php
use acitd\Orm\{Database,Entity,Cast};


# CONNECT TO THE DATABASE

$database=new Database(new PDO('mysql:host=localhost;dbname=𝘥𝘢𝘵𝘢𝘣𝘢𝘴𝘦','𝘶𝘴𝘦𝘳','𝘱𝘢𝘴𝘴𝘸𝘰𝘳𝘥'));


# DEFINE ENTITIES

# Country Entity
class Country extends Entity{}
Country::schema($database,null,[
  'id',
  'name',
  'code'
]);

# User Entity
class User extends Entity{}
User::schema($database,null,[
  'id',                          // Auto-increment ID
  'name',                        // String column
  'active'=>Cast::bool(),        // Boolean column
  'height'=>Cast::decimal(2),    // Decimal column with 2 decimal places
  'data'=>Cast::json(),          // JSON-encoded column
  'date'=>Cast::datetime(),      // Datetime column
  'country_id',                  // Foreign key
  'favorite_numbers'=>Cast::int_list('|'), // List of integers, separated by '|'
  'location'=>Cast::point()      // 2D coordinates (Point)
]);


# CREATE THE USER

# Get the Greece country
$greece=Country::where('code=','GR')->one();

# Create a new user from Greece
$user_id=User::create([
  'name'=>'User_'.upiqid(),
  'active'=>true,
  'height'=>1.80,
  'country_id'=>$greece,
  'favorite_numbers'=>[42,43,44,45]
]);

echo 'The user with ID '.$user_id.' has been created!';


# UPDATE THE USER

$new_favorite_number=100;

# Getting the user from the database
$user=User::get($user_id);

# Adding new number
$user->favorite_numbers[]=$new_favorite_number;
$user->update();

echo 'The user with ID '.$user_id.' has been updated!';


# DELETE THE USER
$user->delete();

echo 'The user with ID '.$user_id.' has been deleted!';
```
