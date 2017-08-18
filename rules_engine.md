# Securing & Transforming data

DevLess comes with built-in tooling for securing and transforming data. For example, you can allow only admins to write to your "products" table or normalize all emails to be lowercase. 

## Securing data

By default, almost all endpoints in DevLess are public. This is good for enabling easy development, but bad for production. 

Securing data in DevLess can be done on two levels. One cruder, based on endpoint access rules, which applies to the entire service. You can also apply more fine-grained access by using the *rules engine*. The *rules* engine allows you to apply logic when reading from or writing to the database. This includes the ability to do fine-grained access checks.

### Endpoint Access Rules

You can change the endpoint access rules going to your DevLess admin panel, and clicking on the "privacy" menu item on the left. Here you can apply access rules for querying, writing etc. on a per-service basis. By default, almost all are public. 

* `PUBLIC` access means that anyone with an API key can use the resource. 
* `AUTHENTICATE` access means that **any** logged in user can use the resource. 
* `PRIVATE` access that no-one can access the resource. This can be overridden in the rules engine. For production ready systems, you probably want this on all resources.

### Securing using the rules engine

DevLess comes with a rules engine. The rules engine can be used to apply table-specific grants for reading or writing. You can access the rules which applies to your service in the DevLess admin UI. Under services, select your service and then select the "rules" tab.

Here you will see something like this (you might have to scroll in the editor):

```php
 -> beforeQuerying()
 -> beforeUpdating()
 -> beforeDeleting()
 -> beforeCreating()

 -> onQuery()
 -> onUpdate()
 -> onDelete()
 -> onCreate()

 -> onAnyRequest()

 -> afterQuerying()
 -> afterUpdating()
 -> afterDeleting()
 -> afterCreating()
 
 ```

Lets take a look at how to allow only admins to create entries in the `people` table. Start with setting the endpoint access rule for creating data to `PRIVATE`. We can now grant admins only access to creating data in the `people` table:

```php
-> beforeCreating()->onTable('people')->grantOnlyAdminAccess()
```

We can also make viewing the people table public:
```php
-> beforeQuerying()->onTable('people')->allowExternalAccess()
```

### Video tutorial

For a more in-depth and hands-on walk-through of securing data, there is [a video tutorial](https://www.youtube.com/watch?v=SOlXNSPFmOg).

## Transforming data

The rules engine can also transform data, both before writing to the database and after reading from it. This is done by using the same hooks as when securing data. 

### Before storing

For example, you can normalize all emails to be lowercase by using the beforeCreating hook. 

```php
->beforeCreating()->onTable("people")->convertToLowerCase($input_name)->storeAs($input_name)`
```
What happens here is that we for the `people` table convert all inputs for the field `name` to be lower-cased with the `convertToLowerCase` method. We then overwrite the `$input_name` variable with `storeAs`. The `$input_name` is the variable that will be stored in the database in the `name` field.

### Before returning data to the client

We can also manipulate data before we send it back to the client. There are three main methods for doing this.

For modifying the response message, use the `mutateResponseMessage(new_message)` method.  The message is used for notifying the client about what happened using in a textual format. You can use it to e.g. give a more context aware message:

```php
-> afterCreating()->onTable("people")->mutateResponseMessage("Contact added")`
```

For modifying the payload, use the `mutateResponsePayload(payload_to_add)`. This can be used to add additional information for your clients.

E.g. to add the timestamp at which the server returned the payload, we can do this: 

```php
->afterQuering()->onTable("people")->getTimeStamp()->storeAs($timestamp)->mutateResponsePayload(["timestamp"=>$timestamp])
```
We can also mutate the status code. This is for **advanced users only**. Modifying this will impact how SDKs and clients interpret the response, so proceed with caution. Use the `mutateStatusCode` method to change the status code.

E.g., to make all requests for a table 

### On reading from the database



### Controlling flow

{{ 'https://www.youtube.com/embed/Mwurl21niSw' | noembed}}



###Manipulate incoming data

{{ 'https://www.youtube.com/watch?v=z6CXQhcQz6I' | noembed}}

 
###Change the default DevLess output

{{ 'https://youtu.be/a2ScbtehNeE' | noembed}}

