## API Engine

  - [DevLess API Engine(DAE)](#DevLess-API-Engine(DAE))
  - [DAE](#DAE)
  - [What is the DevLess API engine](#What-is-the-DevLess-API-Engine)
  - [Features of the API engine](#Features-of-the-API-engine-include)
  - [Structure of the Authentication Service](#Structure-of-the-Authentication-Service)
  - [Creating a table](#Creating-the-table)
  - [Adding data to table](#Adding-data-to-table)
  - [Query data from  table](#Query-data-from-table)
  - [Updating data in table](#Updating-data-to-table)
  - [Delete data from table](#Delete-data-from-table)
  - [Accessing Rules](#Accessing-scripts)
  - [RPC Calls](#rpc)
  - [Lean View](#Lean-View)

<a name="DevLess-API-Engine(DAE)"></a>
## DevLess API Engine(DAE)
**DevLess API Engine(DAE)** is an open source API engine that generates CRUD API access to databases as well as allows the execution of rules against data.

The current implementation of the DevLess API Engine is in PHP and on top of the Laravel framework.

<a name="DAE"></a>
## DAE
**DAE** can be used as a standalone (accessed solely via API calls) however a management console is provided to interact with the API engine and is available in the complete [DevLess suite](https://github.com/DevlessTeam/DV-PHP-CORE).

This document explains the various syntax for accessing and working with the DevLess API Engine.

<a name="What-is-the-DevLess-API-Engine"></a>
## What is the DevLess API Engine?
The DevLess API Engine is  a Laravel based backend engine that generates RESTful endpoints by connecting to a data access point (database). It also has the ability to execute rules based on scripts within a sandbox.

**Installation procedure**
#### [README](https://github.com/DevlessTeam/DV-PHP-CORE/blob/master/readme.md)

<a name="Features-of-the-API-engine-include"></a>
## Features of the API engine include :

**Database access**
* Create  database tables

* Add data to tables

* Query tables

* Updates data in  tables

* Truncate, delete and drop  tables

**Rules**
* Run rules within sandbox

**Views**
* Access to HTML views

As you would have noticed from above the DevLess API Engine(DAE) works with three main entities tables, rules and  "lean views"  each known as a **RESOURCE** .

You can also pair up resources together known as  **SERVICES**  and this is responsible for one functionality of the **APPLICATION**.

In response to this order of organisation the restful endpoints are generated in the form:
``` <hostname>\service\<service_name>\<resource_name>\ ```


eg: ``` https:\\demo.devless.io\service\authentication\db?table=auth_table``` *(this example gets all data from the auth_table)*

**Endpoints definition for specific resources and actions**

For the rest of the documentation, we will assume creating a user authentication service.

<a name="Structure-of-the-Authentication-Service"></a>
## Structure of the Authentication Service

* Table name is auth_table

**Fields required include**
* username (string)
* password (hashed string)

Also, we assume our server is on  http://localhost:8000/   .

**NB:** To get started you need to create a new service either via the management console or from a database management client. In our case it is assumed we have done that and the name of the service name is authentication.

<a name="Creating-the-table"></a>

<a name="Creating-the-table"></a>
## Creating the table
```
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

**Explanation** : From the URL after the ``api/v1/service`` route, the service_name ``authentication`` is passed followed by the ``schema`` sub-route which is the  route for creating new tables for any service.

Also, the request payload takes the name of the table to be created and is prefixed with the service name during the creation of the table itself to ensure tables from other services do not conflict.
You may also add a table description but this is optional

For the ``field`` , there are a couple of options you have to provide with some being optional and others required.

``"name"`` this is the name to be given to the field and this is required.

``"field_type"`` this is a soft data type and is required as well. The options available are shown below on the left and their translated database type on the right

    'text'       => 'string',
    'textarea'   => 'longText',
    'integer'    => 'integer',
    'decimal'      => 'double',
    'password'   => 'string',
    'percentage' => 'integer',
    'url'        => 'string',
    'timestamp'  => 'timestamp',
    'boolean'    => 'boolean',
    'email'      => 'string',
    'reference'  => 'integer',
    'base64'     => 'base64',

**NB:** The password ``"field_type"``  hashes any data provided in the name field automatically.

* ``"default"``  Allows you to setup a default value for the field.

* ``"required"`` You can make the field a required field.

* ``"is_unique"`` States whether this field should be unique or not.

* ``"ref_table"`` also in the case where the ``field_type`` you chose was a ``reference`` type you would have to  select the table you would like to reference . The engine automatically creates the relationship between the  field and primary key of the table being referenced. Also you can set the `ref_table` to _devless_users this will reference the users table.

The ``Response Body`` provides three pieces of information:
 the ``status__code`` in this case ``606``, 
 a ``message`` and 
 ``payload`` which might contain extra   information or return data.
 You may find the complete list of status code in the **[appendix](#appendix)**

Next step would be adding some data to the table

<a name="Adding-data-to-table"></a>
## Adding data to  table
```
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


We will repeat the above by replacing the name ``Edmond`` with ``Charles`` and keeping the password same. So now we have two records in auth_table.

Now we can query our table


<a name="Query-data-from-table"></a>
## Query data from  table
```
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
Now we query the auth_table using the get parameters provided by the engine.
* The ``table`` parameter is used to provide the table name.
* The ``orderBy`` parameter makes it possible to arrange the data in descending order based on the field provided.
* The ``where`` parameter is used to get data where the field provided before the comma contains the data passed. ``eg: &where=name,edmond``
* The ``orWhere`` parameter allows for quering data that might contain a keyword. ``eg: &orWhere=name,edmond``.
* The ``size`` parameter is used to set the total number of records to be returned.
* The ``offset`` parameter sets an offset on the record.
* The ``search`` parater allows to search columns of a table that might contain a keyword.``eg: &search=name,edmond``.

Next, we are going to change the username from Edmond to James


<a name="Updating-data-to-table"></a>
## Updating data to table
```
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

In other to update a field in the table you need to pass the ``table name`` and the ``parameters``. The parameters will include the field name and new data as well as the ``id`` of that field as shown above.


Now that we have made changes to our table content we can now try to delete a field, truncate the table then delete the whole table.


<a name="Delete-data-from-table"></a>
## Delete  data from  table

```
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

This is very trivial and understandable as by now you would have noticed the pattern :)

* To truncate the table you set the ``truncate`` parameter to ``true`` "truncate":true  this returns a response as ```{"status_code":636,"message":"The table or field has been truncated","payload":[]}```

* You may also drop the table by passing ``true`` to the ``drop`` parameter this also returns a response as ``{"status_code":636,"message":"dropped table successfully","payload":[]}``



We have gone through a basic CRUD operation using the API engine. The next thing we are going to look at is the script .

Again whenever you create a service with the management console a scripting column is added. That's where your script lives. In case you want the complete API engine with the management console download it from [DevLess complete](#devlesscomplete). Another way you can add a script is doing so with a database client


<a name="Accessing-rules"></a>
## Accessing rules
- Rules are run each time you make a call to any of the CRUD actions
- Each Service has a Rules section.
- Detailed explanation of how to use Rules can be found at [Services](/docs/{{version}}/service)#scripts

<a name="rpc"></a>
## RPC Calls
DevLess Services come with in built functionalities that can be accessed via RPC calls. 
some of these functionalities include Authentication (SignUp, Login, profile ..).
Also Every new service created has an [ActionClass](/docs/{{version}}/service)#actionclass which is basically a class within which you can add methods. These methods are then accesible
over RPC.

## Request structure

```
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

## SignUp call via RPC. 

```
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

<a name="Lean-View"></a>

<a name="Lean-View"></a>
## Lean View
One last resource we need to look at is the ``Lean View``.

This provides a way to prepare a simple management console for each service .

For instance in the case of the authentication service we created above , we might want to throw in a simple dashboard to display how many users we have and how many of them are active.
