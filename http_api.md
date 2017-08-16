# REST API

DevLess has a rich restful API. It is used by all the SDKs. This means that everything that you can do in the SDKs, you can do using the REST API.

The REST API Can:

* Do CRUD actions on any data table
* Create/Delete services and tables in your DevLess instance
* Use [JSON RPC](http://www.jsonrpc.org/) to call a wide range of functions exposed by DevLess core or add-on modules.

All operations in the REST API requires a header named `Devless-token`. This token is unique to each DevLess setup. It can be viewed by opening your DevLess GUI & pressing the "connect to DevLess" button in the top-right corner:   
![Connect to devless](assets/connect_to_devless.png)

From there, select the "raw" tab. There you can view the DevLess token.

## Authentication

The authentication flow is implemented using DevLess' JSON RPC functionality. If you do not authenticate, you can still access all public data in DevLess.

You should log in as the end-user of your product. DevLess will make sure that the correct access is given, based on access rules.

### Signing Up

You can sign up new users to your application by calling the `signUp` method. This method is exposed over the JSON RPC interface. We can call it on  `http://$DEVLESS_URL/api/v1/service/devless/rpc?action=signUp` using the `POST` http verb.

```bash
$ curl -F -XPOST -H "Devless-token: $DEVLESS_TOKEN" "http://$DEVLESS_URL/api/v1/service/devless/rpc?action=signUp" -d@- <<EOF
{
    "jsonrpc": "2.0",
    "method": "devless",
    "id": "1000",
    "params": [
            "john@example.com",
            "password123",
            "john",
            "+12345678910",
            "John",
            "Doe",
            null
    ]
}
```

This is what the params are, in order: **email**, **password**, **username**, **phone\_number**, **first\_name**, **last\_name**, **role**.

Either an email, a username or a phone number needs to be provided. All other arguments except for the password are optional. Optional arguments that you don't want to set should be sent as `null`.

This will return a response like this:

```js
{
  "status_code": 637,
  "message": "Got RPC response successfully",
  "payload": {
    "jsonrpc": 2,
    "result": {
      "profile": {
        "username": "john",
        "first_name": "John",
        "last_name": "Doe",
        "phone_number": 12345678910,
        "id": 2,
        "email": "john@example.com",
        "status": true
      },
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
    },
    "id": 1000
  }
}
```

The `status_code` is one of the codes defined in [DevLess Status Codes](devless-status-codes.md), `637` indicates success. The `message` is a human-readable message about the status of the call. the `payload` contains a `result`, which is the result from the `signUp` action. It contains the created profile, but also a `token`. The token is a [JSON Web Token](https://jwt.io/). This token should be used for further interaction with DevLess as this user.

### Logging In

Logging in is quite similar to signing up. We use the `login` action by calling `http://$DEVLESS_URL/api/v1/service/devless/rpc?action=login` with the `POST` verb:

```
curl -L -XPOST -H "Devless-token: $DEVLESS_TOKEN" "http://$DEVLESS_URL/api/v1/service/devless/rpc?action=login" -d@-   <<EOF
{
    "jsonrpc": "2.0",
    "method": "devless",
    "id": "1000",
    "params": [
            "john",
            "john@example.com",
            "+12345678910",
            "password123"
    ]
}
EOF
```

This is what the params are, in order: **username**, **email**, **phone\_number**, **password**.

Either an email, a username or a phone number needs to be provided. The password is required.

This will return a response like this:

```js
{
  "status_code": 637,
  "message": "Got RPC response successfully",
  "payload": {
    "jsonrpc": 2,
    "result": {
      "profile": {
        "username": "john",
        "first_name": "John",
        "last_name": "Doe",
        "phone_number": 12345678910,
        "id": 2,
        "email": "john@example.com",
        "role": false
      },
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
    },
    "id": 1000
  }
}
```

Again, the `status_code` is one of the codes defined in [DevLess Status Codes](devless-status-codes.md). As when signing up, the `token` is a [JSON Web Token](https://jwt.io/). This token should be used for further interaction with DevLess as this user.

## CRUD

Lets say that we have set up a service named `contacts`, with a table named `people`. The table has one column for `name` and one for `email`.

### Create - Adding data to a table

We can add data to the table, by using the `POST` verb and sending in JSON data:

```
$ curl -L -XPOST \
  -H "Content-type: application/json" \
  -H "Devless-token: $DEVLESS_\
  "http://$DEVLESS_URL/api/v1/service/contacts/db" -d@- <<EOF
{
  "resource":[{
    "name":"people",
    "field":[{
      "name":"joe",
      "email":"joe@example.com"
    }]
  }]
}
EOF
```

Note that "contacts", the name of the service, becomes part of the URL. The `name` param indicates which table to store data in.

This will give you a response looking something like this:

```js
{
  "status_code": 609,
  "message": "Data has been added to people table successfully",
  "payload": {
    "entry_id": 7
  }
}
```

Again, the status code corresponds to one of the [Status Codes](devless-status-codes.md), `609` indicates that data was added successfully. You also get back the `id` of the created resource. This is an automatically generated unique identifier for your resource.

### Read - Query data from table

Querying data can be done by using the `GET` verb on the same path as when adding data. This can be combined with some very powerful query parameters:

| Query Param | What |
| :--- | :--- |
| `table` | Specifies which table to query from. |
| `orderBy` | Orders the data in descending order based on the field provided |
| `where` | Selects data based on field values. The format is `where=field,value`. E.g. to select all people with the name "joe", use `&where=name,joe`. If multiple `where` statements are sent, only records matching all statements will be returned. |
| `orWhere` | Similar to `where`, but in the case of multiple statements, records matching _any_ of the statements will be returned. |
| `size` | Specifies how many records to return |
| `offset` | Specifies what offset to start with. Combine with size for pagination |
| `search` | Similar to the `where` parameter, but does partial matching. e.g. `&search=email,gmail` |
| `greaterThan` | Selects data where the column value is **greater than** the specified value. E.g. `&greaterThan=age,18`. |
| `greaterThanEqual` | Selects data where the column value is greater than **or equal to** the specified value. E.g. `&greaterThanEqual=age,18`. |
| `lessThan` | Selects data where the column value is **less than** the specified value. E.g. `&lessThan=age,18`. |
| `lessThanEqual` | Selects data where the column value is less than **or equal to** the specified value. E.g. `&lessThanEqual=age,18`. |

For example, you can select 2 people both named `joe` like this:

```bash
curl -L -H "Devless-token: $DEVLESS_TOKEN"\
  "http://$DEVLESS_URL/api/v1/service/Contacts/db?table=people&orderBy=email&where=name,joe&size=2"
```

This will get you a response looking something like this:

```js
{
  "status_code": 625,
  "message": "Got response successfully",
  "payload": {
    "results": [
      {
        "id": 3,
        "devless_user_id": 1,
        "email": "joe@example.com",
        "name": "joe"
      },
      {
        "id": 8,
        "devless_user_id": 1,
        "email": "joe@gmail.com",
        "name": "joe"
      }
    ],
    "properties": {
      "count": 4,
      "current_count": 2
    }
  }
}
```

Next, we are going to change the username from Edmond to James

### Updating data

Data can be updated by using the `PATCH` verb. For example:

```bash
curl -L -XPATCH \
  -H "Devless-token: $DEVLESS_TOKEN" \
  -H "Content-type: application/json" \
  "http://$DEVLESS_URL/api/v1/service/contacts/db" \
  -d@- <<EOF
{
  "resource":[{
    "name":"people",
    "params":[{
      "where":"id,3",
      "data":[
        {"email": "joe@foobar.com"}
      ]
    }]
  }]
}
EOF
```

Again, note that the service name, `contacts` is embedded in the URL. The `name` parameter refers the table name. For parameters, we need to supply a `where` clause. This selects which record\(s\) to be updated. The `data` part describes what updates to do to the record.

The response looks like this:

```js
{
  "status_code": 619,
  "message": "table people updated successfuly",
  "payload": []
}
```

### Deleting data

Deleting data is a lot like updating data. Instead of using a `PATCH` verb, we we use the `DELETE` verb. Like so:

```bash
curl -L -XDELETE \
  -H "Devless-token: $DEVLESS_TOKEN" \
  -H "Content-type: application/json" \
  "http://$DEVLESS_URL/api/v1/service/contacts/db" \
  -d@- <<EOF
{
  "resource":[{
    "name":"people",
    "params":[{
      "where":"id,3",
      "delete": true
    }]
  }]
}
EOF
```

Again, the name of the service is in the URL, and the `name` indicates the table. The `where` clause selects which entry to delete. We also specify `delete` to be true: this is what indicates that we want to delete this record.

The response looks similar to the update call as well:

```js
{
  "status_code": 636,
  "message": "Data / table / field has been deleted",
  "payload": []
}
```

* To truncate the table you set the`truncate`parameter to`true`"truncate":true this returns a response as`{"status_code":636,"message":"data/table/field deleted successfully","payload":[]}`

* You may also drop the table by passing`true`to the`drop`parameter this also returns a response as`{"status_code":636,"message":"data/table/field deleted successfully","payload":[]}`

We have gone through a basic CRUD operation using the API engine. The next thing we are going to look at is the script .

Again whenever you create a service with the management console a scripting column is added. That's where your script lives. In case you want the complete API engine with the management console download it from[DevLess complete](https://docs.devless.io/docs/1.2.7/api-engine#devlesscomplete). Another way you can add a script is doing so with a database client

## Creating or Deleting tables

So far, we have been looking at creating and updating resources within a table. However, we can also use HTTP APIs to create or delete tables.

### Creating tables

Creating a table is done by issuing a `POST` request to `http://$DEVLESS_URL/api/v1/service/contacts/schema`. A `name` of the new table is required, while the `description` is optional.

Please note that **access** for creating a table is **private by default**. This means that no-one can use it from the API. Change this by logging into the DevLess UI, go to the _Privacy_ tab on the left, select your service and set _Schema Access_ to `AUTHENTICATE` \(logged in can create\) or `PUBLIC` \(everyone can create\).

Creating a table also entails specifying which fields it should have. For each field, parameters can be specified:

| Param | Required? | What |
| :--- | :---: | :--- |
| `name` | **yes** | The name of the field. Must be lowercase letters or underscore \(\_\). |
| `field_type` | **yes** | This is the type of field. It specifies both the database representation and validation rules. Valid values are: `text`, `textarea`, `integer`, `decimal`, `password`, `percentage`, `url`, `timestamp`, `boolean`, `email` and `reference`. The `password` type will make DevLess automatically apply cryptographic hashing. |
| `required` | **yes** | Specifies if all records in the table need to have this value |
| `default` | no | The default value for this field. Only applies to fields which are not `required`. |
| `is_unique` | **yes** | Sets if the field is a unique key for records in this table. |
| `ref_table` | no | Only valid if the `reference` field type is used. For references, this specifies which field it refers to. DevLess automatically creates the relationship between the field and primary key of the table being referenced. You can use any table as the reference, including the built in`_devless_users` table. |

For example, I could create a table listing my favorite horses like this:

```bash
curl -L -XPOST -H "Content-type: application/json"  -H "Devless-token: $DEVLESS_TOKEN" http://$DEVLESS_URL/api/v1/service/Contacts/schema -d@- <<EOF
{
  "resource":[{
    "name":"horses",
    "description":"A list of all my favorite horses.",
    "field":[
      {
        "name":"name",
        "field_type":"text",
        "default":null,
        "required":true,
        "is_unique":false,
        "ref_table":"",
        "validation":true
      },
      {
        "name":"age",
        "field_type":"integer",
        "default":0,
        "required":false,
        "is_unique":false,
        "ref_table":"",
        "validation":true
      }
    ]
  }]
}
EOF
```

The result would look something like this:

```js
{
  "status_code": 606,
  "message": "Created table successfully",
  "payload": []
}
```

### Deleting all data in a table

Deleting all data in a table is known in the SQL world is _truncation_. DevLess follows this convention. Deleting, or truncating, all data in a table is done using the same endpoint and verb (`DELETE`) as deleting a single entry, but with different parameters.

I.e. to delete the `horses` created above, we can do this: 
```bash
curl -L -XDELETE \
  -H "Devless-token: $DEVLESS_TOKEN" \
  -H "Content-type: application/json" \
  "http://$DEVLESS_URL/api/v1/service/contacts/db" \
  -d@- <<EOF
{
  "resource":[{
    "name":"horses",
    "params":[{
      "truncate": true
    }]
  }]
}
EOF
```
TODO: This does not work atm



### Delete a table

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

**NB:**To get started you need to create a new service either via the [developer console](/developer-portal.md) or from a database management client. In our case it is assumed we have done that and the name of the service is authentication.

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

**Explanation**: From the URL after the`api/v1/service` route, the service\_name `authentication`is passed followed by the`schema`sub-route which is the route for creating new tables for any service.

Also, the request payload takes the name of the table to be created and is prefixed with the service name during the creation of the table itself to ensure tables from other services do not conflict. You may also add a table description but this is optional

For the`field`, there are a couple of options you have to provide with some being optional and others required.

The`Response Body`provides three piece of information: the`status_code`in this case`606`, a`message`and`payload`which might contain extra information or return data. Here are the list of all the [status codes](/status-code.md)

Next step would be adding some data to the table

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



