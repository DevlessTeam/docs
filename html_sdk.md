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
<div class="dv-notify-failed" style="color: red">Failed to person</div>
<form class="dv-add-oneto:profiles:details">
    <input type="text" name="name" placeholder="Name"><br>
    <input type="text" name="email" placeholder="Email"><br>
    <button type="submit">Submit</button>
</form>
```
The `dv-notify-success` and `dv-notify-failed` classes is a means of getting notifications about if the operation succeeded or not. The `dv-notify-success` element will be hidden until an the person was successfully added, while `dv-notify-failed` will be hidden until something goes wrong. Examples are connection failures or incorrect input. 

## 
