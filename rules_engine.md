# Securing & Transforming data

DevLess comes with built-in tooling for securing and transforming data. For example, you can allow only admins to write to your "products" table or normalize all emails to be lowercase. 

## Securing data

By default, almost all endpoints in DevLess are public. This is good for enabling easy development, but bad for production. 

Securing data in DevLess can be done on two levels. One cruder, based on endpoint access rules, which applies to the entire service. You can also apply more fine-grained access by using the *rules engine*. The *rules* engine allows you to apply logic when reading from or writing to the database. This includes the ability to do fine-grained access checks.

### Endpoint Access Rules

You can change the endpoint access rules going to your DevLess admin panel, and clicking on the "privacy" menu item on the left. Here you can apply access rules for querying, writing etc. on a per-service basis. By default, almost all are public. 

* `PUBLIC` access means that anyone with an API key can use the resource. 
* `AUTHENTICATE` access means that **any** logged in user can use the resource. 
* `PRIVATE` access that no-one can access the resource. This can be overridden in the rules engine. For production ready systems, you probably want this on all tables.

### Securing using the rules engine

DevLess comes with a rules engine. The rules engine can be used to apply table-specific grants for reading or writing. You can access the rules which applies to your service in the DevLess admin UI. Under services, select your service and then select the "rules" tab.

Here you will see something like this:

```php
 -> beforeQuerying()
 -> beforeUpdating()
 -> beforeDeleting()
 -> beforeCreating()
 

 -> onQuery()
 -> onUpdate()
 -> onDelete()
 -> onCreate()
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

For example, you can normalize all emails to be lowercase by using the beforeCreating hook. 

```php
->beforeCreating()->convertToLowerCase($input_name)->storeAs($input_name)`
```


