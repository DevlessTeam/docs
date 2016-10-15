## MANAGEMENT CONSOLE

- [App](#app)
- [Services](#services)
- [Service Hub](#hub)
- [Privacy](#privacy)
- [API Console](#api-console)
- [Data Tables](#data-tables)
- [Migration](#migration)


The management console is used to administer your Devless instance. The console is segmented into App, Services, Privacy, API Console, Data tables and Migration. Below is a detailed breakdown of each section.

<a name="app"></a>
## App

This segment holds the configurations of the instance. The instance and the admin details can be updated from here. App key and token to access the instance from within your application can also be found here.

**NB** : Changing the app key or token will revoke access rights to Devless from an app connecting to the instance. This feature is very useful in situations where access to your backend is compromised.

<a name="services"></a>
## Services

Services are the core components of the backend instance. A service is a representation of a feature of your  app. To have a complete application one might need one or more services installed and integrated to bring the backend features of the app to life. There are three ways to install a service into a Devless instance, i.e. [Creating](/docs/{{version}}/service) or [importing](#migration) one and from the [Service Hub](#hub).

<a name="hub"></a>
## Service Hub

The Service Hub is the store house for resuable services hosted by Devless. These services may be installed directly into the framework or downloaded and installed later.

<a name="privacy"></a>
 ## Privacy

Manage the access control level (ACL) of  Service endpoints . The ACL options are **Private** , **Authenticated** and **Public**. Any newly created service has its privacy set to **Private** which means no client application can access endpoints of that particular service . The **Authenticated** option requires the client application user to be [logged in](/docs/{{version}}/authentication). This is handled by the [Devless SDKs](/docs/{{version}}/SDKs). A client application can access the backend without authentication if the ACL on the service is set to **Public.**

<a name="api-console"></a>
 ## API Console

Run CRUD simulations on your services internally. Use our Postman like feature to visualize the expected response from the Devless backend.
### Steps
- Select the Service
- Select the Table
- Choose the Operation (Query, Create, Update, Delete)
- Provide the necessary params. ***(optional)***.
- Hit Run

This returns a response with data into the response body section. Checkout [Devless API Engine docs](https://github.com/DevlessTeam/docs/blob/master/api-engine.md) for detailed explanation.

<a name="data-tables"></a>
 ## Data Tables
**Service tables:**
An auto-generated table with entries based on the selected table name and its contents. To view data, select a service with the corresponding table name to generate the data-table.

**Users:**
List all users registered via  Devless [authentication](/docs/{{version}}/authentication) 

<a name="migration"></a>
 ## Migration

Services can be imported and exported into and from the base framework respectively.
This feature comes in handy when migrating your application from development to production, also when switching host providers.
