# Rules Reference


### What are Rules?
Rules allow you set conditions and rules which may modify incoming data or output by providing you  a [method chain](https://en.wikipedia.org/wiki/Method_chaining?oldformat=true#PHP) based  [DSL](https://www.wikiwand.com/en/Domain-specific_language).  The methods provided are verbose, this is to ensure readability.
 
### How to get started
On creating a new service you will be redirected to the service page where you will find a section where you may write out rules for that particular service. This means any data action ie: querying, delete, adding data to that particular Service table will be affected by the rule that you set.
![](/assets/service_rules.png). A newly created service will have a rules page similar to the one above.

### Rules Syntax definition 
Rules is based on a PHP method chain. Which is a bunch of methods you call together to get particular results. 
Just like  method chains in any language, you may chain up a bunch of methods like so `->beforeQuering()->assign(input_name)->to(name)`.  
A couple of things to note though is that:
*  Rules in DevLess uses the arrow operator `->` for joining methods together instead of `.` as seen in many languages.

*  To concatenate strings together you are advised to use the `concatenate` method as `.` is used for concatenation in PHP and this might be a little confusing. So instead of `->beforeQuerying()->assign("hello "." world")->to(greetings)` do  `->beforeQuerying()->concatenate("hello ","world" )->storeAs(greetings)` 
*  Although PHP variables starts with a `$` prefix you may choose to omit this when working with DevLess rules , eg `->beforeQuering()->assign(input_name)->to(name)` and `->beforeQuering()->assign($input_name)->to($name)` will work fine with DevLess Rules.
*  There are times you might have to work with DevLess arrays. DevLess Rules implement [arrays as seen in PHP](http://php.net/manual/en/function.array.php) eg `["key" => "value"]`

### Database events
Rules are database event driven .This means that Rules will only execute based on a database actions.Examples of database actions are  Querying, Adding, Updating and Deleting Records.
 For any newly created service  you will find these events already set in there. 
For example if you will like to capitalize all first names you are about to add to the database you will have something similar to the below 
```

  
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
 
```
You will write your code under the `beforeCreating()` method to denote the fact that you want this action to only performed only when a write is about to be made the `register` table . 

Similarly the  **after** methods will be run when the db action is performed .
You most likely will  be mutating the output response using the **after** db actions or performing actions like sending out emails . We will touch this a little bit more when we take a look at response .

The **on** methods will run before and after the respective db action. You may use this to execute Rules that you need   to run on both before and after actions. 

Also the **onAnyRequest** method will execute Rules on any kind of db actions made to the service. 

### Using incomming inputs in Rules
In the case of  DB actions that require the client to pass in variables. eg Adding data and updating records , these records will be made available within Rules as variables prefixed with `input_` . 
For example if the client sends in JSON data like `{"liked":0, "comment": "I agree with you"}`
the keys will be converted to variables prefixed with  `input_` and assigned the values and so now you will be able to access the liked values through the variable `input_liked ` and comments through ,`input_comments`. 

### Working with variables
Just like in many programming languages. Rules allow you to create and access variables. 
To assign a variable you use the `assign` `to` combination. eg `assign("DevLess")->to(name)` this similar to `name = "DevLess"` in some programming languages 
Once you assign a value to a variable you can pass this to any method to be used. eg `->convertToUpperCase(name)` 
Another instance where you might want to set the value of a variable is getting the output of a method. eg `->convertToUpperCase(name)->storeAs(nameInUpperCase) //nameInUpperCase = "DEVLESS" `.
The method `convertToUpperCase` will convert the name given to it to upperCase and store the new uppercase name in the variable nameInUpperCase.
**NB:** If you make reference to a variable without assigning it a value you will get null back . 


### Modifying Data   
DevLess rules are good for running custom logic on incoming data. The reason you might need this is to make sure data sent to the backend by the client is in the format required. 
Since in most instances you might be using DevLess to serve both your mobile and web app. You may want to modify the backend instead on the frontend, so that you do it once instead of having 
to do it over the different platforms ie: Mobile and web . 
Devless provides inbuilt methods to help with the modificatiion of incoming data. 

#### Working with Strings
Rules provide a host of methods you may use to modify you text inputs. 
Eg ```->getCharacter(5, "Devless")->storeAs(word) // word = "s" ``` This will return the 5th word in Devless counting from 0.
There are a whole bunch of string methods which you may use. [String Methods](#)

#### Working with Numbers
Rules also allow you to perform mathmatical operations. One use case for this is calculating the price against the quantity of a product, as you wouldn't want to entrust this to 
the client side Eg. ```->calculate(2+3/2)->storeAs(ans) //ans = 3.5```.
Get the entire list of [Number Methods](#)

#### Generating Unique and Random Values
There are methods available incase you will like to generate unqiue identifiers. These methods are known as generators. 
Eg: ```->generateUniqueId()->storeAs(uniqueId) `uniqueId = '5a155ef7dc237' ```
Complete list of [Generator Methods]() can be found [here]()

#### Working with Dates in DevLess
You may want to get a timestamp or format some date you have. Either ways the Date Methods allow you do a whole lot with dates and time. 
Eg: ```->getTimestamp()->storeAs(stamp)->succeedWith(stamp)``` 
Find the complete list of [Date Methods here]()

#### EVENT Variable 
We already saw the input_* variable that gives you access to incoming data within rules. There is also the `EVENT` variable that
contains information about the current state of the entire service including the rules currently being run, as well as the logged in users 
`id` and token. To view the entire list of info contained in `EVENT`  
add this line to the rules of any service eg: ``` ->stopAndOutput(1000, 'content of event variable', EVENT)``` and head over to the api console and see what you get.

#### Flow Controls
Rules won't be complete without flow controls. Flow Controls provide ways to perform actions only if certain conditions are met. 

**->onTable('table_name')** Using `onTable` allows you to run code given that the table currently being accessed matches the one set in the `onTable` statement.
eg: `->beforeQuerying()
			->onTable('meals')
					->succeedWith("shows when we are not on meals table")
			->end()
`
With the above example the sucees message will only show if we try to perform a db action on the `meals` table. 
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
This is the same as the code below as seen in languages like ruby. 

```
  if 1 == 2 
  		puts "In if block"
  elsif 1==1
  		puts "In first elsif block"]
  elsif 1==1 
  		puts "In second elsif block"
  else 
  		puts "In otherwise block"
  end
```
#### Assertions 
From the above you may have noticed that the truth statements in `whenever`, `elseWhenever`  use a very basic form of assertion ie `1 == 2` and `1 ==1`. Sometimes you may need to perform an assertion that is a little bit more complex 
An example could be checking if a string contains a substring, `edmond in edmond@devless.io` . 
This can easily be archived with the inbuilt Assertion library that comes with rules. 
ie:  `->beforeCreating()->whenever(assertIt::contains("edmond@devless.io", "edmond"))->then->stopAndOutput(1001,'message', 'email containes edmond')`
Find the full list of [assertions here]() 


#### Modifying Default output

### Changing privacy settings  dynamically 

You may also change the access rights of a service within Rules. There are 4 methods available for modifing privacy.
Also note that the privacy section on the DevLess Dashboard allows you to configure these privacies at the service level.

**authenticateUser:** based on which part of the rules its used this will require the user to be authenticated to access the backend 

**denyExternalAccess:** This will block external access to the backend.

**allowExternalAccess:** This makes every resource available to everyone 

**grantOnlyAdminAccess:**  This will locked down every resource but will be accessible to the creator of the DevLess instance only.

#### Getting Help with Rules
Rules provide an inbuilt method `->help()` which you may use to find out more about a particular method eg: `->help('stopAndOutput')` 
or just `->help()` to see the full list of callable methods and how they work 
 	
#### Fillers 
Fillers are a set of nice to have constructs in Rules. They do  nothing to the meaning of your rules (semantically). 
They are only there for you to add to make your code more readable.
Eg:  `->beforeCreating()
            ->whenever(assertIt::contains("edmond@devless.io", "edmond"))
            ->then->stopAndOutput(1001,'message', 'email containes edmond')`
Notice the `then` keyword before the `stopAndOutput` method. That is an example of a filler you can take it out and your code will still have the same behaviour. 
The list of available helpers are:  `also`,`then`,`firstly`,`secondly`,`thirdly`,`lastly`,`beSureTo`,`next`,

#### Making External Calls 
Rules provide a host of methods that allow you manipulate data before they hit the db. But then Rules is not mean't to be a a complete programming language and so don't expect to use this for mainstream programming.
That been said you may make api Request over to another server within Rules. 
This comes handy when you have external services and resources you need to accees 
`makeExternalRequest:` eg: `beforeUpdating()->makeExternalRequest('GET', 'https://www.calcatraz.com/calculator/api?c=3%2A3')->storeAs($ans)
         * ->succeedWith($ans)` This will call on an api that multiplies 3 by 3 

`import:` Import allows you to import code from the [ ActionClass](/services.md) Section of any Service.
 Also you may get access to methods that allow you to perform CRUD actions  on Data as well as  Åuth by importing Devless. eg: `->import("devless")
               ->beforeCreating()
                  ->queryData("service_name", "table_name")->storeAs(data)
                  ->stopAndOutput(1000, "message here", data) 
`
The above imports queryData from devless and allows you to query data from other services as well as the current one. Find the list of of methods under the [devless import here]()

#### Collections 
You may have noticed there are no reference to loops anywhere above. We try to keep rules as simple as possible. Well that isn't to say not adding loops make things simple. You generally need loops to go over a set of data or run a block of intructions over a couple of times. 

You will handly need to loop over a bunch of rules  a number of times as rules carry out a more specific task. 
What you might need however is to mutate a list of values in a `collection`. 
A collection is an array of values eg: `[ ["name"=>"Mike", "age"=>12], ["name"=>"James", "age"=>20] ]`. 

The example shows a collection of names. 
You may want to say capitalize all the names in the collection.For this you may use the apply method
`-> beforeCreating()
        ->assign(["Mike", "Jerry", "John", "Zac" ])
                    ->apply("convertToUpperCase")
        ->to(names)
        ->stopAndOutput(1000, "inputs", names)
 `
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
There are a host of methods that makes [working with collections]() easy. 













































































































































































































