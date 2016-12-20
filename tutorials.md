## Tutorials

- [A Simple Address Book](#address-book)

<a name="address-book"></a>
## A Simple Address Book
In this tutorial, we will be using the [DevLess Official Javascript SDK](https://github.com/DevlessTeam/DV-JS-SDK) to persist data from a simple address book. We will build the frontend with HTML and JavaScript and make it look good with [Bootstrap](https://getbootstrap.com/).

You can get the full source code to this tutorial [here](#).

**Frontend**
Let's build the frontend. With any preferred text editor type or copy the code below, paste and save it as addressbook.html:
```
<!DOCTYPE html>
<html>
	<head>
		<title>Address Book</title>
		<link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
	</head>
	<body>
		<div class="container">
			<div class="row">
				<div class="col-lg-4 col-md-4 col-xs-8">
					<h2>A simple Address Book</h2>
					<br>
					<form>
						<div class="form-group">
							<label for="firstName">First Name</label>
							<input type="text" class="form-control" id="firstName">
						</div>
						<div class="form-group">
							<label for="lastName">Last Name</label>
							<input type="text" class="form-control" id="lastName">
						</div>
						<div class="form-group">
							<label for="email">Email</label>
							<input type="email" class="form-control" id="email">
						</div>
						<div class="form-group">
							<label for="phoneNumber">Phone Number</label>
							<input type="tel" class="form-control" id="phoneNumber">
						</div>
						<button type="submit" class="btn btn-primary" id="submit">Add contact</button>
					</form>
				</div>
			</div>
		</div>

		<script type="text/javascript" src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
		<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
	</body>
</html>
```
You should now have a fairly pretty frontend like this:
[![front_end.png](https://s29.postimg.org/5ve62aniv/front_end.png)](https://postimg.org/image/ia0y2mf0z/)

Now to the goodies :yum:

**Backend with DevLess**
-	First, we create a DevLess instance with just a single click of the [Heroku click to deploy](https://heroku.com/deploy?template=https://github.com/DevlessTeam/DV-PHP-CORE/tree/heroku) option. You will need to create a Heroku account if you don't have one.
-	After our DevLess instance is live, we can then create a service for our web app. To create a service, 
> go to the Services tab on the Menu at the left hand pane and 
> click "Create Service"
[![create_service_1.png](https://s30.postimg.org/qplezfwi9/create_service_1.png)](https://postimg.org/image/7ki5pohu5/)

> Fill out the service name `addressbook` and any description you like. We will skip the restof the sections since we are not connecting our service to a remote database.
> Click "Create" and we have an addressbook service running on our DevLess instance.
[![create_service_2.png](https://s30.postimg.org/jaw372amp/create_service_2.png)](https://postimg.org/image/t87404i8d/)

- Next up, we'll create a table for the data we'll be collecting from our address book. Our table will have four fields to match the data (First Name, Last Name, Email, Phone Number) on our front end. The types for each of the fields will be the data type we'll be collecting, ranging from TEXT, TEXTAREA, INTEGER etc to EMAIL.
> go to the Tables tab and click "New table"
> fill out details in the Add Tables dialogue for the Table Name `addresses`, Description (whatever you want).
> fill out the field details making sure to click the `Add a Field` button to be able to add the remaining fields. For this tutorial, our four Fields will include `firstName`, `lastName`, `email`, and `phoneNumber` while their corresponding Field Types will be `TEXT`, `TEXT`, `EMAIL` and `TEXT` respectively. Skip the Reference Table and Default Value sections but check `REQUIRED` Field Options on both firstName and lastName fields to make it impossible to add a contact without names. Also check the `UNIQUE FIELD` on both email and phoneNumber fields to ensure no two persons share the same email or phone numbers. Click "Create Table" and we have our table of addresses. 
[![add_tables_dialogue.png](https://s30.postimg.org/ez7hi23pt/add_tables_dialogue.png)](https://postimg.org/image/7j87w9g0d/)

- Now we visit the App Panel to obtain remote access to our adrressbook db.
> Click on App tab on the Menu to display the App Panel. Here is where you can revoke access to any app (frontend) that currently has access to your DevLess backend.
> Clicking "Connect to my App" will bring up the Choose connection type dialogue where we are presented with the script snippet that will give our app access to our DevLess backend.
> Copy out the content `var constants...`, open another script tag in addressbook.html under the one for Bootstrap CDN and paste the snippet there.

**Addressbook Logic**
We want data captured in the fields to be persisted in our DevLess backend so we can retrieve and update it at any time. To do that, we'll add some JavaScript code (we'll be using the DevLess Javascript SDK remember) to the script tag we just added at the bottom of addressbook.html
```
``` 