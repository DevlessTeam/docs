
<a name="address-book"></a>
## A Simple Address Book
In this tutorial, we will be using the [CDN](https://library.devless.io/cdn/1.0/dv_sdk.js) version of the [DevLess Official Javascript SDK](https://github.com/DevlessTeam/DV-JS-SDK) to persist data from a simple address book. We will build the frontend with HTML and JavaScript, jQuery and make it look good with [Bootstrap](https://getbootstrap.com/).

You can get the full source code to this tutorial [here](https://gist.github.com/johnotu/5b7f1e96d7b0138ddb292273949169c8).

**Step 1**
We build the frontend with our preferred text editor and save it as addressbook.html:
```
<!DOCTYPE html>
<html>
	<head>
		<title>Address Book</title>
		<link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
		<style type="text/css">
			button{margin: 0 20px;}
		</style>
	</head>
	<body>
		<div class="container">
			<div class="row">
				<div class="col-md-8 col-md-offset-2">
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
						<button type="button" class="btn btn-primary" id="submit">Add contact</button>
					</form>
					<p id="response"></p>
					<br>
					<div class="table-responsive">
						<table class="table table-condensed table-bordered" id="address-table">
							<thead class="active">
								<tr>
									<th>First Name</th>
									<th>Last Name</th>
									<th>Email</th>
									<th>Phone Number</th>
									<th></th>
									<th></th>
								</tr>
							</thead>
							<tbody></tbody>
						</table>
					</div>
				</div>
			</div>
		</div>
		<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>

<!-- DevLess SDK -->
		<script src="https://library.devless.io/cdn/1.0/dv_sdk.js"></script>		
	</body>
</html>
```
[![front_end.png](https://s29.postimg.org/5ve62aniv/front_end.png)](https://postimg.org/image/ia0y2mf0z/)

**Step 2**
-	Create a DevLess instance if you don't have one. [Heroku click to deploy](https://heroku.com/deploy?template=https://github.com/DevlessTeam/DV-PHP-CORE/tree/heroku2). You will need to create a Heroku account if you don't have one.
-	Create a service for our web app. Go to the Services tab on the Menu and click "Create Service"
[![create_service_1.png](https://s30.postimg.org/qplezfwi9/create_service_1.png)](https://postimg.org/image/7ki5pohu5/)

- Fill out the service name `addressbook` and any description you like and click "Create" skipping the rest of the sections since we are not connecting our service to a remote database.
[![create_service_2.png](https://s30.postimg.org/jaw372amp/create_service_2.png)](https://postimg.org/image/t87404i8d/)

- From the Tables tab, create a table named `addresses` for the data we'll be collecting from our address book. Fill out four fields `firstName`, `lastName`, `email`, and `phoneNumber` with Field Types `TEXT`, `TEXT`, `EMAIL` and `TEXT` respectively. Skip the Reference Table and Default Value sections but check `REQUIRED` Field Options on both firstName and lastName fields to make it impossible to add a contact without names. Also check the `UNIQUE FIELD` on both email and phoneNumber fields to ensure no two persons share the same email or phone numbers. Click "Create Table" and we have our table of addresses. 
[![add_tables_dialogue.png](https://s30.postimg.org/ez7hi23pt/add_tables_dialogue.png)](https://postimg.org/image/7j87w9g0d/)

**Step 3**
- From the App tab on the Menu, click "Connect to my App" to bring up the Choose connection type dialogue. Copy out the content `var constants...` and paste in another script tag in addressbook.html
[![Screenshot from 2016-12-21 15-30-20.png](https://s20.postimg.org/xnwivzhq5/Screenshot_from_2016_12_21_15_30_20.png)](https://postimg.org/image/4li8t5vg9/)

[![Screenshot from 2016-12-21 15-30-36.png](https://s20.postimg.org/ab32r7r0d/Screenshot_from_2016_12_21_15_30_36.png)](https://postimg.org/image/9ybol18qh/)
```
<script>
	$(function(){
		var constants = { "token":"7f0449c4d77ade309aa26ef0a9de6bbe", "domain":"http://localhost:3000" }; Devless = new Devless(constants);
	})
</script>
```

**Step 4**
On the Privacy tab, click on "API" to display the Endpoints Access Rules dialogue. This is where we can set access rights for the various CRUD tasks that can be carried out on our database.
Select the service name and choose `PUBLIC` for Query, Create, Update and Delete Access.
[![Screenshot from 2016-12-22 00-43-01.png](https://s20.postimg.org/g5n2l74hp/Screenshot_from_2016_12_22_00_43_01.png)](https://postimg.org/image/wgn6higzd/)

**Step 5** 
Within the last script tag, add the jQuery code that will:

* _Create_ data by adding values of all the fields on submission.
```
$('button#submit').click(function(){
	var firstName = $('#firstName').val(),
			lastName = $('#lastName').val(),
			email = $('#email').val(),
			phoneNumber = $('#phoneNumber').val();
	var data = {
		"firstName": firstName,
		"lastName": lastName,
		"email": email,
		"phoneNumber": phoneNumber
	};
	Devless.addData("addressbook", "addresses", data, function(response){
		console.log(response);
		$('p#response').text(response.message);
		$('input').val('');
		if(response.status_code === 609){
			$('tbody').empty();
			getAddresses();
		}
	});
});
```

* _Read_ data by querying the database to show all addresses when the addressbook is opened
```
function getAddresses(){
	var params = {};
	Devless.queryData('addressbook', 'addresses', params, function(response){
		console.log(response);
		var tableContent = '',
					updateButton = "<button type='button' class='btn btn-info update'>Update</button>", data = response.payload.results, dataLength = data.length;
		for(var i=0; i<dataLength; i++){
			tableContent += '<tr>';
			tableContent += '<td>' + data[i].firstname + '</td>';
			tableContent += '<td>' + data[i].lastname + '</td>';
			tableContent += '<td>' + data[i].email + '</td>';
			tableContent += '<td ' + "id='" + data[i].id + "'>" + data[i].phonenumber + '</td>';
			tableContent += '<td>' + "<button type='button' class='btn btn-info' " + "onclick='" + "updateAddress(" + data[i].id + ")'" + ">Update</button>" + '</td>';
			tableContent += '<td>' + "<button type='button' class='btn btn-danger' " + "onclick='" + "deleteAddress(" + data[i].id + ")'" + ">Delete</button>" + '</td>';
			tableContent += '</tr>';
			$('tbody').append(tableContent);
			tableContent = '';
		};
	});
}
```
_Remember to call this function `getAddresses()` at the beginning of the script tag_

* _Update_ addresses

```
function updateAddress(id){
	Devless.queryData('addressbook', 'addresses', {where:["id," + id]}, function(response){
		console.log(response);
		var data = response.payload.results[0];
		$('input#firstName').val(data.firstname);
		$('input#lastName').val(data.lastname);
		$('input#email').val(data.email);
		$('input#phoneNumber').val(data.phonenumber);
		$('form').append("<button type='button' class='btn btn-info' id='update-address' onclick='update(" + data.id + ")'>Update</button>")
	})
}

function update(id){
	var firstName = $('#firstName').val(),
			lastName = $('#lastName').val(),
			email = $('#email').val(),
			phoneNumber = $('#phoneNumber').val();
	var data = {
		"firstName": firstName,
		"lastName": lastName,
		"email": email,
		"phoneNumber": phoneNumber
	};
	Devless.updateData("addressbook", "addresses", "id", id, data, function(response){
		console.log(response);
		$('input').val('');
		$('tbody').empty();
		getAddresses();
		$('#update-address').remove();
	});
}
```

* _Delete_ addresses

```
function deleteAddress(id){
	Devless.deleteData('addressbook', 'addresses', 'id', id, function(response){
		console.log(response);
		$('p#response').text(response.message);
	});
	$('tbody').empty();
	getAddresses();
}
```

We should now have a working address book with the addresses persisted with a DevLess backend.

[![Screenshot from 2017-01-02 11-10-52.png](https://s20.postimg.org/j34xb35od/Screenshot_from_2017_01_02_11_10_52.png)](https://postimg.org/image/qj46wvtdl/)

See the full code [here](https://gist.github.com/johnotu/5b7f1e96d7b0138ddb292273949169c8)
