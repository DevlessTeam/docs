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

DevLess comes with a rules engine. The rules engine can be used to apply table-specific grants, and to 