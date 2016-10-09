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

This segment holds the configuration of the instance. The instance and the admin details can be updated from here. App key and token to access the instance from within your application can be found here.

**NB** : Changing the app key or token will revoke access rights to Devless from an app connecting to the instance. This feature is very useful in situations where access to your backend is compromised.

<a name="services"></a>
## Services

Services are the core components of the backend instance. A service is a representation of a feature of your app. To have a complete application one might need one or more services installed and integrated to bring the backend features of the app to life. There are three ways to install a service into a Devless instance, i.e. [Creating](/docs/{{version}}/service) or [importing](#migration) one and from the [Service Hub](#hub).

<a name=“hub”></a>
The Service Hub is the store house for resuable services hosted by Devless. These services may be installed directly into the framework or downloaded and installed later.

<a name="privacy"></a>
 ## Privacy

Manage the access control level (ACL) of to the endpoints of Services. The ACL options are **Private** , **Authenticated** and **Public**. Any newly created service has its privacy set to **Private** which means no client application can access the Devless backend. The **Authenticated** option requires the client application to be [logged in](/docs/{{version}}/auth). This is handled by the **Devless SDK.** A client application can access the backend without authentication if the ACL on the service is set to **Public.**

<a name="api-console"></a>
 ## API Console

Run CRUD simulations on your services internally. Use our Postman like feature to visualize the expected response from the Devless backend. eg
- Select the Service
- Select the Table
- Choose the Operation (Query, Create, Update, Delete)
- Provide the necessary params. ***(optional)***.
- Hit Run

This returns a response with data into the response body section. Go to the [Devless API Engine docs](https://github.com/DevlessTeam/docs/blob/master/api-engine.md) for more details

<a name="data-tables"></a>
 ## Data Tables
**Service tables:**
An auto-generated table with entries based on the selected table name and its contents. To view data, select a service with the corresponding table name to generate the data-table.

**Users:**
List al users registered via the Devless     [auth](/docs/{{version}}/auth) 

<a name="migration"></a>
 ## Migration

Services can be imported and exported into and from the base framework.
This feature comes in handy when migrating your application from development to production, also when switching host providers.
