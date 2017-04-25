## HTML SDK

- [Introduction](#intro)
- [Add Record](#add)
- [Query Data](#query)
- [Update Record](#update)
- [Delete Data](#delete)
- [Notification](#notify)
- [Authentication(Signnup/Login)](#auth)
- [Video Tutorial](#watch)


<a name="intro"></a>
## Introduction
**DevLess** provides [multiple ways](/docs/{{version}}/SDKs) to work get DevLess into your app 
 . Of all the many ways of doing this the easiest and quickest way to
  get started is using the DevLess HTML SDK. The HTML SDK allows a 
  developer to start building their app by just writing out HTML.

**NB:** Reading through [Services and How they work ](/docs/{{version}}/service) makes working with the HTML SDK
 a lot easier.

### Getting Ready 
- You should start off by creating an `HTML Page `
- Head to  `App` on your DevLess instance and hit on the connect to app button
- Copy the connection details and paste into the `HTML Page`

This last step makes DevLess available to your page. 
It should look something like the below
```
<script src="http://localhost:8000/js/devless-sdk.js"
 class="devless-connection" 
 devless-con-token="0a0e773f5b565ba7a15d11ee8cb70455"></script>

```
We can now begin to perform activities like add data from the DevLess backend 

<a name="add"></a>
## Add Data
**Add Data** To add data to a service by name `profile` and table name `details`
with fields `firstname`, `lastname` and age `age`.
The snippet below should work. 
`dv-add-oneto:service_name:table_name`
```
<div class="dv-notify"></div>
<div class="dv-notify-success" style="color: green">Details was added successfully</div>
<div class="dv-notify-failed" style="color: red">Failed to  add details to Profile</div>
<form class="dv-add-oneto:profiles:details">
	<input type="text" name="firstname" placeholder="Please enter your first name"><br>
	<input type="text" name="lastname" placeholder="Please enter your last name"><br>
	<input type="number" name="age" placeholder="Please enter your age"><br>
	<button type="submit">Submit</button>
</form>


```
As you may have noticed from the above the form has a class `dv-add-oneto:profile:details`
This tells DevLess that the content of the form should be added to the `details` table of the `profiles` service.

Also the name of the form fields should have the name of the fields of the servic table.
In this case `firstname`, `lastname` and `age` 

**NB**: Remember to add the name to field types like `<textarea name="content">`
Also you can upload images using the HTML SDK. DevLess converts content of the image upload into `base64`
and so you are required to make the field type for this `base64`



<a name="query"></a>
## Query Data
**Query Data** To query data using the `HTML SDK ` is very easy as well.
 ```
<table>
  <tr>
    <th>Firstname</th>
    <th>Lastname</th> 
    <th>Age</th>
  </tr>
  <tbody class="dv-get-all:profiles:details">
  <tr>
    <td class="var-firstname">Eve</td>
    <td class="var-lastname">Jackson</td> 
    <td class="var-age">94</td>
  </tr>	
  </tbody>
  
</table>
   <script src="http://localhost:8000/js/devless-sdk.js" class="devless-connection" devless-con-token="0a0e773f5b565ba7a15d11ee8cb70455"></script>
```
To get data you add the class `dv-get-all:profile:details`
to the outer structure. Now to print out specific records you prefix the value name with the keyword `var`.

**NB**: In the case were you have a related table who's data you will like to get, all you have to do is add the reference table name after the var keyword then the field you will like to get data from. 
 EG. If a merchant's table is referenced then you can get the name of the merchant with `var-merchants-name`.
Also when you are  working with tables remember to add the class in the `<tbody>`.
 
 <a name="update"></a>
 ## Update Data
 **Update Data** Now that we have been able to query data we added to the DevLess backend. Lets try updating it. Shall we?
```
   <table>
     <tr>
       <th>Firstname</th>
       <th>Lastname</th> 
       <th>Age</th>
     </tr>
     <tbody class="dv-get-all:profiles:details">
     <tr>
       <td class="var-firstname">Eve</td>
       <td class="var-lastname">Jackson</td> 
       <td class="var-age">94</td>
       <td><button class="dv-update">Update</button></td>
     </tr>	
     </tbody>
     
   </table>
   <div class="dv-notify"></div>
   <div class="dv-notify-success" style="color: green">Details was updated successfully</div>
   <div class="dv-notify-failed" style="color: red">Failed to  update details to Profile</div>
   <form class="dv-update-oneof:profiles:details">
   	<input type="text" name="firstname" placeholder="Please enter your first name"><br>
   	<input type="text" name="lastname" placeholder="Please enter your last name"><br>
   	<input type="number" name="age" placeholder="Please enter your age"><br>
   	<button type="submit">Update</button>
   </form>
   
   
   <script src="http://localhost:8000/js/devless-sdk.js" class="devless-connection" devless-con-token="0a0e773f5b565ba7a15d11ee8cb70455"></script>
 
``` 
 <a name="delete"></a>
 ## Delete Data
 **Delete Data** To delete data using the SDK is very simple. All you do is add the class `dv-delete` to whatever you want to click to delete the record 
  EG.   
 ```
    <div class="dv-notify"></div>
    <div class="dv-notify-success" style="color: green">Record was deleted successfully</div>
    <div class="dv-notify-failed" style="color: red">Failed to  delete record </div>
    
    <table>
      <tr>
        <th>Firstname</th>
        <th>Lastname</th> 
        <th>Age</th>
      </tr>
      <tbody class="dv-get-all:profiles:details">
      <tr>
        <td class="var-firstname">...</td>
        <td class="var-lastname">...</td> 
        <td class="var-age">...</td>
        <td><button class="dv-update">Update</button></td>
        <td><button class="dv-delete">Delete</button></td>
      </tr>	
      </tbody>
      
    </table>
    <script src="http://localhost:8000/js/devless-sdk.js" class="devless-connection" devless-con-token="0a0e773f5b565ba7a15d11ee8cb70455"></script>
 

```
 
 <a name="notify"></a>
  ## Notify
  **Notify**: This provides the developer ways to know the state of calls made to the DevLess backend, 
 You may have noticed it being used in the ealier examples 
 There are three types of notifiers available. Namely 
- **'dv-notify'** This give debugging info right from the backend
- **'dv-notify-success'** This class causes a the tag to disappear and only show on successful action performed. 
- **'dv-notify-failed'** This class will only show up if the call to the DevLess backend returns false response. 

EG.
```
<div class="dv-notify"></div>
<div class="dv-notify-success">This Div will only show with this message if an action is successful</div>
<div class="dv-notify-failed">This Div will only show with this message if an action is fails</div>

```


 
 <a name="notify"></a>
  ## Notify
  **Notify**: This provides the developer ways to know the state of calls made to the DevLess backend, 
 You may have noticed it being used in the ealier examples 
 There are three types of notifiers available. Namely 
- **'dv-notify'** This give debugging info right from the backend
- **'dv-notify-success'** This class causes a the tag to disappear and only show on successful action performed. 
- **'dv-notify-failed'** This class will only show up if the call to the DevLess backend returns false response. 

EG.
```
<div class="dv-notify"></div>
<div class="dv-notify-success">This Div will only show with this message if an action is successful</div>
<div class="dv-notify-failed">This Div will only show with this message if an action is fails</div>

```


<a name="auth"></a>
## Authentication
The HTML SDK allows you to Signup, Login, Get Profile, Edit your profile and logout 

EG.
## Signup
```
  <form class="dv-signup" action="/dasboard">
    <input type="text" name="username" placeholder="Enter username here">
    <input type="email" name="email" placeholder="Enter email here">
    <input type="number" name="phone_number" placeholder="Enter phone number here">
    <input type="text" name="first_name" placeholder="Enter first name here">
    <input type="text" name="last_name" placeholder="Enter last name here">
    <input type="password" name="password" placeholder="Enter password here">
    <button type="submit">SignUP</button>
  </form>

```
## Login
```
  <form class="dv-login" action="/dasboard">
    <input type="text" name="username" placeholder="Enter username here">
    <input type="email" name="email" placeholder="Enter email here">
    <input type="number" name="phone_number" placeholder="Enter phone number here">
    <!-- Choose between username, phone_number and email-->
    <input type="password" name="password" placeholder="Enter password here">
    <button type="submit">SignUP</button>
  </form>
```
## Get Profile
```
  <div class="dv-profile">
    <span class="var-username"></span>
    <span class="var-email"></span>
    <span class="var-firstname"></span>
    <span class="var-last_name"></span>
    <span class="var-phone_number"></span>
  </div>
```
## Update Profile 

```
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

## Logout
```
  <button type="dv-logout" action="/home">Logout</button>
```

<a name="watch"></a>
  ## Video Tutorial
  I have prepared video tutorial demonstrating the use of the `HTML SDK` [here](#)
   
   
   
 


   


