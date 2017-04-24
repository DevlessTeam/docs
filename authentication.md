## Authentication

- [signUp](#signup)
- [login](#login)
- [getProfile](#getprofile)
- [updateProfile](#updateprofile)
- [logout](#logout)
- [Authentication Tokens](#token)

**NB**: The Call on any of the authentication methods vary based on the [client library](/docs/{{version}}/SDKs) used. This part of the documentation explains the general concepts behind authentication with DevLess.

To make use of the DevLess authentication engine a method call from within the client has to be made to the parent DevLess service. Where the parent service name is `devless` and method names: 


<a name="signup"></a>
## Signup

``signUp($email, $password, $username, $phone_number, $first_name, $last_name, $remember_token)``

To signup a new User the DevLess Engine requires you to at least send over either an email, phone_number or username with a password. You may also send over a first_name and a last_name. Once a User registration is successful DevLess will automatically login the User and return the client a session token as well as profile.In the situation where the registration fails false is returned. 


<a name="login"></a>
## Login

``login($username, $email, $phone_number, $password)``

To login a  user, DevLess will expect the client to send over either a username, email or phone_number with a password. Once login is successful DevLess will return a session token and the profile to the client and in case the credentials do not match, return false. 

<a name="getprofile"></a>
## getProfile

``profile()``

This will return the user's profile only if the devless_user_token has been set in the header and has not expired.

<a name="updateprofile"></a>
## updateProfile

``updateProfile($email, $password, $username,
            $phone_number, $first_name, $last_name, $remember_token)``

To update the Users profile provide the value in the above order. In other for the field to be updated the DevLess instance will require the client to be logged in.

<a name="logout"></a>
## Logout

``logout()``  

With a call to this method, the DevLess instance will render the users token invalid and the user will not be able to access services that require authentications.


<a name="token"></a>
## Authentication Tokens
The DevLess framework generates a JWT(JSON Web Token) for users who successfully login to the instance. The token is returned to the DevLess instance within the headers under the key name ``devless-user-token``. A token is valid for an hour, The client is will have to request another token if they are dormant for an hour.
