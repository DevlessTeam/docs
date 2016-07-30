**Management Console**

The management console is used to administrate your Devless instance. The console is segmented into App, Services, Privacy, API Console, Data tables and Migration. Below is a detailed breakdown of each section.


- **App**

These segment holds the configuration of the instance. The instance and the admin details can be updated from here. App key and token to access the instance can be found here.

**NB** : Changing the app key or token will revoke access rights to Devless from an app connecting to the instance. This feature is very useful in situations where access to your backend is compromised.


- **Services:**

Services are the core components of the backend instance. A service is a representational backend feature of your app. To have a complete application one might need one or more services installed and integrated to bring the backend features of the app to life. There are two ways to install a service into a Devless instance, i.e. creating or importing them.


- **Privacy**

Manage the access control level (ACL) of your services (endpoints). The ACL options are **Private** , **Authenticated** and **Public**. Any newly created service has its privacy set to **Private** which means no client application can access the Devless backend. The **Authenticated** options requires the client application to be logged in. This is handled by the **Devless SDK.** A client application can access the backend without authentication if the ACL on the service is set to **Public.**


- **API Console**

Run CRUD and Scripts simulations on your services internally. Use our Postman look alike feature to visualize the expected response from the Devless backend. For instance,

- Select the Service 
- Select the Table 
- Choose the Operation (Query, Create, Update, Delete & Run Script)
- Provide the necessary params. ***(optional)***.
- Hit Run

This returns a response with data into the response body section. Go to the [Devless API Engine docs](https://github.com/DevlessTeam/docs/blob/master/api-engine.md) for more details


- **Data Tables**

An auto-generated table with entries based on the selected table name and its contents. To view data, select a service with the corresponding table name to generate the data-table.


- **Migration**

Services can be imported and exported into and from the base framework. To import a service, head to the Devless Store and grab your preferred service and plug it into the framework.
This feature comes in handy when migrating your application from development to production, also when switching host providers.