# DevLess Status Codes

| **STATUS CODE**  | DESCRIPTION | EXPLANATION |
| :--- | :---: | :---: |
| 400 | Sorry something went wrong with payload\(check JSON format\) | You should almost never see this error if you use DevLess via any of the SDKs . But if you ever use the raw Endpoints where you have to pass in JSON chances are you pass in a malformed JSON and you get this message.😐 |
| 600 | Data type does not exist | In the event you ask DevLess to create you a table field with a type it doesn't understand it will yell at you with this message. [DevLess data types](devless-status-codes.md) |
| 601 | Reference column name does not exist | In the process of creating a relationship between two tables and you intentionally or unintentionally select a column that does not exist for referencing you get this error. |
| 602 | Database schema could not be created | DevLess works very hard to make things simple and as part of that sometimes it still has to work to make your life easier to the point sometimes it shuts up and work when its not feeling well. But when it breaks down in the middle of creating a new table and can't say it, it sends you this error. |
| 603 | Table could not be created | Same explanation as the above 👆 |
| 604 | Service does not exist or you just misspelt it. Also be sure the service is set to active | Since Service names are case sensitive DevLess might not get it when you miss the case or misspell your service. Which in this case means the service does not exist as DevLess can't find it |
| 605 | No such resource, try \(rpc db view or schema\) | DevLess allows one to perform three major actions namely working with data belonging to a service known as  **db.** Working with **rpc** for invoking classes from within DevLess and **schema**   for creating tables. You will hardly encounter this error if you use the official [DevLess SDKs](sdks.md). Also note that these are to be passed in as lower case |
| 606 | Created table successfully | In the event your table creation process was successful you get this message |
| 607 | Could not find the right DB method | So this message is likely to hit someone working on the internals of DevLess. This error arises in the event the DB method chosen does not belong to the following **\[query, create,update,delete\]** |
| 608 | Request method not supported | So there are a host of request method types . If you ever decide to send in a request using a request type outside of **POST, GET,PATCH,DELETE** you see DevLess is going to yell at you |
| 609 | Data has been added to the table successfully | You get this when your Data hits the DB |
| 610 | Query parameter does not exist | In the event where you manufacture your own query params and expect DevLess to understand them I will like to say you are too kind. [List of query params](https://github.com/devless/devless-docs-1-3-0/tree/949b00258810c377469907e7bc8021ecb2d4412d/api-engine.md) |
| 611 | Table name is not set | This means you didn't pass in the table name parameter when working with DB action |
| 612 | Query parameter is not set | The query parameter you passed in the URL does match any within DevLess |
| 613 | Database has been deleted successfully | When your Data finally hits the Database |
| 614 | Parameters 'where' and 'data' not set | Chances are there is something wrong with your delete payload. check it out [here](https://github.com/devless/devless-docs-1-3-0/tree/949b00258810c377469907e7bc8021ecb2d4412d/api-engine.md) |
| 615 | Delete action not set | Same explanation as the above 👆 |
| 616 | There is something wrong with your field | When the field you have specified for logging in /signup is not identified by DevLess |
| 617 | No such table belongs to the service | When the name  provided does not belong to the specified service |
| 618 | Validator type does not exist | So DevLess matches data types with validators . In the event the specified validation type specified does not exist DevLess yells. Contributors working on the core are more likely to see this compared to an ordinary user |
| 619 | Table was updated successfully | When you issue a record update and everything goes smoothly |
| 620 | Table could not be deleted | This hard one may arise when for some reason that was not captured a table fails to delete |
| 621 | Asset file could not be found | This means the file name you typed in is not available in the service folder . If you use the **AssetPath\($payload, 'filename'\)** you will be fine most of the time |
| 622 | Token was updated successfully | This means your devless-token update was successful |
| 623 | Token could not be updated | This is the opposite of the above 👆 |
| 624 | Sorry, this is not an open endpoint | This hmm. You shouldn't be seeing this message anytime soon. |
| 625 | Got response successfully | You will be seeing this message more often as you do more reads than writes |
| 626 | Saved script | You get this message each time you save a service rule |
| 627 | Sorry, no such resource exists or resource is set to private \(you can take this from the Privacy Tab or from the rules section of that service\) | When you try to use your service to get some data or make rpc and you get this it either means it set to private or its not been created yet |
| 628 | Sorry, user is not authenticated, try logging in | Yh this is what you get when you try to access data you are to login to get |
| 629 | Sorry, table could not be updated | For some reason not captured the table updates fails. Most likely the new changes are same as what already exists |
| 630 | failed to push JSON to file | During service exports a JSON representation of the service is generated and mostly due to folder permission issues DevLess is not able to write to file |
| 631 | Sorry access has been evoked \(Please check app to see if your 'DevLess Token' match\) | When DevLess discovers the token you have in your frontend does not match the one DevLess is aware of. You can confirm the token from the app section on DevLess |
| 632 | There is something wrong with your input field |  |
| 633 | Token has expired. Please try logging in again | This simply means you logged in sometime back went dormant and DevLess destroyed your [JWT](https://jwt.io/) token |
| 634 | Seems the table does not exist | When you try to perform an operation on a table and the table does not exist 😂😂😂. . Anyways could be that you spelt it wrongly though |
| 635 | Sorry to use offset, you need to set size | So if you decide to get your data from an offset you will need to set the number of records you want as well . Well this forces you to paginate |
| 636 | Data /  Table / field has been deleted | You get this when you delete either a record field or table. Don't ask me why this message shows for three operations. |
| 637 | Got RPC response successfully | So if your RPC request goes through successfully you get this message but then this doesn't mean the results of the RPC call will be positive |
| 638 | Sorry there is no such method or the method is set to private or protected | Yh so RPC allows you to access classes but based on the privacy clause set on the docstring of each method you may not get access to the method |
| 639 | Sorry, RPC can only be processed over POST | Based on the spec for RPC everything happens over a POST 🙂 and DevLess implements it as such |
| 640 | Sorry, there are no such related tables | When you lie to DevLess telling it table A is related to b and that it should get data from b this is what you get |
| 641 | Something is wrong with your payload | When you are about to add data via JSON and your payload is all wrong .Look up the right way from [here](https://github.com/devless/devless-docs-1-3-0/tree/949b00258810c377469907e7bc8021ecb2d4412d/api-engine.md). |
| 642 | There is no such method in Rules engine | Mostly you misspelt the method but append **-&gt;help\(\)** to the current action type to get the list of methods eg . if you are querying to **beforeQuering\(\)-&gt;help\(\)** |
| 643 | Sorry, your account is not active | This means the owner of the app which could be you has frozen user accounts |
| 644 | Seems user already exists | When you try to create a user that already exists |
| 645 | Sorry but you are not logged in as Admin | Somethings are for Admin only.🙂 . Get over it. |
| 646 | Seems your query params is malformed | When you pass anything in as query params. Two ways to avoid this: use the SDK or follow URL rules |
| 647 | Seems there is no such route/endpoint. Please check your urls | Shows up when you hit a route\(URL\) that does not exists |
| 700 | Internal system error | This captures all unforseen errors . Sometimes errors from within the core of the framework |

