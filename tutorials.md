## Tutorials

- [A Simple Address Book](#address-book)
- [A Simple Blog Engine](#blog-engine)


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

<a name="blog-engine"></a>
## A Simple Blog Engine

In this other tutorial, we will be using the [Official DevLess Python SDK](https://github.com/DevlessTeam/DV-PY-SDK) to persist data from a simple blog engine we will build with [Flask](http://flask.pocoo.org/).

You can get the full source code to this tutorial [here](https://github.com/johnotu/BlogEngineWithDevLess).

**Required**

* Python [*requests*](https://pypi.python.org/pypi/requests) module
* Flask. See [installation guide.](http://flask.pocoo.org/docs/0.12/installation/)
* DevLess Instance [Heroku click to deploy](https://heroku.com/deploy?template=https://github.com/DevlessTeam/DV-PHP-CORE/tree/heroku). You will need to create a Heroku account if you don't have one.


**Step 1: Create project directory and files**

Our folder structure for this project will look like:

```
/blog
	/static
		style.css
	/templates
		admin.html
		base.html
		index.html
		show-post.html
		show-update.html
	blog.py
	devless.py
```

> **style.css** is where we'll keep our custom css styling.
> **admin.html** is where such admin tasks like creating, editing and deleting a blog post will be done.
> **base.html** is our base template which every other page will extend. We'll be using [Jinja](http://jinja.pocoo.org/), a templating engine for Python which comes with Falsk.
> **index.html** is the landing page where all blog posts will be listed.
> **show-post.html** will display a particular blog post.
> **show-update.html** will display the blog post to be updated.
> **blog.py** will contain all the business logic, routes and views for our blog engine.
> **devless.py** is the offical DevLess Python SDK downloaded from [here](https://github.com/DevlessTeam/DV-PY-SDK).


**Step 2: Install Flask**

While in the root directory of our project */blog*, follow these [instructions](http://flask.pocoo.org/docs/0.12/installation/) to install Flask or if you already have a virtual environment, just
```python
pip install Flask
``` 

**Step 3: Create a DevLess instance**
- Create a DevLess instance if you don't have one. [Heroku click to deploy](https://heroku.com/deploy?template=https://github.com/DevlessTeam/DV-PHP-CORE/tree/heroku). You will need to create a Heroku account if you don't have one.

-	Create a service for our web app. Go to the Services tab on the Menu and click "Create Service"
[![create_service_1.png](https://s30.postimg.org/qplezfwi9/create_service_1.png)](https://postimg.org/image/7ki5pohu5/)

- Fill out the service name `blog` and any description you like and click "Create" skipping the rest of the sections since we are not connecting our service to a remote database.
[![create_service_2.png](https://s30.postimg.org/jaw372amp/create_service_2.png)](https://postimg.org/image/t87404i8d/)

- From the Tables tab, create a table named `post` for the data we'll be collecting from our blog engine. Fill out five fields `title`, `subtitle`, `pubdate`, `author` and `text` with Field Types `TEXT` for all of them. Skip the Reference Table and Default Value sections but check `REQUIRED` Field Options for all of them. Click "Create Table" and we have our table of blog posts. 
[![add_tables_dialogue.png](https://s30.postimg.org/ez7hi23pt/add_tables_dialogue.png)](https://postimg.org/image/7j87w9g0d/)

**Step 4: Connect to DevLess**

- In **blog.py** add

```
python
import devless, datetime

devless = devless.Sdk("http://localhost:3000", "7f0449c4d77ade309aa26ef0a9de6bbe")
```

- From the App tab on the Menu, click "Connect to my App" to bring up the Choose connection type dialogue. Copy out the content `var constants...` and replace the address ("http...") and token ("7f0449c...") with your own.

[![Screenshot from 2016-12-21 15-30-20.png](https://s20.postimg.org/xnwivzhq5/Screenshot_from_2016_12_21_15_30_20.png)](https://postimg.org/image/4li8t5vg9/)

[![Screenshot from 2016-12-21 15-30-36.png](https://s20.postimg.org/ab32r7r0d/Screenshot_from_2016_12_21_15_30_36.png)](https://postimg.org/image/9ybol18qh/)



**Step 5: Initialize Flask**

```
python
# blog.py
from flask import Flask, render_template, request, redirect, url_for, flash, session

app = Flask(__name__)
app.secret_key = 'whatever'

if __name__ == '__main__':
	app.run()
```

**Step 6: Create the base view**

```
html
# base.html

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<title>My Blog - {% block head %}{% endblock %}</title>
	<link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
	<link href="{{ url_for('static', filename='style.css') }}" rel="stylesheet">
</head>
<body>
	<header class="intro-header">
		<div class="container">
			<div class="row">
					<div class="col-lg-8 col-lg-offset-2 col-md-10 col-md-offset-1">
						<div class="site-heading">
							{% block title %}{% endblock %}
							<span class="subheading">{% block sub_title %}{% endblock %}</span>
						</div>
					</div>
				</div>
			</div>
	</header>
	<div class="container">
		<div class="row">
			<div class="col-lg-8 col-lg-offset-2 col-md-10 col-md-offset-1">
				{% block content %}{% endblock %}
			</div>
		</div>
	</div>

	<hr>

	<footer>
		<div class="container">
				<div class="row">
					<div class="col-lg-8 col-lg-offset-2 col-md-10 col-md-offset-1">
						<p class="copyright text-muted">A Simple Blog Engine</p>
					</div>
				</div>
		</div>
	</footer>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
	<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```


**Step 7: Write methods and views to Create, Update and Delete a blog post**

```
python
# blog.py

@app.route('/admin')
def admin_panel():
	all_posts = get_posts()
	posts = all_posts['payload']['results']
	posts = sorted(posts, key=lambda k: k['id'], reverse=True)
	return render_template('admin.html', posts=posts)

@app.route('/add', methods=['POST'])
def add_post():
	title = request.form['title']
	sub_title = request.form['subtitle']
	author = request.form['author']
	text = request.form['text']
	add_blog_post(title, sub_title, author, text)
	flash('New blog entry was succesfully added')
	return redirect(url_for('blog'))

def add_blog_post(title, subtitle, author, text):
	date = str(datetime.datetime.now())[:16]
	data = {'title': title, 'subtitle': subtitle, 'author': author, 'pubdate': date, 'text': text}
	results = devless.add_data('blog', 'post', data)
	print results

def update_post(id, title, subtitle, author, text):
	data = {'title': title, 'subtitle': subtitle, 'author': author, 'text': text}
	results = devless.where('id', id).update_data('blog', 'post', data)
	print results

def delete_post(id):
	results = devless.where('id', id).delete_data('blog', 'post')
	print results


@app.route('/update/<int:id>')
def show_update_post(id):
	post = get_post_by_id(id)
	post = post['payload']['results'][0]
	return render_template('show-update.html', post=post)

@app.route('/update/<int:uid>', methods=['POST'])
def update(uid):
	title = request.form['title']
	sub_title = request.form['subtitle']
	author = request.form['author']
	text = request.form['text']
	update_post(uid, title, sub_title, author, text)
	flash('Blog post was successfully updated')
	return redirect(url_for('blog'))

@app.route('/del/<int:id>')
def delete(id):
	delete_post(id)
	return redirect(url_for('blog'))
```

[![Screenshot from 2017-01-14 11-32-57.png](https://s20.postimg.org/6citrw9od/Screenshot_from_2017_01_14_11_32_57.png)](https://postimg.org/image/jgoe4l1q1/)

```
html
# admin.html

{% extends "base.html" %}

{% block head %}Admin{% endblock %}

{% block title %}
	<h1>Admin Panel</h1>
	<hr class="small">
{% endblock %}

{% block content %}
	{% if posts %}
		<h2>Published blog posts</h2>
		<br>
		<div class="table-responsive">
			<table class="table table-condensed table-bordered">
				<thead class="active">
					<tr>
						<th>Title</th>
						<th>Published</th>
						<th></th>
						<th></th>
					</tr>
				</thead>
				<tbody>
					{% for post in posts %}
						<tr>
							<td>{{ post.title }}</td>
							<td>{{ post.pubdate }}</td>
							<td><a href="{{ url_for('show_update_post', id=post.id) }}"><span class="glyphicon glyphicon-edit"></span></a></td>
							<td><a href="{{ url_for('delete', id=post.id`) }}"><span class="glyphicon glyphicon-trash"></span></a></td>
						</tr>
					{% endfor %}
				</tbody>
			</table>
		</div>
	{% else %}
		<h3>No blog posts are available yet</h3>
	{% endif %}

	<hr>
	<h2>Add new blog post</h2>
	<br>
	<form action="{{ url_for('add_post') }}" method=post>
		<div class="form-group">
			<label for="title">Title:</label>
			<input type="text" class="form-control" name="title">
		</div>
		<div class="form-group">
			<label for="sub-title">Sub Title:</label>
			<input type="text" class="form-control" name="subtitle">
		</div>
		<div class="form-group">
			<label for="author">Author:</label>
			<input type="text" class="form-control" name="author">
		</div>
		<div class="form-group">
			<label for="text">Text:</label>
			<textarea rows="30" class="form-control" name="text"></textarea>
		</div>
		<button type="submit" class="btn btn-primary">Add Post</button>
	</form>
{% endblock %}
```

[![Screenshot from 2017-01-14 11-31-56.png](https://s20.postimg.org/58ypfxp19/Screenshot_from_2017_01_14_11_31_56.png)](https://postimg.org/image/8skn5qrqx/)



**Step 8: List all blog posts**

```
python
# blog.py

def get_posts():
	results = devless.get_data('blog', 'post')
	return results

@app.route('/')
def blog():
	all_posts = get_posts()
	posts = all_posts['payload']['results']
	posts = sorted(posts, key=lambda k: k['id'], reverse=True)
	return render_template('index.html', posts=posts)
```

[![Screenshot from 2017-01-14 11-33-10.png](https://s20.postimg.org/5bn6gii2l/Screenshot_from_2017_01_14_11_33_10.png)](https://postimg.org/image/neg97qdx5/)

```
html
# index.html

{% extends "base.html" %}

{% block head %}Home{% endblock %}

{% block title %}
	<h1>My Blog</h1>
	<hr class="small">
{% endblock %}

{% block sub_title %}A Simple Blog Engine using DevLess Backend{% endblock %}

{% block content %}
{% if posts %}
	{% for post in posts %}
		<div class="post-preview">
			<a href="{{ url_for('show_post', id=post.id) }}">
				<h2 class="post-title">{{ post.title }}</h2>
				<h3 class="post-subtitle">{{ post.subtitle }}</h3>
			</a>
			<p class="post-meta">Posted by {{ post.author }} on {{ post.pubdate }}</p>
		</div>
	{% endfor %}
{% else %}
	<h2 class="post-title">No blog posts are available yet</h2>
{% endif %}
{% endblock %}
```

**Step 9: Show a particular blog post**

```

python

# blog.py

def get_post_by_id(id):
	results = devless.where('id', id).get_data('blog', 'post')
	return results

@app.route('/<int:id>')
def show_post(id):
	post = get_post_by_id(id)
	print post
	post = post['payload']['results'][0]
	return render_template('show-post.html', post=post)
```
[![Screenshot from 2017-01-14 11-46-11.png](https://s20.postimg.org/505q3r1ml/Screenshot_from_2017_01_14_11_46_11.png)](https://postimg.org/image/8jrntk4c9/)

```
html

# show-post.html

{% extends "base.html" %}

{% if post %}
	{% block head %}{{ post.title }}{% endblock %}

	{% block title %}
		<h1>{{ post.title }}</h1>
		<hr class="small">
	{% endblock %}
	
	{% block sub_title %}{{ post.author }}{% endblock %}
	
	{% block content %}
		<p>{{ post.text }}</p>
	{% endblock %}
{% else %}
	<h2 class="post-title">Sorry, the requested blog post is not available</h2>
{% endif %}
```


**Step 10: A bit of styling for our blog engine**

```css
body {
  font-size: 20px;
}
p {
  line-height: 1.5;
  margin: 30px 0;
}
a {
  color: #333;
}
hr.small {
  max-width: 100px;
  margin: 15px auto;
  border-width: 4px;
}
.intro-header {
  margin-bottom: 20px;
}
.intro-header .site-heading,
.intro-header .post-heading,
.intro-header .page-heading {
  padding: 40px 0 20px;
}
@media only screen and (min-width: 768px) {
  .intro-header .site-heading,
  .intro-header .post-heading,
  .intro-header .page-heading {
    padding: 20px 0;
  }
}
.intro-header .site-heading,
.intro-header .page-heading {
  text-align: center;
}
.intro-header .site-heading h1,
.intro-header .page-heading h1 {
  margin-top: 0;
  font-size: 40px;
}
.intro-header .site-heading .subheading,
.intro-header .page-heading .subheading {
  font-size: 24px;
  line-height: 1.1;
  display: block;
  font-weight: 300;
  margin: 10px 0 0;
}
@media only screen and (min-width: 768px) {
  .intro-header .site-heading h1,
  .intro-header .page-heading h1 {
    font-size: 50px;
  }
}
.intro-header .post-heading h1 {
  font-size: 35px;
}
.intro-header .post-heading .subheading,
.intro-header .post-heading .meta {
  line-height: 1.1;
  display: block;
}
.intro-header .post-heading .subheading {
  font-size: 24px;
  margin: 10px 0 30px;
  font-weight: 600;
}
.intro-header .post-heading .meta {
  font-style: italic;
  font-weight: 300;
  font-size: 10px;
}
@media only screen and (min-width: 768px) {
  .intro-header .post-heading h1 {
    font-size: 55px;
  }
  .intro-header .post-heading .subheading {
    font-size: 30px;
  }
}
.post-preview > a:hover,
.post-preview > a:focus {
  text-decoration: none;
}
.post-preview > a > .post-title {
  font-size: 30px;
  font-weight: 600;
  margin-top: 30px;
  margin-bottom: 5px;
}
.post-preview > a > .post-subtitle {
  margin: 0;
  font-weight: 300;
  margin-bottom: 10px;
}
.post-preview > .post-meta {
  font-size: 18px;
  font-style: italic;
  margin-top: 0;
}
@media only screen and (min-width: 768px) {
  .post-preview > a > .post-title {
    font-size: 36px;
  }
}
footer {
  padding: 50px 0 65px;
}
footer .list-inline {
  margin: 0;
  padding: 0;
}
footer .copyright {
  font-size: 14px;
  text-align: center;
  margin-bottom: 0;
}
```

We should now have a simple blog engine with Create, Read, Update and Delete capabilities, persisted with a DevLess backend.

You can checkout the full code in this [github repo](https://github.com/johnotu/BlogEngineWithDevLess)
