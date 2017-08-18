**Modules**

DevLess provides  alot out of the box in addition to this DevLess allows you to add or extend existing functionalities using the concept of plugins known as modules .

**When to create a module**

As mentioned DevLess provides most of what you  need right out the box. But sometimes you may require some functionality that is either not available or a little different from what you need. In such situations  you may consider creating a module

**How to install and use Modules**

Before creating a Module  you should check the  `service hub`  ..  within the framework to see if a module meets  your need.

To install a Module is easy, head over to the  `service hub` ..section within your DevLess instance and hit `install:update` of the module you will like to install . Now when you visit the `all services`  you should see your module installed.

**How  to create one**

If you are familiar with the concept of services in DevLess, congrats you are half way to creating a Module.

Whenever you create a service additional files are generated. These files contain code that make extending the internals of DevLess easy.

These files are tacked away in `resources/views/service_views/<service_name>`_ within the DevLess framework. At this point to access the files you will need to setup a _[_development environment.      _](/dev-setup.md)

Action Classes and me

Making RPC call 

UIs and asset management 

Callable Helpers 

Submitting a module to the store 



