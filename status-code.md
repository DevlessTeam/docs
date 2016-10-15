## #NAV

- [System Errors](#system-errors)
- [Service Messages](#service-messages)


<a name="system-errors"></a>
## System Errors
| Status Code   | Message                                                    | Explanation                                                                                                                                                                     |
|---------------|------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| System Errors |                                                            |                                                                                                                                                                                 |
| 400           | Sorry something went wrong with payload(check json format) | The JSON object passed is malformed                                                                                                                                             |
| 700           | Internal system error                                      | This error signifiers either there is an error from within the framework caused .This is usually accompanied by a payload containing the line an file where the error occurred  |
|               |                                                            |                                                                                                                                                                                 |

<a name="service-messages"></a>
## Service Messages

| Status Code      | Message                                         | Explanation                                                                                                                                                                                                |
|------------------|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Service Messages |                                                 |                                                                                                                                                                                                            |
| 622              | Token updated successfully                      | The app token has been updated thus any app with the older version of the token will not be able to access the api engine                                                                                  |
| 623              | Token could not be updated                      | The app token could not be updated                                                                                                                                                                         |
| 624              | Sorry this is not an open endpoint              | The endpoint the user is trying to access is either private or not an endpoint at all                                                                                                                      |
| 628              | Sorry User is not authenticated, try logging in | The Service or Resource the user is trying to access requires that they are authenticated but user has not  authenticated with the api engine checkout the Auth service from the [store](store.devless.io) |
| 633              | Token has expired please try logging in again   | User might have been authenticated but their JWT expired and user needs to login again                                                                                                                     |
|                  |                                                 |                                                                                                                                                                                                            |



<a name="service-errors"></a>
## Service Errors

| Status Code    | Message                                          | Explanation                                                                                                                                                                                                                                                                                                             |
|----------------|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Service Errors |                                                  |                                                                                                                                                                                                                                                                                                                         |
| 618            | Validator type does not exist                    | The api engine checks data against a defined set of validation rules and will throw this error when the validation rule specified does not exist. Some validator types include text,textarea,integer,decimal.... see complete list from [creating table](http://docs.devless.io/docs/0.1/api-engine#Creating-the-table) |
| 631            | Sorry access has been revoked                    | Users receive this error when their access parameters are not valid eg app key, app token                                                                                                                                                                                                                               |
| 630            | Failed to push json to file                      | When using the migrate methods you might encounter this error which means the service schema could not be created .This may be caused by a malformed json or access rights of some kind                                                                                                                                 |
| 627            | Sorry no such resource or resource is private    | Raised when a resource privacy has been set to private                                                                                                                                                                                                                                                                  |
| 608            | Request method not supported                     | the api engine allows request coming in with the following methods only get,post,patch and delete. All other methods used will throw this error                                                                                                                                                                         |
| 604            | Service,does not exist or is not active          | Raised when a service is set to be inactive or does not exist                                                                                                                                                                                                                                                           |
| 605            | No such service resource try (script,db or view) | Raised,when a user tries to access a resource of a service which is not available ..Example of supported resources are view,script, db and schema                                                                                                                                                                       |
