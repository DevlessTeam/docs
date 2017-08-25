# Extending Services

Services is a core concept in DevLess. You can create your own basic services easily. But many of the services available at the [service hub](/using_services.md) have much more functionality. Let's take a look at how you can extend services with additional functionality.

## Video guide

As many other pages in this documentation, there is a video guide to go along with it. It's [right here!](https://youtu.be/tS6-tS3yjLc)

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

The `ActionClass.php` file contains a `PHP` class, named after your service, with a list of sample methods. You can add methods to this class, which will then be available for calling via DevLess' [HTTP API](/http_api.md), [SDKs](/sdks.md) or from other services within DevLess.

E.g. to create a simple "greeter" method, we can add: 

```php
/**
* @ACL public
*/
public function greet($name)
{
  return "Hello, $name";
}
```

Assuming you labelled your service "greeter" then you can access the `greet` method of the greeter service e.g. using curl:

```bash
% curl -L -XPOST \
  -H "Devless-token: $DEVLESS_TOKEN" \
  -H "Content-type: application/json" \
  "http://$DEVLESS_URL/api/v1/service/greeter/rpc?action=greet" \
  -d@- <<EOF
{
  "jsonrpc": "2.0",
  "method": "greeter",
  "id": "1000",
  "params": ["Edmond"]
}
EOF
```

Gets you the following response:

```js
{
  "status_code": 637,
  "message": "Got RPC response successfully",
  "payload": {
    "jsonrpc": 2,
    "result": "Hello, Edmond",
    "id": 1000
  }
}
```
You can write any kind of functionality you want this way, going from very simple to very complex.

### Setting up Access Control Levels for your methods

So you may have functionalities you want exposed to the outside world, but others that you only want other services to be able to access. e.g. in the case of an email service, you might want to set the permission on the methods within the service so that RPC calls can't send emails.

To do so just decorate the method with the right `ACL` (Access Control Level), e.g:

```php
/**
 * @ACL private
 */

public function sendEmail($subject, $body, $reciever)
{
  //sending email to $reciever
}
```

There are three **ACLs **:

| ACL | Description | 
| --- | --- |
| `public` | Public to everyone. |
| `protected` | Accessible to logged in users of DevLess.  |
| `private` |  Blocks all calls made from the outside world. This means the method can only be accessed from within DevLess services. | 


### Using services from other services

Methods exposed by one service can be used by other services in the same DevLess instance. This can be done both from the rules engine and from other service's `ActionClass.php` code base. 

To use services from within the rules engine, use the `run` method:
```php
->beforeQuerying()->run('mailer', 'sendEmail', ['hello', 'message goes here', 'joe@email.com'])->getResult($state)->succeedWith($state)
```

To use services from within php code, use the ``` execute($service, $method, $params = null)``` method. E.g.:

```php
ActionClass::execute('mailer', 'sendEmail', ['hello', 'message goes here', 'joe@email.com'])
```

## Changing the Documentation/Management UI

You may have noticed the `docs:UI` button on each service listed on the "All Service" part of the DevLess UI. Clicking on this lists out all the methods you currently have in the ActionClass of the service. If you add another method this will automagically be added to the docs.

The `index.blade/php` is the file responsible for the rendering of the docs. You may customize this to serve as an Admin panel for a particular service.

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

### Submitting you service to the store

Once you have extended a service, you may decide to share this with the world. One way you can share your new service is via the DevLess Service Hub.

* To do this you have to head over to **migration tab** on your instance then export the service.
* Next you will have to host your service online. For example, using a code hosting platform like GitHub.
* Clone the Service Hub JSON file. `https://github.com/DevlessTeam/service-hub.git` and update the JSON file with info about your service. 
* You can now send a pull request  of the updated JSON 

If everything is good it will be merged and your service will show up on the Service Hub. 



