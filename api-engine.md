
---

## API Engine

* [DevLess API Engine\(DAE\)](#devless-api-enginedae)

* [Features of the API engine](#features-of-the-api-engine-include-)

* [Structure of the Authentication Service](#structure-of-the-authentication-service)
* [Creating a table](#creating-the-table)
* [Adding data to table](#adding-data-to-table)
* [Query data from table](#query-data-from-table)
* [Updating data in table](#updating-data-to-table)
* [Delete data from table](#delete-data-from-table)
* [Accessing Rules](#accessing-rules)
* [RPC Calls](#rpc-calls)

## DevLess API Engine\(DAE\)

**DevLess API Engine\(DAE\)**is an open source API engine that generates CRUD API access to databases as well as allows the execution of rules .

The current implementation of the DevLess API Engine is in PHP and on top of the [Laravel framework](https://laravel.com/).

**DAE **can be used as a standalone \(accessed solely via API calls\) however a [developer console](/developer-portal.md) is provided to interact with the API engine.

This document explains the syntax and process for accessing and working with the **DEA**.

## Features of the API engine include :

**Database access**

* Create database tables

* Add data to tables

* Query tables

* Updates data in tables

* Truncate, delete and drop tables

**Rules**

* Run rules against data. This allows you to edit data that is about to be added or updated within the database as well as the request.

As you would have noticed from above the DevLess API Engine\(DAE\) works with two main entities tables, rules each known as a**RESOURCE**.

You can also pair up resources together known as a **SERVICE **and this is responsible for one functionality of your **APPLICATION**.

In response to this order of organisation the restful endpoints are generated in the form:`<hostname>\service\<service_name>\<resource_name>\`

eg:`https:\\demo.devless.io\service\authentication\db?table=auth_table`_\(this example gets all data from the auth\_table\)_

**Endpoints definition for specific resources and actions**

For the rest of the documentation, we will assume creating a user authentication service.

## Structure of the Authentication Service

* Table name is auth\_table

**Fields required include**

* username \(string\)
* password \(hashed string\)

Also, we assume our server is on [http://localhost:8000/](http://localhost:8000/).

**NB:**To get started you need to create a new service either via the [developer console](/developer-portal.md) or from a database management client. In our case it is assumed we have done that and the name of the service  is authentication.

## Creating the table

```js
METHOD: POST

URL_STRUCTURE: http://localhost:8000/api/v1/service/authentication/schema

HEADERS: Content-Type application/json , Devless-token get_it_from_the_app_section_of_DevLess

REQUEST PAYLOAD:

{  
     "resource":[  
        {  
           "name":"auth_table",
           "description":" demo authenticate users",
           "field":[  

              {  
                 "name":"username",
                 "field_type":"text",
                 "default":null,
                 "required":true,
                 "is_unique":true,
                 "ref_table":"",
                  "validation":true
              },
              {  
                 "name":"password",
                 "field_type":"password",
                 "default":null,
                 "required":true,
                 "is_unique":false,
                 "ref_table":"",
                  "validation":true
              }
           ]
        }
     ]
  }        

RESPONSE PAYLOAD: {"status_code":606,"message":"created table successfully","payload":[]}
```

**Explanation**: From the URL after the`api/v1/service`  route, the service\_name `authentication`is passed followed by the`schema`sub-route which is the route for creating new tables for any service.

Also, the request payload takes the name of the table to be created and is prefixed with the service name during the creation of the table itself to ensure tables from other services do not conflict. You may also add a table description but this is optional

For the`field`, there are a couple of options you have to provide with some being optional and others required.

`"name"`this is the name to be given to the field and this is required.

`"field_type"`this is a [soft data type](/This is a DevLess table data type) and is required as well. The options available are shown below on the left and their translated database type on the right

```php
'text'       => 'string',
'textarea'   => 'longText',
'integer'    => 'integer',
'decimal'    => 'double',
'password'   => 'string',
'percentage' => 'integer',
'url'        => 'string',
'timestamp'  => 'timestamp',
'boolean'    => 'boolean',
'email'      => 'string',
'reference'  => 'integer',
```

**NB:**The `password`field\_type hashes any data provided in the name field automatically.

* `"default"` allows you to setup a default value for the field.

* `"required"` allows to  make the field a required field.

* `"is_unique"`  states whether this field should be unique or not.

* `"ref_table"`also in the case where the `field_type` you chose was  a  `reference` type you would have to select the table you would like to reference . The engine automatically creates the relationship between the field and primary key of the table being referenced. Also you can set the  `ref_table` to `_devless_users` this will reference the users table.

The`Response Body`provides three piece of information: the`status_code`in this case`606`, a`message`and`payload`which might contain extra information or return data. Here are the list of all the [status codes](/status-code.md)

Next step would be adding some data to the table

## Adding data to table

```js
METHOD: POST

URL_STRUCTURE: http://localhost:8000/api/v1/service/authentication/db/

HEADERS: Content-Type application/json , Devless-token get_it_from_the_app_section_of_DevLess

REQUEST PAYLOAD:

{  
   "resource":[  
      {  
         "name":"auth_table",
         "field":[  

            {  
               "username":"Edmond",
               "password":"password"
            }
         ]
      }

    ]
}

RESPONSE PAYLOAD: {"status_code":609,"message":"data has been added to table successfully","payload":[]}
```

We will repeat the above by replacing the name`Edmond`with`Charles`and keeping the password same. So now we have two records in auth\_table.

Now we can query our table

## Query data from table

```js
METHOD: GET

URL_STRUCTURE: http://localhost:8000/api/v1/service/authentication/db?table=auth_table&order=username&where=username,edmond&size=2

HEADERS: Content-Type application/json , Devless-token get_it_from_the_app_section_of_DevLess

REQUEST PAYLOAD: passed as part of the URL

RESPONSE PAYLOAD:
{
  "status_code": 625,
  "message": "Got response successfully",
  "payload": {
    "0": {
      "id": 1,
      "username": "Edmond",
      "password": "$2y$10$NO2Lpw5hYxoAtQ7EM.D4peWAhAWMjrDKgA4/0.bjJHnVuh1UPT0XS"
    },
    "related": []
  }
}
```

Now we query the auth\_table using the get parameters provided by the engine.

* The`table`parameter is used to provide the table name.
* The`orderBy`parameter makes it possible to arrange the data in descending order based on the field provided.
* The`where`parameter is used to get data where the field provided before the comma contains the data passed.`eg:&where=name,edmond`
* The`orWhere`parameter allows for quering data that might contain a keyword.`eg:&orWhere=name,edmond`.
* The`size`parameter is used to set the total number of records to be returned.
* The`offset`parameter sets an offset on the record.
* The`search`parameter allows  searching columns of a table that might contain a keyword.`eg:&search=name,edmond`.
* The `greaterThan` parameter allows query for records who's column value is greater than a certain number `eg:&greaterThan=age,18`
* The `lessThan` parameter allows query for records who's column value is less than a certain number `eg:&lessThan=age,18`
* The `greaterThanEqual` parameter allows query for records who's column value is greater than or equals a certain number `eg:&greaterThanEqual=age,18`
* The `lessThanEqual` parameter allows query for records who's column value is less than or equal to a certain number `eg:&lessThan=age,18`
* The 

Next, we are going to change the username from Edmond to James

## Updating data to table

```js
METHOD: PATCH

URL_STRUCTURE: http://localhost:8000/api/v1/service/authentication/db

HEADERS: Content-Type application/json , Devless-token get_it_from_the_app_section_of_DevLess

REQUEST PAYLOAD:
{  
   "resource":[  
      {  
         "name":"auth_table",
         "params":[  
            {  
               "where":"id,1",
               "data":[
                   {"username": "james"}
               ]

            }
         ]
      }

    ]
}        

RESPONSE PAYLOAD: {"status_code":619,"message":"table authentication updated successfully","payload":[]}
```

In other to update a field in the table you need to pass the`table name`and the`parameters`. The parameters will include the field name and new data as well as the`id`of that field as shown above.

Now that we have made changes to our table content we can now try to delete a field, truncate the table then delete the whole table.

## Delete data from table

```js
METHOD: DELETE

URL_STRUCTURE: http://localhost:8000/api/v1/service/authentication/db

HEADERS: Content-Type application/json , Devless-token get_it_from_the_app_section_of_DevLess

REQUEST PAYLOAD:
{  
   "resource":[  
      {  
         "name":"auth_table",
         "params":[  
            {  
               "delete":"true",         
               "where":"id,=,2"

            }
         ]
      }

    ]
}        

RESPONSE PAYLOAD: {"status_code":636,"message":"The table or field has been deleted","payload":[]}
```

This is very trivial and understandable as by now you would have noticed the pattern 😊

* To truncate the table you set the`truncate`parameter to`true`"truncate":true this returns a response as`{"status_code":636,"message":"data/table/field deleted successfully","payload":[]}`

* You may also drop the table by passing`true`to the`drop`parameter this also returns a response as`{"status_code":636,"message":"data/table/field deleted successfully","payload":[]}`

We have gone through a basic CRUD operation using the API engine. The next thing we are going to look at is the script .

Again whenever you create a service with the management console a scripting column is added. That's where your script lives. In case you want the complete API engine with the management console download it from[DevLess complete](https://docs.devless.io/docs/1.2.7/api-engine#devlesscomplete). Another way you can add a script is doing so with a database client

## Accessing rules

* Rules are run each time you make a call to any of the CRUD actions
* Each Service has a Rules section.
* Detailed explanation of how to use Rules can be found at
  All about Rules

## RPC Calls

DevLess Services come with in built functionalities that can be accessed via RPC calls. some of these functionalities include Authentication \(SignUp, Login, profile ..\). Also Every new service created has an ActionClass which is basically a class within which you can add methods. These methods are then accessible over RPC.

## Request structure

```js
METHOD: POST

URL_STRUCTURE: http://localhost:8000/api/v1/service/service_name/rpc?action=method_name

HEADERS: Content-Type application/json , Devless-token get_it_from_the_app_section_of_DevLess

REQUEST PAYLOAD:
{
      "jsonrpc": "2.0",
      "method": "service_name",
      "id": "1000",
      "params": []
}
```

## SignUp call via RPC

```js
METHOD: POST

URL_STRUCTURE: http://localhost:8000/api/v1/service/devless/rpc?action=signUp

HEADERS: Content-Type application/json , Devless-token get_it_from_the_app_section_of_DevLess

REQUEST PAYLOAD:
{
      "jsonrpc": "2.0",
      "method": "devless",
      "id": "1000",
      "params": ["moon@gmail.com", "password","username","03043355646","fname","lname","null"]
}      

RESPONSE PAYLOAD: {
  "status_code": 637,
  "message": "Got RPC response successfully",
  "payload": {
    "jsonrpc": "2.0",
    "result": {
      "profile": {
        "username": "jeff",
        "first_name": "fname",
        "last_name": "lname",
        "phone_number": "03043355646",
        "id": 42,
        "email": "moon@gmail.com"
      },
      "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.IntcInRva2VuXCI6XCIzMmJkOTgwNmQxN2U5YWZjN2UwYjMzMzU0MmQ5NTJjNlwifSI.GEdumRzlZHGPv5Do5DrezjHX_llLRoFUI1_u32J0vE0"
    },
    "id": "1000"
  }
}
```



