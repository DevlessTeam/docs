# HTML Kit

The HTML Kit makes it easy to build web pages that interact with DevLess, without writing a line of JavaScript.

## Scaffolding

TODO: Describe how to get a scaffold

## Larger examples

This document aims to describe how one can interact with DevLess with quite simple, but detailed, examples. For a larger example of how DevLess can be used, check out the [starter pack](https://github.com/DevlessTeam/web-starter-pack). There is also a video tutorial on the starter pack setup [here](https://www.youtube.com/watch?v=EqMp0EYSf0s).

## Interacting with devless

### Setting up

Go to your DevLess instance, press this buttton: ![](/assets/connect_to_devless.png). It is located at the top right in your DevLess panel. It should present you with an HTML tag to connect to your instance. It should look something like this:

```
<script src="http://bestapp.herokuapp.com/js/devless-sdk.js" class="devless-connection" devless-con-token="abcde123456789"></script>
```

Copy **the tag from your instance** into a HTML page, and you are connected!

### Adding data

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

The `dv-notify-success` and `dv-notify-failed` classes is a means of getting notifications about if the operation succeeded or not. The `dv-notify-success` element will be hidden until a person was successfully added, while `dv-notify-failed` will be hidden until something goes wrong. For example, connection failures or incorrect input.

### Listing data

In order to list data, you can use the `dv-get-all:SERVICE:TABLE` class. This makes the HTML SDK repeat the inner HTML for all entries in the table. You can use the `var-NAME` class to populate that element with that entry's value for the column referred to by the `NAME`.

The classes can be used on any kind of html tag. This makes it trivial to layout your webpage any way you want it.

The `var-NAME` syntax can also be used within the `href` attribute of the `<a>` tag. Note that the `<a>` tag also needs to have `class="var-NAME"` set. Within the `href` attribute, the `var-NAME` part is _interpolated_, i.e. you can surround it with other text.

Accessing data in `referred` tables can be done by using `var-REFERED_TABLE_NAME`. E.g. `var-category-name` for a product, which is linked to a category table with a field name.

For example, we can create a list of all our contacts like this:

```html
<ul class="dv-get-all:contacts:people">
    <li>
        <span class="var-name"></span>:
        <a class="var-email" href="mailto:var-email">
            <span class="var-email"></span>
        </a>
    </li>
</ul>
```

Here we list all entries in the `people` table in the `contacts` service. For each contact we print their name and their email. For the email, we also add a `mailto` link.

The classes can be used on any kind of html tag. This makes it trivial to layout your webpage any way you want it.

### Selecting Data

Data can be selected based on any column. This is done by using the `dv-where` prefix to the `get-all` class. For example, if I wanted to select all contacts with the name of 'joe', we would do it like this:

```html
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

```html
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

When it comes to updating existing data, we  use a combination of the `dv-update` and `dv-update-oneof` classes. Within a listing, use the `dv-update` class on a `<button>` or `<a>` tag. Then, create a `<form>` with the fields you want to update with a class of `dv-update-oneof:SERVICE:TABLE`. As when adding data, the `name` of the `input` elements map to columns in the table.

For example, to updating emails in our contact book:

```html
<ul class="dv-get-all:contacts:people">
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



