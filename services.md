# Extending Services

Services is a core concept in DevLess. You can create your own basic services easily. But many of the services available at the [service hub](/using_services.md) have much more functionality. Let's take a look at how you can extend services with additional functionality.

## Workflow

The workflow for extending services goes something like this:

1. Set up a local instance of DevLess. For this step, see the [Development Setup Guide](/dev-setup.md).
1. Create a new service in the DevLess UI. Just go to the "Services" tab and press "Create New"
1. Extend the service by modifying files in `DV-PHP-CORE/resources/views/service_views/<service_name>`. This is covered on this page.
1. Share your service by migrating it from the migration tab.

## Anatomy of a Service

Services are represented as 2 major parts.

- Service meta-data. This includes the name and description. When creating a service, this is stored in DevLess' database. When migrating, this is exported as JSON meta-data in the service file.
- Service code & assets. For new services, this is generated at `DV-PHP-CORE/resources/views/service_views/<service_name>`. When migrating, all these files is packed in the service file.

The service code contains:

- An `ActionClass.php` file. This contains a class with the same name as your service. This is where you can add new functionality.
- An `index.blade.php` file. This is the documentation UI for your service. By default, it uses the `help` method from the `ActionClass.php` file to list all methods.
- An `assets` folder. These are public assets such as css and js files, which can be accessed from e.g. the `index.blade.php` file.

## Extending functionality

Start by [setting up DevLess locally](/dev-setup.md) and creating a service through the services UI. 

When the service was created, it generated the base for a service at `DV-PHP-CORE/resources/views/service_views/<service_name>`.

The `ActionClass.php` file contains a `PHP` class with a list of sample methods . You can add methods to this file, which will then be available for calling via DevLess' [HTTP API](/http_api.md), [SDKs](/sdks.md) or from other services within DevLess.

E.g. to create a simple "greeter" method, we can use: 
```

```php
/**
* @ACL public
*/

public function sendEmail($subject, $body, $recpt)

{

//sending email to $reciever

}
```

Assuming you labelled your new module contacts the you should be able to access the `samplePublicMethod` in the **ActionClass** of the contacts module EG: using the  [JavaScript SDK](/sdks.md) 
```js
SDK.call('contacts', 'samplePublicMethod', [], function(resp){alert(resp);})
```
This should return `"Public Hello"`.

Now you may go ahead and add your own methods with functionalities you require.

Here is an in dept video explaining the  process

**TODO: **create a video for adding functionality

#### Setting up Privacy for your methods

So you may have functionalities you want exposed to the outside world but then you want other modules to access this functionality. EG: An email module.  In this case you might want to set the permission on the methods within the module.

To do so just decorate the method with the right `ACL`

EG:

```
/\*\*

\* @ACL public

\*/

public function sendEmail\($subject, $body, $recpt)

{

//sending email to $reciever

}
```

There are three **ACLs ** there is `public` which makes the method publicly available to anyone. `protected` which requires uses to authenticate to gain access to the method. `private` blocks all calls made from the outside world. This means the method can only be accessed within another module  no where else.  To use  a module within another , you may use the `run` method within the   Rules section in the  service you will like to use this in . EG: `->beforeQuerying()->run('mailer', 'sendEmail', ['hello', 'message goes here', 'joe@email.com'])->getResult($state)->succeedWith($state)` .

**TODO:**How the ACLs work and affect the methods and how to use services within other services

## Changing the Documentation/Management UI

You may have noticed the `docs:UI` button on each service listed on the **All Service** section. Clicking on this lists out all the methods you currently have in the **ActionClass** of that service on adding another this will be automagically added with the docs as well.

The `index.blade/php` is the file responsible for the rendering of the docs. You may customize this to serve as an Admin panel for that particular service/Module.

You may also add extra files. Be sure to follow the template naming convention `<filename>.blade.php`. There blade template engine is [documented in the Laravel docs](https://laravel.com/docs/5.1/blade), but using regular html/css/js generally just works.

There are a few inbuilt helpers that may make creating the interface easier:

* `DvAssetPath($payload, $partialAssetPath)` allows you to call on asset files from the asset folder within the service directory. e.g: 
  ```html
  <script src="<?=DvAssetPath($payload, '/js/main.js')?> >"  ></script>``` 
  This will pull in main.js from the js folder within the assets directory within that service. `$payload` is a global variable and preset so you don't have to worry about it. 
* `DvNavigate($payload, 'pageName')`: Once you add extra pages to the service navigating between them should be done using `DvNavigate` e.g: 
  ```html
    <a href="<?= DvNavigate($payload, '<pagename>'); ?>" />`:  
    ```
    Page names don't have to include the `blade.php` suffix.
* `DvJSSDK()`: This method will insert the JS SDK into the page . e.g: 
  ```html
  <?= DvJSSDK()?>```
  You can now use all functionality in the JS SDK within your page.

#### TODO:video to explaining how to create view pages

### Submitting you module to the store

Once you have a module, you may decide to share this with the world. One way you can share your new module  via the DevLess Service Hub.

* To do this you have to head over to **migration tab** on your instance then export the module.
* Next you will have to host your module online. Preferably using a code hosting platform like GitHub.
* Clone the Service Hub JSON file. `https://github.com/DevlessTeam/service-hub.git` and update the JSON file with info about your service. 
* You can now send a pull request  of the updated JSON 

If everything is good it will be merged and your module will show up on the Service Hub. 



