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
  ->join('left',$maintainer=User::attachment('@id='.$team->maintainer_id))
  ->join('left',$country=Country::attachment('@id='.$maintainer->country_id))
  ->select([
    'name',                                              # object's selected property
    'location',                                          # object's selected property
    'owner_email'=>'email',                              # object's renamed selected property
    'maintainer_name'=>$maintainer->name,                # object property attachment
    'maintainer_country'=>$country->name,                # object property attachment
    'team'=>$team([                                      # sub-object attachment
      'id',                                              # sub-object's selected property
      'team_name'=>'name'                                # sub-object's renamed selected property
    ]),
    'team.maintainer_name'=>$maintainer->name,           # sub-object property attachment
    'team.maintainer_location'=>$maintainer->location    # sub-object property attachment
  ])
->one();

```

#### RESULT
```json
{
  "name":"Alex",
  "location":[37.9795,23.7162],
  "owner_email":"alex@domain.com",
  "maintainer_name":"Peppe",
  "maintainer_country":"Italy",
  "team":{
     "id":1,
     "team_name":"ACITD TEAM",
     "maintainer_name":"Peppe",
     "maintainer_location":[41.902782,12.496366]
  }
}
```
