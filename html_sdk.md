# HTML Kit

The HTML Kit makes it easy to build web pages that interact with DevLess, without writing a line of JavaScript.

## Scaffolding
TODO: Describe how to get a scaffold

## Setting up

Go to your DevLess instance, press this buttton: ![](/assets/connect_to_devless.png). It is located at the top right in your DevLess panel. It should present you with a HTML tag to connect to your instance. It should look something like this:

```
<script src="http://bestapp.herokuapp.com/js/devless-sdk.js" class="devless-connection" devless-con-token="abcde123456789"></script>
```

Copy **the tag from your instance** into a HTML page, and you are connected!

## Adding data 

In order to add data to a service, you can use a `form` tag with a class of `dv-add-oneto:SERVICE:TABLE`. 

For example, say that I have a service named `contacts` with a table named `people`. I can then use a form with a class of `dv-add-oneto:contacts:people`. Any inputs within this form will be mapped to the columns in the `people` table based on the `name` property.

For example, using the following markup we can add people with name & email to the people table:

```html
<div class="dv-notify-success" style="color: green">Person added</div>
<div class="dv-notify-failed" style="color: red">Failed to add person</div>
<form class="dv-add-oneto:contacts:people">
    <input type="text" name="name" placeholder="Name"><br>
    <input type="text" name="email" placeholder="Email"><br>
    <button type="submit">Submit</button>
</form>
```
The `dv-notify-success` and `dv-notify-failed` classes is a means of getting notifications about if the operation succeeded or not. The `dv-notify-success` element will be hidden until an the person was successfully added, while `dv-notify-failed` will be hidden until something goes wrong. For example, connection failures or incorrect input.

## Listing data 

In order to list data, you can use the `dv-get-all:SERVICE:TABLE` class. This makes the HTML SDK repeat the inner HTML for all entries in the table. You can use the `var-NAME` class to populate that element with that entry's value for the column referred to by the `NAME`.

For example, we can create a list of all our contacts like this:

```html
<ul class="dv-get-all:contacts:people">
    <li>
        <span class="var-name"></span>:
        <span class="var-email"></span>
    </li>
</ul> 

```
The classes can be used on any kind of html tag. This makes it trivial to layout your webpage any way you want it.

## Selecting Data

Data can be selected based on any column. This is done by using the `dv-where` prefix to the `get-all` class. For example, if I wanted to select all contacts with the name of 'joe', we would do it like this:
```html
<ul class="dv-where:name:joe-get-all:contacts:people">
    <li>
        <span class="var-name"></span>:
        <span class="var-email"></span>
    </li>
</ul> 

```
You can use any of the fields in the row to do this, just replace the `name` and `joe` part with the column name and the value.

## Deleting data

Using our previous data listing as the base, 