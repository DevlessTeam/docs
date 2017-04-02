## DataStore

- [DataStore](#ds)
- [Query records](#query)
- [Add records](#add)
- [update records](#update)
- [Deleting table and records](#delete)
- [Instance Info](#info)
- [Dump](#dump)


<a name="ds">DataStore</a>

**DataStore** Helper is the DevLess internal API for working with data related to services.
The API can be accessed from within the rules engine as well.

**To use the DataStore class**

Set ``use App\Helpers\DataStore;``
at the very top of the class right after the ``<?php`` tag then

<a name="query">Query Records</a>

**Query Records**

To get all records
```
DataStore::service('serviceName', 'tableName')->queryData();
```
You may pass in query parameters
eg: The size parameter gets a given number of results
```
DataStore::service('serviceName', 'tableName')->size(7)->queryData();
```
Available query parameters include :

``size($size)`` Size as explained earlier makes it possible to return a given number of records.

``OrderBy($field)`` OrderBy makes it possible to order the records in asc order based on the given $field.

``where($column, $value)`` You may query data where a given ``$column`` equals a given ``$value``  NB: ``where`` may also be used when updating particular records.

``orWhere($column, $value)`` Use this when you want results that may have a column set to one thing or there other. 

``offset($number)`` It is also possible to skip a number of records with the offset parameter by providing the $number of records to skip.  

``search($column, $value)`` you may also run a search based on a specific column type
``eg: DataStore::service('serviceName', 'tableName')->size(7)->search('name', 'John Doe')->queryData();``

<a name="add">Add Records</a>

**Add Records**

To add data to a given table  passing it in as an array of values
```
DataStore::service('serviceName', 'tableName')->addData([['firstName'=>'edmond','lastName'=>'Mensah']])
```
You may also make double entries
```
DataStore::service('serviceName', 'tableName')->addData([
['firstName'=>'edmond','lastName'=>'Mensah'],
['firstName'=>'charles','lastName'=>'Feggerson']
])
```
<a name="update">Update Records</a>

**Update Records**

To update records you need the record id
```
$data = ['firstName'=>'james'];
DataStore::service('serviceName', 'tableName')->where($id, $value)->update($data)
```
***NB: $id should be a string identifier ('id') if not declared as a variable*** 

<a name="delete">Delete tables or records</a>

**Deleting table and records**

To delete records from the table
```
DataStore('serviceName', 'tableName')->where($id, $value)->delete();
```
To truncate a table
```
DataStore('serviceName', 'tableName')->truncate()
```
To drop table
```
DataStore('serviceName', 'tableName')->drop()
```

<a name="info">Instance Info</a>

 You may also get all info about the DevLess instance using the DataStore from within your application.

 ```
 $instance = DataStore::instanceInfo();

 $user = $instance['admin'];
 $app  = $instance['app'];

 var_dump($user, $app)
 ```

<a name="dump">Dump</a>

DevLess also provides a key/value store. This could be used to store user configuration settings or any data not part of the service but relates to the current instance only

```
 use App\Helpers\DataStore;


 DataStore::setDump('created_on', time());

 $data = getDump('created_on');
 var_dump($data);

 DataStore::updateDump('created_on', time());

 $data = getDump('created_on');

 DataStore::destroyDump('created_on');

```
