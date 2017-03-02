## Services

- [Creating Services](#create-service)
- [Database Setup](#database-setup)
- [Working with Tables](#tables)
- [Scripts](#scripts)
- [Action Classes](#actionclass)
- [Console](#console)


<a name="create-service"></a>
## Creating Services
To create a **service**
- Head over to the services tab from the management console
- Click on **Create Service**
- Fill out the service name and description skipping the [advance database options](#database-setup) and hit Create
- Once the service is created you will be redirected to the new service panel.
- From here you can do a whole lot with your newly created service.

**NB:** DevLess(in both lower and upper case)cannot be set as a service name as this is a reserved keyword and represents the DevLess service "parent service" from which all other services inherit.

<a name="database-setup"></a>
## Database Setup
Services are the backbone of the DevLess framework. DevLess allows each service to connect to different data sources if needed. But by default DevLess uses the underlying database for every service unless otherwise specified.
During the creation of a service you may specify the database options else leave as is to use the default.


<a name="tables"></a>
## Working with Tables
**Creating Tables:** Once you have successfully created a service you can start creating tables.

To create a table :

- Hit on the **TABLES** tab after selecting the service for which you want to create the table .
- Next hit **New Table** to add a new table to the service.
- Fill out the modal form with the **Table Name** , **Description(optional)**
and for the **Add Fields** section you can begin creating fields as needed.

You will need to provide :

- **Field Name**: This will be the name of the field.
- **Field Type**: This allows you to pick the data type as well as validation to associate with this field. You may select from the dropdown the field type that applies.
- **Reference Table**: In the case where you have already created a table for the particular service, you can  reference that table by selecting it. The referencing is automatically done via the id from within DevLess. Also you may reference a user from within the framework, which is available within the dropdown.
- **Default Value(optional)**: You may also set the default value for the field.
- **REQUIRED**: Once this option is ticked the client app has to always make sure this field is filled else the request is rejected by DevLess.
- **UNIQUE FIELD**: On ticking this no two entries may have the same value.

**Add a Field**: This allows you to add more fields.

**NB:** You are not required to add an id to the table as this is done automatically within DevLess.	

Once you are satisfied with your field entries hit on Create Table to create it.
Once the table is created successfully it should show up on the **TABLES** tab. You can now try accessing the table from the [Api Console](/docs/{{1.0}}/management-console/#api-console).

<a name="scripts"></a>
## Working with Rules
**A Rule** is the glue feature of a service. It is based on the philosophy that if there is any logic that can be processed within the client without leaving the system **vulnerable**  then it should be done leaving the backend to **validate**, **persist data** and perform **actions** that is possible only on the backend, in this case DevLess. Rules are equiped to validate and perform actions based on assertions.
The core utilites available within a Rules are :
- An Assertion utility lib
- Flow Control
- Persistence actions
- Global vars and input parameters

A newly created service will have the below script template available

```
use App\Helpers\Assert as Assert;  
 $rules
 -> onQuery()
 -> onUpdate()
 -> onDelete()
 -> onCreate()

```

``use App\Helpers\Assert as Assert; `` represents the importation of the assertion library provided by DevLess. You can also import your own assertion library. 

`` $rules `` is an instantiation of the rules engine.

``-> onQuery()
 -> onUpdate()
 -> onDelete()
 -> onCreate()
 ``  represents the persistence actions available within Rules.

 **Example** say you had a table with fields ``name`` and ``age``. You can check if the age is equal to a certain value ``onCreate()`` and then perform a particular action from an [ActionClass](#actionclass).

 Code below
 ```
 use App\Helpers\Assert as Assert;  
 $rules
 -> onQuery()
 -> onUpdate()
 -> onDelete()
 -> onCreate()
				->whenever(Assert::Eq(25, $input_age))
								->run('service_name', 'methodname',[])

 ```
 The above code is always run once a call is made to the service and the methods attached to the chain after the ``onCreate()`` method will be executed when the client attempts to add data to any of the service tables.

 ``->whenever(Assert::Eq(25, $input_age))`` states that whenever the input age (``$input_age``) is equal (``Assert::Eq()``) to ``25`` the ``methodname`` from the [ActionClass](#actionclass) ``service_name`` should be run

 **Flow Control**

 Rules provide a flow control similar to if else statements in most programming languages.

 **DevLess flow control**

 ```
 -> whenever('assert here')
 		-> run()
 -> elseWhenever('assert here')
		-> run()
 -> otherwise()
 ```

similar to

```
if('assert here'){
	'expression'
} else if('assert here'){
	'expression'
} else{}
```    

You can have as many elseWhenever statements in between  ``->whenever('assert here')`` and ``->otherwise()`` just like with ``else if(){}`` in the case of ``if(){}`` and ``else{}``.

 **Assertions**:
As with every flow control system there is a need for a statement to assert to either true or false to direct the flow. In the case of Rules DevLess provides a  [library](/docs/{{version}}/assertions) full of methods to help with assertions e.g ` Assert::Eq($value1, $value2)  notEq($value, $value2) range($value, $min, $max)
endsWith($value, $suffix) regex($value, $pattern)` etc

For the list of all assetions available please check out the [Assertion Library](/docs/{{version}}/assertions)

Also there are four ways to execute logic once an assetion passes

- ``run('service_name', 'method_name', ['param1'])`` //running an action class
- ``succeedWith('message goes here')``
- ``failWith('message goes here')``

**Example of a complete rules flow**

```
use App\Helpers\Assert as Assert;
	$rules
	-> onCreate()
		-> whenever(Assert::Eq($input_name, 'james'))
			-> run('notify', 'email',['user_email'=>$input_email])

-> elseWhenever(Assert::Eq($input_name,'ema'))
	-> succeedWith('Am not sending you an email')

-> otherwise()
	-> failWith('well non of the above flow statements asserted to true');
```


Also  **onTable('table_name')** is method is provided incase you want to run a block of code only if a particular table is selected. 

eg:

``` 

use App\Helpers\Assert as Assert;
 $rules
 
 -> onQuery()
        ->whenever('wealth' == 'success')
            ->onTable('users_table')
                ->succeedWith("on Table")
        ->elseWhenever('success' == 'success')
            ->onTable('products_table')
                    ->succeedWith(" On  trail")
 -> onCreate()
 -> onUpdate()
 -> onDelete()

```

 **Available scope Variables**

 Within the scope of a script there are list of useful variables made available:
 - ``$user_id``: This provides the id of the user placing the request provided they are logged in
 - ``$user_token``: This is the JSON Web Token(JWT) of the user making the request provided they are logged in. 
 - ``$input_*``: This is a prefixed variable usually set when the request comes with parameters. eg when a user makes a ``addData()`` request  using any of the SDKs with say a parameter body like ``{"name":"edmond", "age":34}`` Rules will make "edmond" available under the variable name ``$input_name`` and "age" accessed via ``$input_age`` respectively.

<a name="actionclass"></a>
## Action Classes
An **ActionClass** is a class with the class name same as the name of the service. The Action Class can be found under ``"resources/views/service_views/service_name/ActionClass.php"`` where ``service_name`` is the name of the service.

In the event where there is more to the implementation of your backend you may want to write out logic in DevLess native language ``PHP`` and methods from the Action Class would be made available within Rules and also to the client application via the available [SDKs](/docs/{{version}}/SDKs).
An example of a typical Action Class:
```
<?php

/**
 * Created by DevLess.
 * Author: eddymens
 * Date Created: 9th of October 2016 08:30:41 AM
 * @Service: demo
 * @Version: 1.0
 */

//Action method for serviceName
class demo
{
		public $serviceName = 'demo';

		/**
		 * Sample method accessible to via endpoint
		 * @ACL public
		 */
		public function methodone()
		{
				return "Sample Protected Method";
		}

		/**
		 * Sample method accessible only by authenticated users
		 * @ACL protected
		 */
		public function methodtwo()
		{
				return "Sample Protected Method";
		}

		/**
		 * Sample method not accessible via endpoint
		 * @ACL private
		 */
		public function methodthree()
		{
				return "Sample Protected Method";
		}

		/**
		 * This method will execute on service importation
		 * @ACL private
		 */
		public function __onImport()
		{
				//add code here
		}

		/**
		 * This method will execute on service exportation
		 * @ACL private
		 */
		public function __onDelete()
		{
				//add code here

		}

}
```

Each method within the Action Class is decorated within the comment section with the kind of access rights you want to attach to it. eg: `` @ACL private`` denotes that no client can be able to call on that method also `` @ACL protected`` requires the client to be authenticated before being granted access to the method. A method may be made open and available to every client by setting the method access rights to ``@ACL public``.

**NB**: A method without an @ACL decorator is not registed as an Action Class method.

By default on creating a service a couple of methods are generated two of the most important methods are ``__onImport()`` : Code contained in this method will be executed when the service is imported into a DevLess instance. and ``__onDelete()`` will execute whenever you export a service from a DevLess instance.

All of DevLess internals are available to service Action Classes.

Two of the most important utilities available within the Action Class includes:
[DataStore](/docs/{{version}}/datastore)
and the
[PHP SDK](/docs/{{version}}/SDKs)

<a name="console"></a>
## Console
**Console** a.k.a lean views can be said to be the eye of each service. A simple admin manager may be created to micro manage the service.
The entry point of views for a service by name "service_name" is ``"resources/views/service_views/service_name/index.blade.php"``.

There are a bunch of helpers available to make the development of service views fairely easy eg:

- ``DvAssetPath($payload, $partialAssetPath)`` //get asset files from the assets folder eg: ``DvAssetPath($payload, 'js/main.js')``
- ``DvAdminOnly("Sorry you don't have access to this page")`` //set ontop to make sure non "Admins" cant access the file or page
- ``<a href="<?= DvNavigate($payload, 'pageName'); ?>" />``
- ``DvRedirect($url, $time);`` //redirect the page to the given url

By default ``$payload`` is set  and contains every information regarding the particular service

**NB**: We are working hard to make available a quick and easy way to build admin panels for services. Also data management is easy with the Data Tables.
