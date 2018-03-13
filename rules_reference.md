# Rules

### What are Rules?

Rules allow you set conditions and rules which may modify incoming data or output by providing you  a [method chain](https://en.wikipedia.org/wiki/Method_chaining?oldformat=true#PHP) based  [DSL](https://www.wikiwand.com/en/Domain-specific_language).  The methods provided are verbose, this is to ensure readability. To get the best out of Rules, it's recommended that you are thorough with this page.

### How to get started

On creating a new service you will be redirected to the service page where you will find a section where you may write out rules for that particular service. This means any data action ie: query, delete, adding or updating data to that particular Service table will be affected by the rule that you write.  
![](/assets/service_rules.png) A newly created service will have a rules page similar to the one above.
re
### Rules Syntax definition

Rules provide you with ways to modify data from the client as well as modify what is being sent back. Rules is based on a PHP method chain. Which is a bunch of methods you call together to get particular results.   
Just like  method chains in any language, you may chain up a bunch of methods like so `->beforeQuering()->assign(input_name)->to(name)`.  
A couple of things to note though is that:

* Rules in DevLess uses the arrow operator `->` for joining methods together instead of `.` as seen in many languages.

* To concatenate strings together you are advised to use the `concatenate` method as `.` is used for concatenation in PHP and this might be a little confusing. So instead of `->beforeQuerying()->assign("hello "." world")->to(greetings)` do  `->beforeQuerying()->concatenate("hello ","world" )->storeAs(greetings)`

* Although PHP variables starts with a `$` prefix you may choose to omit this when working with DevLess rules , eg `->beforeQuering()->assign(input_name)->to(name)` and `->beforeQuering()->assign($input_name)->to($name)` will work fine with DevLess Rules. 

## NB **during compilation of Rules DevLess prefixes all variables with **`$`** before the PHP interpreter runs it. And so from time to time you will find situations where you have to set the **`$`** prefix yourself.**

* There are times you might have to work with DevLess arrays. DevLess Rules implement [arrays as seen in PHP](http://php.net/manual/en/function.array.php) eg `["key" => "value"]` but are referred to as collections in DevLess Rules. 

### Database events

Rules are database event driven .This means that Rules will only execute based on a database actions.Examples of database actions are  `Querying`, `Adding`, `Updating` and `Deleting` Records.  
 For any newly created service  you will find these events already set in there.   
For example if you will like to capitalize all first names you are about to add to the database you will have something similar to the below

    /**
    * <?
    * Rules allow you to establish control over the flow of 
    * your data in and out of the database.
    * For example if you will like to change the output message 
    * your users receive after quering for data,
    * its as easy as `afterQuerying()->mutateResponseMessage("to something else")`. 
    * To view the list of callable method append ->help() to a 
    * flow statement ie ->beforeQuering()->help() and view from your app.
    **/
     -> beforeQuerying()
     -> beforeUpdating()
     -> beforeDeleting()
     -> beforeCreating()
                    ->onTable('register')->convertToUpperCase(input_first_name)->storeAs(input_first_name)

     -> onQuery()
     -> onUpdate()
     -> onDelete()
     -> onCreate()

     -> onAnyRequest()

     -> afterQuerying()
     -> afterUpdating()
     -> afterDeleting()
     -> afterCreating()

You will write your code under the `beforeCreating()` method to denote that you want this action to only be performed  when a write is about to be made to the `register` table .

Similarly the  **after** methods will be run when the db action is performed .  
You most likely will  be mutating the output response using the **after** db actions or performing actions like sending out emails . We will touch this a little bit more when we take a look at [Modifying Default output](#modifying-default-output) .

The **on** prefixed  methods eg `onQuery` will run before and after the respective db action. You may use these to execute Rules that you need to run on both before and after actions.

Also the **onAnyRequest** method will execute Rules on any kind of db actions made to the service.

### Using incoming inputs in Rules

In the case of DB actions that require the client to pass in variables. eg Adding data and updating records , these data will be made available within Rules as variables prefixed with `input_` .   
For example if the client sends in JSON data like `{"liked":0, "comment": "I agree with you"}`,  
the keys will be converted to variables prefixed with  `input_` and assigned the values and so now you will be able to access the liked values through the variable `input_liked` and comments through ,`input_comment`.

### Working with variables

Just like in many programming languages. Rules allow you to create and access variables.   
To assign a variable you use the `assign` `to` combination. eg `assign("DevLess")->to(name)` this is similar to `name = "DevLess"` in many programming languages   
Once you assign a value to a variable you can pass this to any method to be used. eg `->convertToUpperCase(name)`   
Another instance where you might want to set the value of a variable is when getting the output of a method. eg `->convertToUpperCase(name)->storeAs(nameInUpperCase) //nameInUpperCase = "DEVLESS"`.  
The method `convertToUpperCase` will convert the name given to it to upperCase and store the new uppercase name in the variable nameInUpperCase.  
**NB:** If you make reference to a variable without assigning it, it will be set to `null` .

### Modifying Data

DevLess rules are good for running custom logic on incoming data. The reason you might need this is to make sure data sent to the backend by the client is in the format required.   
Since in most instances you might be using DevLess to serve both your mobile and web app. You may want to modify the backend instead of on the frontend, so that you do it once instead of having to do it over the different platforms ie: Mobile and web .   
Devless provides inbuilt methods to help with the modificatiion of incoming data.

#### Working with Strings

Rules provide a host of methods you may use to modify you text inputs.   
Eg `->getCharacter(5, "Devless")->storeAs(word) // word = "s"` This will return the 5th word in Devless counting from 0.  
There are a whole bunch of string methods which you may use. [String Methods](#strings-methods)

#### Working with Numbers

Rules also allow you to perform arithmetic operations. One use case for this is calculating the price against the quantity of a product, as you wouldn't want to entrust this to   
the client side Eg. `->calculate(2+3/2)->storeAs(ans) //ans = 3.5`.  
Get the entire list of [Math Methods](#Math-Methods)

#### Generating Unique and Random Values

There are methods available incase you will like to generate unqiue identifiers. These methods are known as generators.   
Eg: ``->generateUniqueId()->storeAs(uniqueId) `uniqueId = '5a155ef7dc237'``  
Complete list of [Generator Methods](#generator-methods) can be found [here](#generator-methods)

#### Working with Dates in DevLess

You may want to get a timestamp or formatted date. Either way the Date Methods provides you options.   
Eg: `->getTimestamp()->storeAs(stamp)->succeedWith(stamp)`   
Find the complete list of [Date Methods here](#date-methods)

#### EVENT Variable

We already saw the input\_\* variable that gives you access to incoming data within rules. There is also the `EVENT` variable that  
contains information about the current state of the entire service including the rules currently being run, as well as the logged in users   
`id` and `token`. To view the entire list of info contained in `EVENT`  
add this line to the rules of any service eg: `->stopAndOutput(1000, 'content of event variable', EVENT)` and head over to the api console and see what you get.  
You should have something similar to the below `{  
    "status_code": 1000,  
    "message": "content of event variable",  
    "payload": {  
        "method": "GET",  
        "params": [],  
        "script": "->beforeQuerying()->stopAndOutput(1000, \"content of event variable\", $EVENT);",  
        "user_id": "",  
        "user_token": "",  
        "request_type": "db",  
        "request_phase": "before",  
        "access_rights": {  
            "query": 1,  
            "update": 1,  
            "delete": 1,  
            "script": 1,  
            "schema": 1,  
            "create": 1,  
            "view": 1  
        },  
        "requestPayload": null,  
        "status_code": null,  
        "message": null,  
        "results_payload": null  
    }  
}`

### Flow Controls

Rules won't be complete without flow controls. Flow Controls provide ways to perform actions only if certain conditions are met.

**-&gt;onTable\('table\_name'\)** Using `onTable` allows you to run code given that the table currently being accessed matches the one set in the `onTable` statement.  
eg: `->beforeQuerying()  
            ->onTable('meals')  
                    ->succeedWith("shows when we are not on meals table")  
            ->end()`  
With the above example the success message will only show if we try to perform a db action on the `meals` table.   
Next lets take a look at choice based flow control statements.

```
     ->whenever(1==2)
             ->succeedWith("In whenever block")
     ->elseWhenever(1==1)
            ->succeedWith("In first elseWhenever block")
    ->elseWhenever(1==1)
           ->succeedWith("In second elseWhenever block")
    ->otherwise()
          ->succeedWith("In otherwise block")
    ->end()
```

In the case of the above example we expect `In first elseWhenever block` to be returned when we run this on the console.   
This is the same as the code below as seen in the ruby language.

```
  if 1 == 2 
          puts "In if block"
  elsif 1==1
          puts "In first elsif block"
  elsif 1==1 
          puts "In second elsif block"
  else 
          puts "In otherwise block"
  end
```

### Assertions

From the above you may have noticed that the truth statements in `whenever`, `elseWhenever`  use a very basic form of assertion ie `1 == 2` and `1 ==1`. Sometimes you may need to perform an assertion that is a little bit more complex   
An example could be checking if a string contains a substring , say `edmond` can be found in `edmond@devless.io` .   
This can easily be achieved with the inbuilt Assertion library that comes with Rules.   
ie:  `->beforeCreating()->whenever(assertIt::contains("edmond@devless.io", "edmond"))->then->stopAndOutput(1001,'message', 'email containes edmond')`  
Find the full list of [assertions here](#assertion-methods)

### Modifying Default output

All outputs from DevLess js generally in JSON with the following structure:

`{  
    "status_code": <code_here>,  
    "message": "<message_block>",  
    "payload": <payload_block>  
}`

`status_code`: usually represent the state of request you made and allows your client app to act accordingly.Find the list inbuilt status\_code \(here\)\[status-code\]   
`message`: is a verbose explanation for the `status_code`.   
`payload`: this may contain extra details such as stack traces.

Well within Rules you may intercept the outgoing response either to decide if some other rules should be run or mutate them.

#### getResponse

You can only try to get the response only with the `after` prefixed db methods.  
eg:

    /**
    * <?
    * Rules allow you to establish control over the flow of 
    * your data in and out of the database.
    * For example if you will like to change the output message 
    * your users receive after quering for data,
    * its as easy as `afterQuerying()->mutateResponseMessage("to something else")`. 
    * To view the list of callable method append ->help() to a 
    * flow statement ie ->beforeQuering()->help() and view from your app.
    **/
     -> afterQuerying()
            -> getResponse(status_code, message, payload)
            ->whenever($status_code == 625)
                    ->stopAndOutput(status_code, "in whenever block", payload)

This will output something similar to

```
  {
    "status_code": 625,
    "message": "in whenever block",
    "payload": {
        "results": [
            {
                "id": 1,
                "devless_user_id": 1,
                "name": "kofi"
            },
            {
                "id": 2,
                "devless_user_id": 1,
                "name": "kofi"
            },
            {
                "id": 3,
                "devless_user_id": 1,
                "name": "kofi"
            },
            {
                "id": 4,
                "devless_user_id": 1,
                "name": "mike"
            },
            {
                "id": 5,
                "devless_user_id": 1,
                "name": "sd"
            },
            {
                "id": 6,
                "devless_user_id": 1,
                "name": "mike"
            }
        ],
        "properties": {
            "count": 6,
            "current_count": 6
        }
    }
}
```

You may also get the individual parts of the response with   
`-> getStatusCode()`, `-> getResponseMessage()`, `-> getResponsePayload()`  for  `status_code`, `message` and `payload` respectively.

`->stopAndOutput` You may have noticed `stopAndOutput` being used in multiple examples. This methods stops execution of any rules after it and outputs   
the `status_code` , `message` and `payload` that you provide it. eg `->stopAndOutput(1000, "message goes here", ["name"=>"foo"])` This provides you with the   
option to send out a custom output.

#### mutations.

You may also send out a custom output without necessorily using `stopAndOutput`. To do this you use mutators.   
eg `->afterCreating()->mutateResponse(1000, "message goes here", ["name"=>"foo"])`   
You may also mutate the individual response using `->mutateStatusCode(1000)`,   
`->mutateResponseMessage("message goes here")`,   
`->mutateResponsePayload(["name"=>"foo"])`

### Changing privacy settings dynamically

You may also change the access rights of a service within Rules. There are 4 methods available for modifing privacy.  
Also note that the privacy section on the DevLess Dashboard allows you to configure these privacies at the service level.

**authenticateUser:** based on which part of the rules its used this will require the user to be authenticated to access the backend

**denyExternalAccess:** This will block external access to the backend.

**allowExternalAccess:** This makes every resource available to everyone

**grantOnlyAdminAccess:**  This will locked down every resource but will be accessible to the creator of the DevLess instance only.  
eg: `->beforeCreating()  
          ->authenticateUser()`

### Getting Help with Rules

Rules provide an inbuilt method `->help()` which you may use to find out more about a particular method eg: `->help('stopAndOutput')`   
or just `->help()` to see the full list of callable methods and how they work.

### Fillers

Fillers are a set of nice to have constructs in Rules. They do  nothing to the meaning of your rules \(semantically\).   
They are only there for you to add to make your code more readable.  
Eg:  `->beforeCreating()  
            ->whenever(assertIt::contains("edmond@devless.io", "edmond"))  
            ->then->stopAndOutput(1001,'message', 'email containes edmond')`  
Notice the `then` keyword before the `stopAndOutput` method. That is an example of a filler you can take it out and your Rules will still have the same behaviour.   
The list of available helpers are:  `also`,`then`,`firstly`,`secondly`,`thirdly`,`lastly`,`beSureTo`,`next`,

### Making External Calls

Rules provide a host of methods that allow you manipulate data before they hit the db. But then Rules is not mean't to be a complete programming language and so don't expect to use this for mainstream programming.  
That been said you may make api Request over to another server within Rules.   
This comes in handy when you have external services and resources you need to accees   
`makeExternalRequest:` eg: `->beforeUpdating()->makeExternalRequest('GET', 'https://www.calcatraz.com/calculator/api?c=3%2A3')->storeAs($ans)  
          ->succeedWith($ans)` This will call on an api that multiplies 3 by 3

`import:` Import allows you to import code from the [ ActionClass](/services.md) Section of any Service.  
 Also you may get access to methods that allow you to perform CRUD actions on Data as well as  Åuth by importing Devless. eg: `->import("devless")  
               ->beforeCreating()  
                  ->queryData("service_name", "table_name")->storeAs(data)  
                  ->stopAndOutput(1000, "message here", data)`  
The above imports `queryData` from `devless` and allows you to query data from other services as well as the current one. Find the list of of methods under the [devless import here](#devless-import)

### Collections

You may have noticed there are no references to loops anywhere above. We try to keep rules as simple as possible. Well that isn't to say not adding loops make things simple. You generally need loops to go over a set of data or run a block of intructions a couple of times.

You will handly need to loop over a bunch of rules  a number of times as rules carry out a more specific task.   
What you might need however is to mutate a list of values in a `collection`.   
A collection is an array of values eg: `["Mike", "Jerry", "John", "Zac" ]`.

The example shows a collection of names.   
You may want to say capitalize all the names in the collection.For this you may use the apply method  
`-> beforeCreating()  
        ->assign(["Mike", "Jerry", "John", "Zac" ])  
                    ->apply("convertToUpperCase")  
        ->to(names)  
        ->stopAndOutput(1000, "inputs", names)`  
 this will return `{  
    "status_code": 1000,  
    "message": "inputs",  
    "payload": [  
        "MIKE",  
        "JERRY",  
        "JOHN",  
        "ZAC"  
    ]  
}`

the `apply` method applys the `convertToUpperCase` method on each element in the collection, thus capitalizing each   
element.   
There are a host of methods that makes [working with collections](#collections-methods) easy.

### String Methods

* **concatenate**: Concatenate strings together eg: `->beforeQuerying()->concatenate("user_","edmond")->storeAs($string)->succeedWith($string) #user_edmond"`
* **getFirstCharacter**: Get first character eg: `->beforeCreating()->getFirstCharacter("Hello")->storeAs($first_char)->succeedWith($first_char) #H`
* **getSecondCharacter**: Get second character eg: `->beforeCreating()->getSecondCharacter("Hello")->storeAs($second_char)->succeedWith($second_char) #e`
* **getThirdCharacter**: Get third character eg: `->beforeCreating()->getThirdCharacter("Hello")->storeAs($third_char)->succeedWith($third_char) #l`
* **getCharacter**: Get nth character eg: `->beforeCreating()->getCharacter(4, "Hello")->storeAs($nth_char)->succeedWith($nth_char) #o`
* **getLastCharacter**: Get last character eg: `->beforeCreating()->getLastCharacter("Hello")->storeAs($last_char)->succeedWith($last_char) #o`
* **getLastButOneCharacter**: Get last but one character eg: `->beforeCreating()->getLastButOneCharacter("Hello")->storeAs($last_but_one_char)->succeedWith($last_but_one_char) #l`
* **reverseString**: Reverse a string eg: \`-&gt;beforeQuerying\(\)-&gt;assign\("nan"\)-&gt;to\($string\)
        ->reverseString()->storeAs($reverseString)
        ->whenever(assertIts::equal($string, $reverseString))
                ->succeedWith("Its a palindrome :)") 
        ->otherwise()
                ->failWith("Its not a palindrom :(")
        #Its a palindrome        
        `
* **findNReplace**: replace a string with another eg `->beforeCreating()->findNReplace("{{name}}", "John", "welcome {{name}}")->storeAs($message)->succeedWith($message) #welcome John`
* **convertToUpperCase**: change string to uppercase eg: `->beforeCreating()->convertToUpperCase("John")->storeAs($name)->succeedWith($name) #JOHN`
* **convertToLowerCase**: change string to lowercase eg: `->beforeCreating()->convertToLowerCase("JOHN")->storeAs($name)->succeedWith($name) #john`
* **truncateString**: Truncate a string to some length eg \`-&gt;beforeCreating\(\)-&gt;truncateString\(11, "some long text", "..."\)-&gt;storeAs\($truncatedString\)-&gt;succeedWith\($truncatedString\) #some lon...`
* **countWords**: Count the number of words in a sentence eg: `->beforeCreating()->countWords("text here")->storeAs($desc_length)->whenever($desc_length <= 5)->failWith("Your product description is very short") #Your product description is very short`
* **countCharacters**: Find the number of characters in a word or sentence eg: `->beforeCreating()->countCharacters("")->storeAs($name_length)->whenever($name_length <= 0)->failWith("name seems to be empty")($input_name)->storeAs($name_length)->whenever($name_length <= 0)->failWith("name seems to be empty") #name seems to be empty`

### Math Methods

* **calculate**: Perform mathematical operations eg: `->beforeQuerying()->calculate(3*5)->storeAs($ans)->stopAndOutput(1001,'got answer successfully', $ans) #15`

  * **sumUp**: find the sum of numbers. eg: `->beforeQuerying()->sumUp(3,4,5,6,7)->storeAs($ans)->stopAndOutput(1001,'got answer successfully', $ans) #25`

  * **subtract**: subtract a bunch of numbers. eg: `->beforeQuerying()->subtract(3,4,5,6,7)->storeAs($ans)->stopAndOutput(1001,'got answer successfully', $ans) #19` also `->beforeQuerying()->from(5)->subtract(3)->storeAs($ans)->stopAndOutput(1001,'got answer successfully', $ans)#2`

  * **multiply**:  find the product of numbers.eg: `->beforeQuerying()->multiply(3,4,5,6,7)->storeAs($ans)->stopAndOutput(1001,'got answer successfully', $ans)#2520`

* **divide**: divide a range of numbers.eg: `->beforeQuerying()->divide(6,2)->storeAs($ans)->stopAndOutput(1001,'got answer successfully', $ans) #3`

* **divideBy**: This picks results from your earlier computation and divides it by a given number. eg: `->beforeQuerying()->sumUp(3,4,5,6,7)->divideBy(6)->storeAs($ans)->stopAndOutput(1001,'got answer successfully', $ans) #4.1666666666667`

* **findSquareRootOf**: Find the square root of a number. eg:`->beforeQuerying()->findSquareRootOf(4)->storeAs($root)->succeedWith($root) #2`

* **getSquareRoot**: Get the squareRoot of the result of the preceeding computation eg: `->beforeQuerying()->divide(20, 40)->getSquareRoot()->storeAs($output)->succeedWith($output) #0.70710678118655`

* **roundUp**: round up a number.eg: `->beforeQuerying()->roundUp(3.3445, 1)->storeAs($answer)->succeedWith($answer) #3.3`

* **percentOf**: Find the percent of a number eg: `->beforeQuerying()->find(10)->percentOf(200)->storeAs($discount)->succeedWith($discount) #20`

### Date Methods

* **getTimestamp**:  The `getTimestamp` method returns the current timestamp. eg: `->beforeQuerying()->getTimestamp()->storeAs($timestamp)->succeedWith($timestamp) #1514656911`

* **getCurrentYear**: Get the current year using the `getCurrentYear` method eg:`->beforeQuerying()->getCurrentYear($word = false)->storeAs($currentYear)->succeedWith($currentYear) #2017`

* **getCurrentMonth**: Get the current month using the `getCurrentMonth` method eg:`->beforeQuerying()->getCurrentMonth($word = false)->storeAs($currentMonth)->succeedWith($currentMonth) #12`

* **getCurrentDay**: Get the current day using the `getCurrentDay` method eg: `->beforeQuerying()->getCurrentDay($word = false)->storeAs($currentDay)->succeedWith($currentDay) #30`

* **getCurrentHour**: Get the current hour using the `getCurrentHour` method eg:`->beforeQuerying()->getCurrentHour()->storeAs($currentHour)->succeedWith($currentHour) #06`

* **getCurrentMinute**: Get the current minute using the `getCurrentMinute` method eg:`->beforeQuerying()->getCurrentMinute()->storeAs($currentMinute)->succeedWith($currentMinute) #27`

* **getCurrentSecond**: Get the current second using the `getCurrentSecond` method eg:`->beforeQuerying()->getCurrentSecond()->storeAs($currentSecond)->succeedWith($currentSecond) #02`

* **getFormattedDate**: Get the human readable date using `getFormattedDate` method eg:`->beforeQuerying()->getFormattedDate()->storeAs($formattedDate)->succeedWith($formattedDate) #Saturday 30th of December 2017 06:28:35 PM`

### Generator Methods

* **generateRandomInteger**: generates random integers. This may be used for invitation or promotional code generation. eg `->beforeCreating()->generateRandomInteger($max=10)->storeAs($promo_code)->succeedWith($promo_code) #7`

* **generateRandomAlphanums**:  generates random alphanumeric values. This may be used for generating order Ids. eg `->beforeCreating()->generateRandomAlphanums($length = 5)->storeAs($order_id)->succeedWith($order_id) #QJXIS`

* **generateRandomString**: generates random string.This generates random string codes. eg `->beforeCreating()->generateRandomString($length = 10)->storeAs($promo_code)->succeedWith($promo_code) #vXJNKuBaWK`

* **generateUniqueId**: generates unique Id.This generates unique Id . eg `->beforeCreating()->generateUniqueId()->storeAs($user_id)->succeedWith($user_id) #5a48b44ca776c`

### Assertion Methods

* **anInteger**: check if $value is an integer. eg `->beforeCreating()->whenever(assertIts::anInteger(3))->then->stopAndOutput(1001,'message', 'its an integer') #its an integer`

* **aString**: check if $value is a string. eg: `->beforeCreating()->whenever(assertIts::aString("Hello"))->then->stopAndOutput(1001,'message', 'its a string') #its a string`

* **aBoolean**: check if $value is a  boolean eg: `->beforeCreating()->whenever(assertIts::aBoolean(1 == 1))->then->stopAndOutput(1001,'message', 'its a boolean') #its a boolean`

* **aFloat**: check if $value is a float eg: `->beforeCreating()->whenever(assertIts::aFloat(3.034))->then->stopAndOutput(1001,'message', 'its a float') #its a float`

* **withinRange**: check if $value is within th range $min $max eg: `->beforeCreating()->whenever(assertIts::withinRange($value=2, $min=1, $max=4))->then->stopAndOutput(1001,'message', 'its within range') #its within range`

* **upperCase**: check if $value is upppercase eg: `->beforeCreating()->whenever(assertIts::upperCase("HELLO"))->then->stopAndOutput(1001,'message', 'its upper case') #its upper case`

* **lowerCase**: check if $value is lowercase. eg: `->beforeCreating()->whenever(assertIts::lowerCase("hello"))->then->stopAndOutput(1001,'message', 'its lower case ') #its lower case`

* **alphanumeric**: check if $value is alphanumeric. eg: `->beforeCreating()->whenever(assertIts::alphanumeric("E23D"))->then->stopAndOutput(1001,'message', 'its alphanumeric') #its alphanumeric`

* **alphabets**: check if $value are alphabets eg: `->beforeCreating()->whenever(assertIts::alphabets("abcd"))->then->stopAndOutput(1001,'message', 'its alphabets') #its alphabets`

* **startsWith**: check if $value startswith $prefix eg: `->beforeCreating()->whenever(assertIts::startsWith("E23D", "E"))->then->stopAndOutput(1001,'message', 'it starts with E') #it starts with E`

* **endsWith**: check if $value ends with $suffix eg: `->beforeCreating()->whenever(assertIts::endsWith("E23D", "D"))->then->stopAndOutput(1001,'message', 'it ends with D') #it ends with D`

* **matchesRegex**: check if $value is matched regex eg: `->beforeCreating()->whenever(assertIt::matchesRegex("edmond@devless.io", "<email-regex-goes-here>"))->then->stopAndOutput(1001,'message', 'it matches the email regex')`

* **anEmail**: check if $value is an email eg: `->beforeCreating()->whenever(assertIts::anEmail("edmond@devless.io"))->then->stopAndOutput(1001,'message', 'its an email') #its an email`

* **notEmpty**: check if $value is not an empty array or empty string eg: `->beforeCreating()->whenever(assertIt::notEmpty("some text"))->then->stopAndOutput(1001,'message', 'its not empty') #its not empty`

* **isEmpty**: check if $value is  an empty array or empty string eg: `->beforeCreating()->whenever(assertIt::isEmpty(""))->then->stopAndOutput(1001,'message', 'its  empty') #its  empty`

* **contains**: check if $value is contains $subString eg: `->beforeCreating()->whenever(assertIt::contains("edmond@devless.io", "edmond"))->then->stopAndOutput(1001,'message', 'email containes edmond') #email containes edmond`

* **equal**: check if $value equals $value1 eg: `->beforeCreating()->whenever(assertIts::equal("a", "a"))->then->stopAndOutput(1001,'message', 'a is equal to a :)') #a is equal to a :)`  
     \*

* **notEqual**: check if $value is not equal to $value1 eg: `->beforeCreating()->whenever(assertIts::notEqual("a", "b"))->then->stopAndOutput(1001,'message', 'a is not equal to b ') #a is not equal to b`

* **greaterThan**: check if $value is greater than $value1 eg: `->beforeCreating()->whenever(assertIt::greaterThan(45, 12))->then->stopAndOutput(1001,'message', '45 is greater than 12') #45 is greater than 12`

* **lessThan**: check if $value is less than $value1 eg: `->beforeCreating()->whenever(assertIt::lessThan(12, 45))->then->stopAndOutput(1001,'message', '12 is less than 45') #12 is less than 45`

* **greaterThanOrEqualTo**: check if $value is greater than or equal to $value1 eg: `->beforeCreating()->whenever(assertIt::greaterThanOrEqualTo(45, 45))->then->stopAndOutput(1001,'message', '45 is greater than or equal to 45') #45 is greater than or equal to 45`

* **lessThanOrEqualTo**: check if $value is less than or equal to $value1  `->beforeCreating()->whenever(assertIt::lessThanOrEqualTo($v=45, $v1=45))->then->stopAndOutput(1001,'message', "$v is less than or equal to $v1") #45 is less than or equal to 45`

### Devless Import

* **signUp**: Signup new users  `->beforeCreating()->run('devless','signUp', [$email = "team@devless.io",$password = "pass",$username = null,$phone_number = "020198475",$first_name = "John",$last_name = "Doe",$remember_token = null,$role = 5,$extraParams = null])->storeAs($output)->stopAndOutput(1000, "Created Profile Successfully",$output)`

* **login**: login users  `->beforeCreating()->run('devless','login', [$username = null, $email = "team@devless.io", $phone_number = null, $password = "pass"])->storeAs($output)->stopAndOutput(1000, "login user Successfully")`

* **addData**: add data to a service `->import('devless')->beforeCreating()->addData('service_name','table_name',["name"=>"mike"])->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **queryData**:  Get data from a service table `->import('devless')->beforeCreating()->queryData('service_name','table_name',["where"=>["id,1"]])->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **getData**: Get data from a service table `->import('devless')->beforeCreating()->getData('service_name','table_name',["where"=>["id,1"]])->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **updateData**: update data in a service table `->import('devless')->beforeCreating()->updateData('service_name','table_name', "id", 1, ["name"=>"mike"])->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **deleteData**:  delete record from a service table `->import('devless')->beforeCreating()->deleteData('test','sample', 1)->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **getUserProfile**: get user profile by id `->import('devless')->beforeCreating()->getUserProfile(2)->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **getAllUsers**: Get all users `->import('devless')->beforeCreating()->getAllUsers(2)->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **deleteUserProfile**: Delete a users profile `->import('devless')->beforeCreating()->deleteUserProfile(9)->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **updateUserProfile**: Update a users profile `->import('devless')->beforeCreating()->updateUserProfile($id=1,$email = '',$password = '',$username = 'eddymens',$phone_number = '',$first_name = '',$last_name = '')->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **getUserWhere**: get user profile using a field `->import('devless')->beforeCreating()->getUserWhere("username", "foo")->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **deactivateUserAccount**: Deactivate user account `->import('devless')->beforeCreating()->deactivateUserAccount("username", "foo")->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **activateUserAccount**: Activate User Account`->import('devless')->beforeCreating()->activateUserAccount("username", "foo")->storeAs($output)->stopAndOutput(1000, "output", $output)`

* **toggleUserAccountState**: Toggle User Account Status`->import('devless')->beforeCreating()->toggleUserAccountState(0, "username", "foo")->storeAs($output)->stopAndOutput(1000, "output", $output)`

### Collections Methods

* **fromTheCollectionOf**: convert an array into a collection eg: `->beforeCreating()->fromTheCollectionOf(["name"=>"mike", "age"=>29])->getAllKeys()->storeAs($keys)->stopAndOutput(1000, "got response", $keys) #["name", "age"]`

* **collect**: convert an array into a collection eg: `->beforeCreating()->collect(["name"=>"mike", "age"=>29])->getAllKeys()->storeAs($keys)->stopAndOutput(1000, "got response", $keys) #["name", "age"]`

* **getValuesWithoutKeys**: gets all the values in a collection eg: `->beforeCreating()->collect(["name"=>"mike", "age"=>29])->getValuesWithoutKeys()->storeAs($values)->stopAndOutput(1000, "got response", $values) # ["mike",29]`

* **getAllKeys**: convert an array into a collection eg: `->beforeCreating()->fromTheCollectionOf(["name"=>"mike", "age"=>29])->getAllKeys()->storeAs($keys)->stopAndOutput(1000, "got response", $keys) #["name", "age"]`

* **getFirstElement**: get the first element in a collection eg: `->beforeCreating()->fromTheCollectionOf(["name"=>"mike", "age"=>29])->getFirstElement()->storeAs($element)->stopAndOutput(1000, "got response", $element) #["mike"]`

* **getFirstElement**: convert an array into a collection eg: `->beforeCreating()->fromTheCollectionOf(["name"=>"mike", "age"=>29])->getFirstElement()->storeAs($element)->stopAndOutput(1000, "got response", $element) #["mike"]`

* **appendCollectionTo**: Match up and pair collections eg: `->beforeCreating()->collect(["name"=>"mike", "age"=>29])->appendCollectionTo($superArray=[["id"=>1,"name"=>"sam"],["id"=>2,"name"=>"josh"]], $subArray=[["id"=>2,"age"=>20],["id"=>1,"age"=>12]], $superKey="id",$subKey="id", $resultingKey="result" )->storeAs($element)->stopAndOutput(1000, "got response", $element)`

* **appendCollectionToRelated**: match up and pair collections but store results in related.eg: `->beforeCreating()->collect(["name"=>"mike", "age"=>29])->appendCollectionToRelated($superArray=[["id"=>1,"name"=>"sam"],["id"=>2,"name"=>"josh"]], $subArray=[["id"=>2,"age"=>20],["id"=>1,"age"=>12]], $subArray="id",$subKey="id", $resultingKey="result" )->storeAs($element)->stopAndOutput(1000, "got response", $element)`

* **getElement**: get the nth element in a collections eg: `->beforeCreating()->collect(["Joe", "Sam", "Mike"])->getElement(1)->storeAs($element)->stopAndOutput(1000, "got response", $element) #Joe`

* **getLastElement**: get the last element in a collections eg: `->beforeCreating()->collect(["Joe", "Sam", "Mike"])->getLastElement()->storeAs($element)->stopAndOutput(1000, "got response", $element) #Mike`

* **countTheNumberOfElements**: count the number of elements in a collections eg: `->beforeCreating()->collect(["Joe", "Sam", "Mike"])->countTheNumberOfElements()->storeAs($count)->stopAndOutput(1000, "got response", $count) #3`

* **fetchAllWith**: Fetch all elements whos key are of a particular value eg: `->beforeCreating()->collect([["item"=>"soap", "quantity"=>5],["item"=>"milk", "quantity"=>3],["item"=>"book", "quantity"=>5]])->fetchAllWith("quantity", 5)->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #[["item"=>"soap", "quantity"=>5],["item"=>"book", "quantity"=>5]]`

* **fetchAllWithout**: Fetch all elements whos key are of a particular value eg: `->beforeCreating()->collect([["item"=>"soap", "quantity"=>5],["item"=>"milk", "quantity"=>3],["item"=>"book", "quantity"=>5]])->fetchAllWithout("quantity", 5)->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #[["item"=>"milk", "quantity"=>3]]`

* **fetchOnly**: get a new collection of only a particular key  eg: `->beforeCreating()->collect([["item"=>"soap", "quantity"=>5],["item"=>"milk", "quantity"=>3],["item"=>"book", "quantity"=>5]])->fetchOnly("quantity")->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #[5,3,5]`

* **apply**: apply a method to a collection eg: `->beforeCreating()->collect(["Joe", "Mike"])->apply("convertToUpperCase", $params = [])->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #["JOE","MIKE"]`

* **NB: apply can be used as a for loop. using $ITR and #counter as the index counter ** 
``` ->assign([["name"=>"sackey","id"=>1],["name"=>"kalid","id"=>2],["name"=>"roff","id"=>3]])->to($data)
        ->apply('updateData', ['service_name', 'table_name', 'id', $ITR($data,'#counter.id'), $ITR($data, '#counter')])
 ```
* **applyOnElement**: apply a method to a key in the collection eg: `->beforeCreating()->collect([["name"=>"Joe", "age"=>12],["name"=>"Mark", "age"=>23]])->applyOnElement("convertToUpperCase", "name" )->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #[["name"=>"JOE", "age"=>12],["name"=>"MARK", "age"=>23]]`

* **reverseTheCollection**: reverse the order of a collection eg: `->beforeCreating()->collect(["Joe", "Mike"])->reverseTheCollection()->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #["Mike","Joe"]`

* **sortCollectionBy**: sort the order of a collection by a key eg: `->beforeCreating()->collect(["Zina", "Adam"])->sortCollectionBy("name")->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #["Adam","Zina"]`

* **offsetCollectionBy**: offsets N number of elements eg: `->beforeCreating()->collect(["Adam", "Ben", "Zina"])->offsetCollectionBy(1)->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #["Ben","Zina"]`

* **reduceNumberOfElementsTo**: reduce the number of elements to N eg: `->beforeCreating()->collect(["Adam", "Ben", "Zina"])->reduceNumberOfElementsTo(1)->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #["Adam"]`

* **paginateCollection**: offset N elements and get Y elements eg: `->beforeCreating()->collect(["Adam", "Ben", "Zina"])->paginateCollection(1, 1)->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #["Ben"]`

* **findCollectionDiffs**: find the difference between two collections eg: `->beforeCreating()->findCollectionDiffs(["name"=>"Mark", "age"=>45], ["name"=>"Mark"])->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #  ["age"=> 45]`

* **expandCollection**:  expand and flatten a collection eg: `->beforeCreating()->expandCollection(["name"=>["Mark", "zowy"], "age"=>45])->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #[["name"=>"Mark","age"=>45],["name"=>"zowy","age"=>45]]`

* **addElementToCollection**: add an element to a collection eg: `->beforeCreating()->collect(["name"=>"mike"])->addElementToCollection(23,"age")->storeAs($collection)->stopAndOutput(1000, "got response", $collection)  #["age"=>23,"name"=>"mike"]`

* **removeElementFromCollection**: remove an element from collection  eg: `->beforeCreating()->collect(["age"=>23,"name"=>"mike"])->removeELementFromCollection("age")->storeAs($collection)->stopAndOutput(1000, "got response", $collection)#["name"=>"mike"]`

* **useCollectionAsKeys**: create key value pairs from two collections  eg: `->beforeCreating()->collect(["Mark",23])->useCollectionAsKeys(["name", "age"])->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #["name"=>"Mark","age"=>23]`

* **checkIfCollectionContains**: check if a collection contains a key or value  eg: `->beforeCreating()->collect(["Mark",23])->checkIfCollectionContains(["Mark"])->storeAs($collection)->stopAndOutput(1000, "got response", $collection) #true`



