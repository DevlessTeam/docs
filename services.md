### Module Development

You may have used used a few modules from the [service hub](/using_services.md)  or Interested in how they work , even better interested in creating your own.

#### Setting up

To begin developing modules you will have to setup a [DevLess instance on your local ](/dev-setup.md).

Next create a service . Give it a **name** and **description** that best describes your module .

Modules are an extended version of Services. ie It can be shared and used by many people besides  the original author .

When you create a service several files are also generated internally. These files can be found within the DevLess source. 

`resources/views/service_views/<service_name> ` .

In there you should find two files `ActionClass.php` and `index.blade.php` .Also you will find an `assets` directory



#### Adding functionalities

Functionalities that can be accessed from within your client or other services can be created in the `ActionClass.php` file .

The **ActionClass**  is a `PHP` Class with a list of sample methods . 

These methods can be accessed via any of the [DevLess SDKs](/sdks.md) or [REST Endpoint. ](/http_api.md)   via  the call method. 

 Assuming you labelled you rnew module contacts the you should be able to access the `samplePublicMethod` in the **ActionClass** of the contacts module EG: using the  [JS SDK](/sdks.md)     `SDK.call('contacts', 'samplePublicMethod', [], function(resp){alert(resp);})` this should return `public hello`  . 

Now you may go ahead and add your own methods with functionalities you require. 

Here is an in dept video explaining the  process 

#### Setting up Privacy for your methods 

So you may have functionalities you want exposed to the outside world but then you want other modules to access this functionality. EG: An email module.  In this case you might want to set the permission on the methods within the module.

To do so just decorate the method with the right `ACL`  

EG: 

`/**`

`     * @ACL public`

`     */`

`    public function sendEmail($subject, $body, $recpt)`

`    {`

`        //sending email to $reciever `

`    } `

TODO:  Sample Video creating a simple  module

TODO:  Sample service with a view

**Modules**

DevLess provides  alot out of the box in addition to this DevLess allows you to add or extend existing functionalities using the concept of plugins known as modules .

**When to create a module**

As mentioned DevLess provides most of what you  need right out the box. But sometimes you may require some functionality that is either not available or a little different from what you need. In such situations  you may consider creating a module

**How to install and use Modules**

Before creating a Module  you should check the  `service hub`  ..  within the framework to see if a module meets  your need.

To install a Module is easy, head over to the  `service hub` ..section within your DevLess instance and hit `install:update` of the module you will like to install . Now when you visit the `all services`  you should see your module installed.

**How  to create one **

If you are familiar with the concept of services in DevLess, congrats you are half way to creating a Module.

Whenever you create a service additional files are generated. These files contain code that make extending the internals of DevLess easy.

These files are tacked away in `resources/views/service_views/<service_name>`_ within the DevLess framework. At this point to access the files you will need to setup a _[_development environment.      _](/dev-setup.md)

Action Classes and me

Making RPC call

Securing endpoints

UIs and asset management

Callable Helpers

Submitting a module to the store

