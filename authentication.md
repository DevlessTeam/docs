## Documentation

- [signUp](#signup)
- [login](#login)
- [getProfile](#getprofile)
- [updateProfile](#updateprofile)
- [logout](#logout)
- [Authentication Tokens](#token)

**NB**: The Call on any of the authentication methods vary based on the [client library](/docs/{{version}}/SDKs) used. This part of the documentation explains the general concepts behind authentication with Devless.

To make use of the Devless authentication engine a method call from within the client has to be made to the parent Devless service. Where the parent service name is devless and method names: 

      
<a name="signup"></a>
## Signup 

``signUp($email, $password, $username,$phone_number, $first_name, $last_name, $remember_token)``

To signup a new User the Devless Engine requires you to at least send over either an email , phone_number or username with a password. You may also send over a first_name and a last_name to be set .Once a User registration is successful Devless will automatically login the User and return the client a session token. In the situation where the registration fails false is returned. 


<a name="login"></a>
## Login

``login($username, $email, $phone_number, $password)``

To log a user in Devless will expect the client to send over either a username, email or phone_number with a password. Once login is successful Devless will return a session token to the client and in case the credentials do not much return false. 

<a name="getprofile"></a>
## getProfile 

``profile()``

This will return the users profile only if the devless_user_token has been set in the header and has not expired. 

<a name="updateprofile"></a>
## updateProfile 

``updateProfile($email, $password, $username,
            $phone_number, $first_name, $last_name, $remember_token)`` 

To update the Users profile pass in the key value pair of the field and value you would like to replace it with. In other for the field to be updated the Devless instance will require the client to pass over the User id.

<a name="logout"></a>
## Logout 

``logout()``  

With a call to this method, the Devless instance will render the users token invalid and the user will not be able to access services that require authentications.


<a name="token"></a>
## Authentication Tokens
The Devless framework uses JWT to generate a secure token for users who successfully login to the instance. The token is returned to the Devless instance within the headers under the key name ``devless_user_token``. A token is valid for an hour if not used and will  require the client to request another by logging in. 
