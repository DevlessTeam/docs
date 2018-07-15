# HTTP API

DevLess has a rich restful API. It is used by all the SDKs. This means that everything that you can do in the SDKs, you can do using the REST API.

The REST API Can:

* Do CRUD actions on any data table
* Create/Delete services and tables in your DevLess instance
* Use [JSON RPC](http://www.jsonrpc.org/) to call a wide range of functions exposed by DevLess core or service extensions.

All operations in the REST API requires a header named `Devless-token`. This token is unique to each DevLess setup. It can be viewed by opening your DevLess GUI & pressing the "connect to DevLess" button in the top-right corner:  
![Connect to devless](../.gitbook/assets/connect_to_devless.png)

From there, select the "raw" tab. There you can view the DevLess token.

## Authentication

The authentication flow is implemented using DevLess' JSON RPC functionality. If you do not authenticate, you can still access all public data in DevLess.

You should log in as the end-user of your product. DevLess will make sure that the correct access is given, based on access rules.

### Signing Up

You can sign up new users to your application by calling the `signUp` method. This method is exposed over the JSON RPC interface. We can call it on `http://$DEVLESS_URL/api/v1/service/devless/rpc?action=signUp` using the `POST` http verb.

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

```javascript
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

The `status_code` is one of the codes defined in [DevLess Status Codes](devless-status-codes.md), `637` indicates success. The `message` is a human-readable message about the status of the call. the `payload` contains a `result`, which is the result from the `signUp` action. It contains the created profile, but also a `token`. The token is a [JSON Web Token](https://jwt.io/). This token should be used for further interaction with DevLess as this user by setting it as a header property.ie `-H "Devless-user-token:$DEVLESS_USER_TOKEN"`

### Logging In

Logging in is quite similar to signing up. We use the `login` action by calling `http://$DEVLESS_URL/api/v1/service/devless/rpc?action=login` with the `POST` verb:

```text
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

```javascript
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

### Logging Out

Logging out is straight forward. We use the `logout` action by calling `http://$DEVLESS_URL/api/v1/service/devless/rpc?action=logout` with the `POST` verb:

```text
curl -L -XPOST -H "Devless-token: $DEVLESS_TOKEN" "http://$DEVLESS_URL/api/v1/service/devless/rpc?action=login" -d@-   <<EOF
{
    "jsonrpc": "2.0",
    "method": "devless",
    "id": "1000",
    "params": []
}
EOF
```

**NB:** Again, the `status_code` is one of the codes defined in [DevLess Status Codes](devless-status-codes.md). As when signing up, the `token` is a [JSON Web Token](https://jwt.io/). This token should be used for further interaction with DevLess as this user by setting it as a header property.ie `-H "Devless-user-token:$DEVLESS_USER_TOKEN"`

### Get Profile 

Getting a logged in users profile is also straight forward. We use the `getProfile` action by calling `http://$DEVLESS_URL/api/v1/service/devless/rpc?action=getProfile` with the `POST` verb. Also make sure the user is already [logged in](http-api.md#logging-in) , as you will need the token issued on login to access the users profile. This token is to be set in the header of your request with the key `Devless-user-token`

```text
curl -L -XPOST -H "Devless-token: $DEVLESS_TOKEN" "Devless-user-token" "http://$DEVLESS_URL/api/v1/service/devless/rpc?action=getProfile" -d@-   <<EOF
{
    "jsonrpc": "2.0",
    "method": "devless",
    "id": "1000",
    "params": []
}
EOF
```

## CRUD

Lets say that we have set up a service named `contacts`, with a table named `people`. The table has one column for `name` and one for `email`.

### Create - Adding data to a table

We can add data to the table, by using the `POST` verb and sending in JSON data:

```text
$ curl -L -XPOST \
  -H "Content-type: application/json" \
  -H "Devless-token: $DEVLESS_TOKEN\
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

```javascript
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
| `orderBy` | Orders the data in ascending order based on the field provided |
| `where` | Selects data based on field values. The format is `where=field,value`. E.g. to select all people with the name "joe", use `&where=name,joe`. If multiple `where` statements are sent, only records matching all statements will be returned. |
| `orWhere` | Similar to `where`, but in the case of multiple statements, records matching _any_ of the statements will be returned. |
| `size` | Specifies how many records to return |
| `offset` | Specifies what offset to start with. Combine with size for pagination |
| `search` | Similar to the `where` parameter, but does partial matching. e.g. `&search=email,gmail` |
| `greaterThan` | Selects data where the column value is **greater than** the specified value. E.g. `&greaterThan=age,18`. |
| `greaterThanEqual` | Selects data where the column value is greater than **or equal to** the specified value. E.g. `&greaterThanEqual=age,18`. |
| `lessThan` | Selects data where the column value is **less than** the specified value. E.g. `&lessThan=age,18`. |
| `lessThanEqual` | Selects data where the column value is less than **or equal to** the specified value. E.g. `&lessThanEqual=age,18`. |
| `desc` | Orders the data in descending order based on the field provided E.g. `&desc=name` |
| `asc` | Orders the data in ascending order based on the field provided E.g. `&asc=name` |

For example, you can select 2 people both named `joe` like this:

```bash
curl -L -H "Devless-token: $DEVLESS_TOKEN"\
  "http://$DEVLESS_URL/api/v1/service/Contacts/db?table=people&orderBy=email&where=name,joe&size=2"
```

This will get you a response looking something like this:

```javascript
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

```javascript
{
  "status_code": 619,
  "message": "table people updated successfuly",
  "payload": []
}
```

### Deleting data

Deleting data is a lot like updating data. Instead of using the `PATCH` verb, we we use the `DELETE` verb. Like so:

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

```javascript
{
  "status_code": 636,
  "message": "Data / table / field has been deleted",
  "payload": []
}
```

## Creating or Deleting tables

So far, we have been looking at creating and updating resources within a table. However, we can also use HTTP APIs to create or delete tables.

### Creating tables

Creating a table is done by issuing a `POST` request to `http://$DEVLESS_URL/api/v1/service/contacts/schema`. A `name` of the new table is required, while the `description` is optional.

Please note that **access** for creating a table is **private by default**. This means that no-one can use it from the API. Change this by logging into the DevLess UI, go to the _Privacy_ tab on the left, select your service and set _Schema Access_ to `AUTHENTICATE` \(logged in users can create\) or `PUBLIC` \(everyone can create\).

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

```javascript
{
  "status_code": 606,
  "message": "Created table successfully",
  "payload": []
}
```

### Deleting all data in a table

Deleting all data in a table is known in the SQL world is _truncation_. DevLess follows this convention. Deleting, or truncating, all data in a table is done using the same endpoint and verb \(`DELETE`\) as deleting a single entry, but with different parameters.

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

## RPC Calls

As we saw when doing authentication, DevLess methods can be called using JSON RPC. This goes for both the built-in methods, as well as for any services that you install through the hub or write yourself.

E.g. if we have the Weather service installed we can call it to get how hot it is in Accra:

```bash
curl -L -XPOST \
  -H "Devless-token: $DEVLESS_TOKEN" \
  -H "Content-type: application/json" \
  "http://$DEVLESS_URL/api/v1/service/Weather/rpc?action=getCelciusTemperatureFor" \
  -d@- <<EOF
{
  "jsonrpc": "2.0",
  "method": "Weather",
  "id": "1000",
  "params": ["Accra", "Ghana"]
}
EOF
```

The `method` parameter as well as part of the URL path is the service name. The `action` query parameter specifies which method to call, and the `params` field the parameters to call the method with. See the documentation for each service for more details.

In this case, we get this response body back:

```javascript
{
  "status_code": 637,
  "message": "Got RPC response successfully",
  "payload": {
    "jsonrpc": 2,
    "result": 26,
    "id": 1000
  }
}
```

The `result` field is what we are looking for: it is 26 degrees Celsius in Accra.

