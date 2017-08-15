## DataStore

- [DataStore](#ds)
- [Query records](#query)
- [Add records](#add)
- [Update records](#update)
- [Delete table and records](#delete)
- [Instance Info](#info)
- [Dump](#dump)


## <a name="ds"></a> DataStore

DataStore Helper is the DevLess internal API for working with data related to services. The API can be accessed from within the rules engine as well.

To use the DataStore class import it by inserting `use App\Helpers\DataStore;` at the very top of the php file, right after the `<?php` tag. Now you can use it to interact with the database.

## <a name="query"></a> Query Records

To get all records:
```php
DataStore::service('serviceName', 'tableName')->queryData();
```

You may pass in query parameters. eg: The size parameter gets a given number of results:
```php
DataStore::service('serviceName', 'tableName')->size(7)->queryData();
```
Available query parameters include :

| Parameter Name | Description | 
| --  | -- | 
|`size($size)` |  Size as explained earlier makes it possible to return a given number of records.|
| `offset($number)` | It is also possible to skip a number of records with the offset parameter by providing the $number of records to skip.  |
|`OrderBy($field)`|  `OrderBy` makes it possible to order the records in ascending order based on the given `$field.` |
| `where($column, $value)` |  You may query data where a given `$column` equals a given `$value`. `where` may also be used when updating particular records.
| `orWhere($column, $value)` | Use this when you want results that may have a column set to one thing or there other.  | 
| `search($column, $value)` | You may also run a search based on a specific column type. Will match data 

## <a name="add"></a> Add Records

To add data to a given table by passing it in as an array of values.

```php
DataStore::service('serviceName', 'tableName')->addData([['firstName'=>'edmond','lastName'=>'Mensah']])
```
You may also make double entries
```php
DataStore::service('serviceName', 'tableName')->addData([
  ['firstName'=>'edmond','lastName'=>'Mensah'],
  ['firstName'=>'charles','lastName'=>'Feggerson']
])
```
## <a name="update"></a>Update Records


To update records you need the record id. 
```php
$data = ['firstName'=>'james'];
DataStore::service('serviceName', 'tableName')->where($id, $value)->update($data)
```
The `$id` parameter should be a string identifier. 

# <a name="delete"></a> Delete tables or records

To delete records from the table:
```php
DataStore('serviceName', 'tableName')->where($id, $value)->delete();
```
To truncate a table:
```php
DataStore('serviceName', 'tableName')->truncate()
```
To drop table:
```php
DataStore('serviceName', 'tableName')->drop()
```

## <a name="info"></a> Instance Info

You may also get all info about the DevLess instance using the DataStore from within your application.

 ```php
 $instance = DataStore::instanceInfo();

 $user = $instance['admin'];
 $app  = $instance['app'];

 var_dump($user, $app)
 ```

## <a name="dump"></a> Dump

DevLess also provides a key/value store. This could be used to store user configuration settings or any data not part of the service but relates to the current instance only

```php
 use App\Helpers\DataStore;

 //stores the current time `time()` under the key created_on
 DataStore::setDump('created_on', time());

 //gets the value under the created_on key
 $data = DataStore::getDump('created_on');
 var_dump($data);

 //updates the created_on key to the new current time()
 DataStore::updateDump('created_on', time());

 $data = DataStore::getDump('created_on');
 var_dump($data);
 
 //removes the entire key/value created_on
 DataStore::destroyDump('created_on');

```
