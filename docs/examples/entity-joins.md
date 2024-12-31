# Entity Joins
An advanced example of entity joins.

```php
use acitd\Orm\{Database,Entity,Cast};

# CONNECT TO THE DATABASE
$db=new Database(new PDO('mysql:host=localhost;dbname=𝘥𝘢𝘵𝘢𝘣𝘢𝘴𝘦','𝘶𝘴𝘦𝘳','𝘱𝘢𝘴𝘴𝘸𝘰𝘳𝘥'));

# ENTITIES
class User extends Entity{}
User::schema($db,columns:[
  'id',
  'name',
  'email',
  'country_id',
  'location'=>Cast::point()
]);

class Country extends Entity{}
Country::schema($db,columns:[
  'id',
  'name',
  'code'
]);

class Team extends Entity{}
Team::schema($db,columns:[
  'id',
  'name',
  'owner_id',
  'maintainer_id',
  'date'=>Cast::datetime()
]);

# QUERY
$entity=User::
  where('self.id=',1)
  ->join('left',$team=Team::attachment('@owner_id=self.id'))
  ->join('left',$maintainer=User::attachment('@id='.$team->ref('maintainer_id')))
  ->join('left',$country=Country::attachment('@id='.$maintainer->ref('country_id')))
  ->select([
    'name',                                              # object's selected property
    'location',                                          # object's selected property
    'owner_email'=>'email',                              # object's selected property with rename
    'maintainer_name'=>$maintainer->name,                # object attachment
    'maintainer_country'=>$country->name,                # object attachment
    'team'=>$team(['id','name']),                        # sub-object
    'team.maintainer_name'=>$maintainer->name,           # sub-object attachment
    'team.maintainer_location'=>$maintainer->location    # sub-object attachment
  ])
->one();

```
