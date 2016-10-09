## DataStore

- [DataStore](#ds)
- [initialising the service DI](#init)
- [Query records](#query)
- [Add records](#add)
- [update records](#update)
- [Deleting table and records](#delete)


<a name="ds">DataStore</a>

**DataStore** 
 Helper is the Devless internal api for working with data related to services. 
The api can be accessed from within the rules engine as well as.

**To use the DataStore class** 

Set ``use App\Helpers\DataStore;``
``use App\Http\Controllers\ServiceController as service ;``
at the very top of the class right after the ``<?php`` tag

<a name="init">Initialize Service DI</a>

**Intializing the service DI** 
```
$service  = new service();
```
<a name="query">Query Records</a>

**Query Records**

To get all records 
```
DataStore::service('serviceName', 'tableName', $service)->queryData();
```
You may pass in query parameters 
eg: The size parameter gets a given number of results 
```
DataStore::service('serviceName', 'tableName')->size(7)->queryData();
```
Available query parameters include :

``size($size)`` Size as explained earlier makes it possible to return a given number of records.

``OrderBy($field)`` OrderBy makes it possible to order the records in asc order based on the given $field.

``where($column, $value)`` You may also query data where a given $column equals a given $value  NB:where may also be used when updating particular records 

``offset($number)`` Its also possible to skip a number of records with the offset parameter by providing the $number of records to skip  

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
DataStore('serviceName', 'tableName')->where($id, $value)->update($data)
```
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