# The Basics

The HTML Kit makes it easy to build web pages that interact with DevLess, without writing a line of JavaScript.

## Scaffolding

To get started with editing data for a table, you can use automatically generated "Scaffolds" for a table. Go to the list of tables in your application, and press the button looking like this: `</>`. This will download a html file to your browser's download folder.

You can open this file in a browser, and you will be able to interact with the DevLess data directly. Try editing it! This file is meant to be a starting point for writing web applications using DevLess.

## Larger examples

This document aims to describe how one can interact with DevLess with quite simple, but detailed, examples. For a larger example of how DevLess can be used, check out the [starter pack](https://github.com/DevlessTeam/web-starter-pack). There is also a video tutorial on the starter pack setup [here](https://www.youtube.com/watch?v=EqMp0EYSf0s).

## Interacting with devless

### Setting up

Go to your DevLess instance, press this buttton: ![](../../.gitbook/assets/connect_to_devless.png). It is located at the top right in your DevLess panel. It should present you with an HTML tag to connect to your instance. It should look something like this:

```javascript
<script src="http://bestapp.herokuapp.com/js/devless-sdk.js" class="devless-connection" devless-con-token="abcde123456789"></script>
```

Copy **the tag from your instance** into a HTML page, and you are connected!

### Adding data

In order to add data to a service, you can use a `form` tag with a class of `dv-add-oneto:SERVICE:TABLE`.

For example, say that I have a service named `contacts` with a table named `people`. I can then use a form with a class of `dv-add-oneto:contacts:people`. Any inputs within this form will be mapped to the columns in the `people` table based on the `name` property.

For example, using the following markup we can add people with name & email to the people table:

```markup
<div class="dv-notify-success" style="color: green">Person added</div>
<div class="dv-notify-failed" style="color: red">Failed to add person</div>
<form class="dv-add-oneto:contacts:people">
    <input type="text" name="name" placeholder="Name"><br>
    <input type="text" name="email" placeholder="Email"><br>
    <button type="submit">Submit</button>
</form>
```

The `dv-notify-success` and `dv-notify-failed` classes is a means of getting notifications about if the operation succeeded or not. The `dv-notify-success` element will be hidden until a person was successfully added, while `dv-notify-failed` will be hidden until something goes wrong. For example, connection failures or incorrect input.

### Listing data

In order to list data, you can use the `dv-get-all:SERVICE:TABLE` class. This makes the HTML SDK repeat the inner HTML for all entries in the table. You can use the `var-NAME` class to populate that element with that entry's value for the column referred to by the `NAME`.

The classes can be used on any kind of html tag. This makes it trivial to layout your webpage any way you want it.

The `var-NAME` syntax can also be used within the `href` attribute of the `<a>` tag. Note that the `<a>` tag also needs to have `class="var-NAME"` set. Within the `href` attribute, the `var-NAME` part is _interpolated_, i.e. you can surround it with other text.

Accessing data in `referred` tables can be done by using `var-REFERED_TABLE_NAME`. E.g. `var-category-name` for a product, which is linked to a category table with a field name.

For example, we can create a list of all our contacts like this:

```markup
<div class="dv-notify"></div>
<ul class="dv-get-all:contacts:people">
    <li>
        <span class="var-name"></span>:
        <a class="var-email" href="mailto:var-email">
            <span class="var-email"></span>
        </a>
    </li>
</ul>
```

**NB:** In the case of image tags remember to set an empty src tag `<img src="" class="var-image"/>`

Here we list all entries in the `people` table in the `contacts` service. For each contact we print their name and their email. For the email, we also add a `mailto` link.

**NB:** In the case of image tags remember to set an empty src tag `<img src="" class="var-image"/>`

The classes can be used on any kind of html tag. This makes it trivial to layout your webpage any way you want it.

### Selecting Data

Data can be selected based on any column. This is done by using the `dv-where` prefix to the `get-all` class. For example, if I wanted to select all contacts with the name of 'joe', we would do it like this:

```markup
<div class="dv-notify"></div>
<ul class="dv-where:name:joe-get-all:contacts:people">
    <li>
        <span class="var-name"></span>:
        <a class="var-email" href="mailto:var-email">
            <span class="var-email"></span>
        </a>
    </li>
</ul>
```

You can use any of the fields in the row to do this, just replace the `name` and `joe` part with the column name and the value.

If you only want to show one entry, use this technique but with a unique key in the where-clause.

### Deleting data

Using our previous data listing as the base, we can add deletion functionality by using the `dv-delete` class. The `dv-delete` class can be used within a listing, on `<a>` or `<button>` elements.

```markup
<div class="dv-notify"></div>
<ul class="dv-get-all:contacts:people">
    <li>
        <span class="var-name"></span>:
        <a class="var-email" href="mailto:var-email">
            <span class="var-email"></span>
        </a>
        <button class="dv-delete">Delete</button>
    </li>
</ul>
```

### Updating data

When it comes to updating existing data, we use a combination of the `dv-update` and `dv-update-oneof` classes. Within a listing, use the `dv-update` class on a `<button>` or `<a>` tag. Then, create a `<form>` with the fields you want to update with a class of `dv-update-oneof:SERVICE:TABLE`. As when adding data, the `name` of the `input` elements map to columns in the table.

For example, to updating emails in our contact book:

```markup
<div class="dv-notify"></div>
<ul class="dv-update-oneof:contacts:people">
    <li>
        <span class="var-name"></span>:
        <a class="var-email" href="mailto:var-email">
            <span class="var-email"></span>
        </a>
        <button class="dv-update">Update</button>
    </li>
</ul>
<form class="dv-update-oneof:contacts:people">
    <input type="text" name="email" placeholder="Update email">
    <button type="submit">Update</button>
</form>
```

### Signing up

To signup a user , you will need to add the `dv-signup` class. You will need to place this in the form tag. Also the list of registered users can be found under the users tab on your DevLess instance.  
**NB:** DevLess will move to the URL set for the `action` attribute on a successful signup.

The example code below illustrates signing up a user and moving to the dashboard page on success.

```markup
 <div class="dv-notify"></div>
  <form class="dv-signup" action="/dasboard">
    <input type="text" name="username" placeholder="Enter username here">
    <input type="email" name="email" placeholder="Enter email here">
    <input type="number" name="phonenumber" placeholder="Enter phone number here">
    <input type="text" name="firstname" placeholder="Enter first name here">
    <input type="text" name="lastname" placeholder="Enter last name here">
    <input type="password" name="password" placeholder="Enter password here">
    <button type="submit">Signup</button>
  </form>
```

### Signing in

To sign in users, you will need to add the `dv-signin` class to a form.  
You will also need to pick either `phonenumber` , `email` or `username` as an identifier and a password for signin.

The example below uses `username` and `password` for sign in

```markup
  <div class="dv-notify"></div>
  <form class="dv-signin" action="/dasboard">
    <input type="text" name="username" placeholder="Enter username here">
    <!--<input type="email" name="email" placeholder="Enter email here">-->
    <!--<input type="number" name="phonenumber" placeholder="Enter phone number here">-->
    <!-- Choose between username, phone_number and email-->
    <input type="password" name="password" placeholder="Enter password here">
    <button type="submit">Signin</button>
  </form>
```

### Get profile

Once you are either signed in or signed up. You can now display the profile of the user. You will need the `dv-profile` class and use `var-<profile_field>` to get the the field value.

```markup
   <div class="dv-profile">
    <span class="var-username"></span>
    <span class="var-email"></span>
    <span class="var-firstname"></span>
    <span class="var-last_name"></span>
    <span class="var-phone_number"></span>
  </div>
```

### Updating profile

You can update the profile of a signed up or signed in user using a form.  
You will need to add the `dv-updateProfile` class to the form as well as the fields to be updated.

```markup
    <div class="dv-notify"></div>
    <form class="dv-updateProfile">
    <input type="text" name="username">
    <input type="email" name="email">
    <input type="text" name="firstname">
    <input type="text" name="lastname">
    <input type="number" name="phone_number">
    <input type="password" name="password">
    <button type="submit">Update profile</button>
  </form>
```

### Logging out

Logging out a user is as simple as adding the `dv-logout` class and an `action` attribute to redirect to once the user is logged out.

```markup
   <button class="dv-logout" action="/">Logout</button>
```

### Setting attribute values

There are situations where you might want to set a value of an attribute. EG: Setting the value of a select box.

```markup
    <select class="dv-get-all:service_name:table_name">
        <option class="set-value:var-id "></option>
    </select>
```

The above code will generate a series of `option` tags each looking like `<option value="1">Beverages</option>` with changes depending on data from `table_name`.

### Notifications

To make you page more interactive you may use `dv-notify` as well as the `dv-processing` classes.

**General notifications**: For every of the above operations except for a successful query DevLess returns a more generic message . You may display this by adding the class `<div class="dv-notify"></div>` This will print out the message as sent from the DevLess backend. These kind of messages are good for debugging purposes.

**Specialized notifications:** Most likely you will like to show your users friendly message. In this case you may use `<div class="dv-notify-success">your success message here :)</div>` for successful operations and `<div class="dv-notify-failed">you failure message goes here :(</div>` for failed operations.

**Progress notifications:** Some operations might take a couple of seconds and might lead to users having to wait. Its best practice to signal the user when the task starts and when its done. You can use `<div class="dv-processing">Sending...</div>` to notify users when the processing begins and `<div class="dv-doneProcessing">Done</div>` when the operation is done

