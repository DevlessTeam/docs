# Rules Reference

### What are Rules?
Rules allow you set conditions and rules which may modify incoming data or output by providing you  a [method chain](https://en.wikipedia.org/wiki/Method_chaining?oldformat=true#PHP) based  [DSL](https://www.wikiwand.com/en/Domain-specific_language).  The methods provided are verbose, this is to ensure readability.
 
### How to get started
On creating a new service you will be redirected to the service page where you will find a section where you may write out rules for that particular service. This means any data action ie: querying, delete, adding data to that particular Service table will be affected by the rule that you set.
![](/assets/service_rules.png). A newly created service will have a rules page similar to the one above.

### Rules Syntax definition 
Rules is based on a PHP method chain. Which is a bunch of methods you call together to get particular results. 
Just like  method chains in any language, you may chain up a bunch of methods like so `beforeQuering()->assign(input_name)->to(name)`.  
A couple of things to note though is that:
*  Rules in DevLess uses the arrow operator `->` for joining methods together instead of `.` as seen in PHP .

Also to concatenate strings together you are advised to use the `concatenate` method as `.` is used for concatenation in PHP and  might be a little confusing. So instead of `beforeQuerying()->assign("hello "." world")` try to use # Rules Reference

### What are Rules?
Rules allow you set conditions and rules which may modify incoming data or output by providing you  a [method chain](https://en.wikipedia.org/wiki/Method_chaining?oldformat=true#PHP) based  [DSL](https://www.wikiwand.com/en/Domain-specific_language).  The methods provided are verbose, this is to ensure readability.
 
### How to get started
On creating a new service you will be redirected to the service page where you will find a section where you may write out rules for that particular service. This means any data action ie: querying, delete, adding data to that particular Service table will be affected by the rule that you set.
![](/assets/service_rules.png). A newly created service will have a rules page similar to the one above.

### Rules Syntax definition 
Rules is based on a PHP method chain. Which is a bunch of methods you call together to get particular results. 
Just like  method chains in any language, you may chain up a bunch of methods like so `beforeQuering()->assign(input_name)->to(name)`.  
A couple of things to note though is that:
*  Rules in DevLess uses the arrow operator `->` for joining methods together instead of `.` as seen in PHP .

*  Also to concatenate strings together you are advised to use the `concatenate` method as `.` is used for concatenation in PHP and this might be a little confusing. So instead of `beforeQuerying()->assign("hello "." world")->to(greetings)` `beforeQuerying()->concatenate("hello ","world" )->storeAs(greetings)` 
*  Also although PHP variables starts with a `$` prefix you may choose to omit this when working with rules , eg `beforeQuering()->assign(input_name)->to(name)` and `beforeQuering()->assign($input_name)->to($name)` will work fine with DevLess Rules.
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
                ->onTable('register')->convertToUpperCase(input_first_name)_>storeAs(input_first_name)

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

### Getting inputs in Rules
In the case of  DB actions that require the client to pass in variables. eg Adding data and updating records , these records will be made available within Rules as variables prefixed with `input_` . 
For example if the client sends in JSON data like `"liked":0, "comment": "I agree with you"}`
the keys will be converted to variables prefixed with  `input_` and assigned the values and so now you will be able to access the liked value through the variable `input_liked ` and comments through ,`


input_comments` 

 




## Keywords

Keywords are methods/functions that allow you to control what happens with your data before storing or after reading. This is the full reference:

| Keywords | Description |
| :--- | :--- |
| also | \`also\` filler word like all other filler words does not come with any side effects but is used sorely for readability purposes.  eg: \`-&gt;beforeCreating\(\)-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;convertToUpperCase\(\)-&gt;reverseString\(\)-&gt;storeAs\($text\)-&gt;succeedWith\($text\)\` this can be rewritten as \`beforeCreating\(\)-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;convertToUpperCase\(\)-&gt;also-&gt;reverseString\(\)-&gt;storeAs\($text\)-&gt;succeedWith\($text\)\` notice the introduction of the \`also\` keyword, this does not change the end results but just makes it more readable. @return $this |
| then | \`then\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes. eg: \`-&gt;beforeCreating\(\)-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;convertToUpperCase\(\)-&gt;reverseString\(\)-&gt;storeAs\($text\)-&gt;succeedWith\($text\)\` this can be rewritten as \`beforeCreating\(\)-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;convertToUpperCase\(\)-&gt;then-&gt;reverseString\(\)-&gt;storeAs\($text\)-&gt;succeedWith\($text\)\` notice the introduction of the \`then\` keyword, this does not change the end results but just makes it more readable. @return $this |
| firstly | \`firstly\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`.  @return $this |
| Secondly | \`secondly\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`. @return $this |
| thirdly | \`thirdly\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`. @return $this |
| lastly | \`lastly\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`. @return $this |
| beSureTo | \`beSureTo\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;beSureTo-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`.  @return $this |
| next | \`next\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;next-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`.  @return $this |
| authenticateUser | Appending the authenticateUser method to any query action will force users to login to gain access to the system eg: -&gt;beforeQuerying\(\)-&gt;onTable\('subscriptions'\)-&gt;autheticateUser\(\)  @return $this |
| denyExternalAccess | In the event you do not want to grant external access to a table to any users you should use \`denyExternalAccess\` method . \`-&gt;beforeQuerying\(\)-&gt;onTable\('subscriptions'\)-&gt;denyExternalAccess\(\)\` In this case no user will be able to access the \`subscriptions\` table  @return $this |
| allowExternalAccess | You may decide to set all your table access to either \`authenticated\` or \`private\` from within the \`privacy tab\` but then decide to make one or more tables public in this case use the \`allowExternalAccess\` method \`-&gt;beforeQuerying\(\)-&gt;onTable\('news', 'tariffs'\)-&gt;allowExternalAccess\(\)\`  @return $this |
| grantOnlyAdminAccess | You may decide to provide access of some tables to just the admin instead locking them up totally . To do this use the \`grantOnlyAdminAccess\` method.\`-&gt;beforeQuerying\(\)-&gt;onTable\('news', 'tariffs'\)-&gt;grantOnlyAdminAccess\(\)\` @return $this |
| onQuery | Checks if a table is being queried for data then the code attached to it will run. This will run twice before and after the data is being queried.  @return $this |
| onUpdate | Checks if data is being updated on a table then the code attached to it will run.This will run twice before and after the data is being updated.  @return $this |
| onCreate | Checks if data is being added to table then the code attached to it will run. This will run twice before and after the data is being added  @return $this |
| onDelete | Check if data is being deleted from a table. This will run code attached to it before and after its being run  @return $this |
| beforeQuerying | Checks if a table is about to be queried for data .The code attached to it will run before the data is queried  @return $this |
| beforeCreating | Checks if data is about to be added to a table and runs the code attached to it before this happens @return $this |
| beforeUpdating | Checks if table data is about to be updated, and will run the code attached to it before the action is performed  @return $this |
| beforeDeleting | Check if data is about to be deleted from table. Then run the code attached to the action before its being performed. This allows you to perform actions such as \`afterQuering\(\)-&gt;mutateResponseMessage\("the response message has been altered"\)  @return $this |
| afterQuerying | Runs code attached to this method just before the response is being returned to the client. This allows you to perform actions such as \`afterQuering\(\)-&gt;whenever\($rules-&gt;status\_code == 625\)-&gt;mutateResponseMessage\("the response message has been altered"\)  @return $this |
| afterCreating | Runs code attached to this method after the data has been added to the DB. This allows you to perform actions such as \`afterCreating\(\)-&gt;whenever\($rules-&gt;status\_code == 625\)-&gt;mutateResponseMessage\("the response message has been altered"\) @return $this |
| afterUpdating | Run code attached to this method after the data has been updated . This allows you to perform actions such as \`afterUpdating\(\)-&gt;whenever\($rules-&gt;status\_code == 619\)-&gt;mutateResponseMessage\("the response message has been altered"\)  @return $this |
| afterDeleting | Runs code attached to the \`afterDeleting\(\)\` method. This allows you to perform actions such as \`afterDeleting\(\)-&gt;whenever\($rules-&gt;status\_code == 636\)-&gt;mutateResponseMessage\("the response message has been altered"\) @return $this |
| onAnyReequest | Code attached to the \`onAnyRequest\(\)\` will run regardless of whether its a query, create delete or update action  @return $this |
| whenever | This is the equivalence of if and is mostly used together with assertions to alter execution flows. eg beforeQuerying\(\)-&gt;whenever\(assertIts::equal\($input\_name, "edmond"\)\)-&gt;then-&gt;succeedWith\("yes the names are same"\)  @param $assertion @return $this |
| elseWhenever | This is the equivalence of elseif and is mostly used together with assertions to alter execution flows eg.beforeQuerying\(\)-&gt;whenever\(assertIts::equal\($input\_name, "edmond"\)\)-&gt;then-&gt;succeedWith\("yes the names are same"\)-&gt;elseWhenever\(assertIts::equal\($input\_name, "charles"\)\)\)-&gt;then-&gt;succeedWith\("yes the name is charles"\) @param $assertion @return $this |
| otherwise | This is the equivalence of else eg.beforeQuerying\(\)-&gt;whenever\(assertIts::equal\($input\_name, "edmond"\)\)-&gt;then-&gt;succeedWith\("yes the names are same"\)-&gt;elseWhenever\(assertIts::equal\($input\_name, "charles"\)\)\)-&gt;then-&gt;succeedWith\("yes the names are charles"\)-&gt;otherwise\(\)-&gt;succeedWith\("Its some other name"\) @return $this |
| onTable | Checks if the db action to be conducted around one of the tables. Eg beforeQuerying\(\)-&gt;onTable\('register', 'subscription'\)-&gt;succeedWith\("yes"\). In the example above yes will be outputted in case data is being queried from either register or subscription table.  @param string $expectedTableName  @return mixed\|
| succeedWith | Stop execution with an exception and output the message provided. Eg. afterQuering\(\)-&gt;succeedWith\("I will show up after quering"\)  @param null $msg  @return mixed\|
| failWith | Stop execution with an exception and output the message provided. Eg. afterQuering\(\)-&gt;failWith\("I will show up after quering"\). Difference between this and succeedWith is the status code  @param string  $msg  @return mixed\|
| run | DevLess provides developers with ready to use code and one of the ways to access this is via the run statement. After installing an external service you may call it within the rules portion of your app using run eg: beforeCreating\(\)-&gt;run\('businessMath','discount',\[10, $input\_price\]\)-&gt;getResults\($input\_price\) @param $service  @param $method  @param array $params  @return mixed\|
| makeExternalRequest | In the event where you need to make say an api call, the \`makeExternalRequest\` method becomes handy. eg beforeUpdating()->makeExternalRequest('GET', 'https://www.calcatraz.com/calculator/api?c=3%2A3')->storeAs($ans)  ->succeedWith($ans)  @param STRING $method  @param STRING $url @param JSON $data (opt)  @param JSON $headers (optional) @return $this  @param STRING $method  @param STRING $url  @param JSON $data \(opt\)  @param JSON $headers \(optional\)  @return $this |
| getResults | one of the ways to store results from a method with output is by using the \`getResults\` method. This will allow you to the output of a method  @param $input\_var  @return $this |
| storeAs | Get results from the last executed method and assign to a variable. eg: beforeQuerying\(\)-&gt;sumUp\(1.2,3,4,5\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001, 'summed up numbers successfully' $ans\)  @param $input\_var  @return $this |
| assign | Assigns one variable to another just like \`$sum2 = $sum\` . This method works together with \`to\` method to achive this. eg: afterQuering\(\)-&gt;sumUp\(1,2,3,4,5\)-&gt;storeAs\($sum\)-&gt;assign\($sum\)-&gt;to\($sum2\)-&gt;succeedWith\($sum2\)  @param $input\_var  @return $this |
| usingService | Set the name of service from which you want to use method from.  eg: usingService\('devless'\)-&gt;callMethod\('hello'\)-&gt; withParams\(\)-&gt;getResults\($output\)-&gt;succeedWith\($output\)  @param $serviceName  @return $this |
| callMethod | Set the name of the method after setting the service from which you will like to run. eg:usingService\('devless'\)-&gt;callMethod\('hello'\)-&gt; withParams\(\)-&gt;getResults\($output\)-&gt;succeedWith\($output\)  @param $methodName  @return $this |
| withParams | Set parameters for method from which you will like to run eg: usingService\('devless'\)-&gt;callMethod\('getUserProfile'\)-&gt; withParams\(1\)-&gt;getResults\($output\)-&gt;succeedWith\($output\)  @param can have n number of parameters  @return $this |
| withoutParams | Use this in place of \`params\(\)\` in case the service method you want to run has no parameters  eg: usingService\('devless'\)-&gt;callMethod\('hello'\)-&gt;withoutParams\(\)-&gt;getResults\($output\)-&gt;succeedWith\($output\)  @return $this |
| to | \`to\` keyword should be used together with \`assign\` to assign either variables or values to a new variable  @param $output  @return $this |
| assignValues | Behaves just like the \`assign\` method but has a much shorter construct. where you will assign say the string "edmond" to variable $name using \`assign\` \`assign\("edmond"\)-&gt;to\($name\)\` , \`assignValue\("edmond", $name\)\` provides a much shorter construct but loses its redability.  @param $input  @param $output  @return $this |
| stopAndOutput | Should you perform some rules and based on that will like to exit earlier with a response before the actual db command completes, you will want to use \`stopAndOutput\` eg: beforeQuerying\(\)-&gt;usingService\('devless'\)-&gt;callMethod\('getUserProfile'\)-&gt;withParams\(1\)-&gt;storeAs\($profile\)-&gt;stopAndOutput\(1000, 'got profile successfully', $profile\). @param $status\_code  @param $message  @param $payload  @return $this |
| help | List out all methods as well as get docs on specific method eg: -&gt;help\('stopAndOutput'\)  @param $methodToGetDocsFor  @return $this |
| calculate | Perform mathematical operations eg: \`-&gt;beforeQuerying\(\)-&gt;calculate\(3\*5\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @param mathematical expression  @return $this |
| evaluate | Evaluate PHP expressions eg:beforeQuering\(\)-&gt;evaluate\("\DB::table\('users'\)-&gt;get\(\)"\)-&gt;storeAs\($output\)-&gt;stopAndOutput\(1001, 'got users successfully', $output\)  @param $expression  @return $this |
| sumUp | find the sum of numbers. eg: \`-&gt;beforeQuerying\(\)-&gt;sumUp\(3,4,5,6,7\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @param integer $num @return $this |
| subtract | subtract a bunch of numbers. eg: \`-&gt;beforeQuerying\(\)-&gt;subtract\(3,4,5,6,7\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\` also \`-&gt;beforeQuerying\(\)-&gt;from\(5\)-&gt;subtract\(3\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @return $this |
| multiply | find the product of numbers .eg: \`-&gt;beforeQuerying\(\)-&gt;multiply\(3,4,5,6,7\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @return $this  |
| divide | divide a range of numbers.eg: \`-&gt;beforeQuerying\(\)-&gt;divide\(6,2\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @return $this |
| divideBy | This picks results from your earlier computation and divides it by a given number. eg: \`-&gt;beforeQuerying\(\)-&gt;sumUp\(3,4,5,6,7\)-&gt;divdeBy\(6\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @param $number @return $this |
| findSquareRootOf |  Find the square root of a number. eg:\`-&gt;beforeQuerying\(\)-&gt;findSquareRoot\($number\)-&gt;storeAs\($input\_root\)\`  @param integer $num  @return $this |
| getSquareRoot |  Get the squareRoot of the result of the preceeding computation eg: \`-&gt;beforeQuerying\(\)-&gt;divide\(20, 40\)-&gt;getSquareRoot\(\)-&gt;storeAs\($output\)-&gt;succeedWith\($output\)\`  return $this |
| roundUp | round up a number. eg: \`-&gt;beforeCreating\(\)-&gt;roundUp\($input\_milage, 1\)-&gt;storeAs\($input\_milage\)\`  @param integer $number  @param integer $precision  @return $this |
| percentOf |  Find the percent of a number eg: \`-&gt;beforeQuerying\(\)-&gt;find\(10\)-&gt;percentOf\(200\)-&gt;storeAs\($input\_discount\)\`  @param $number  @return $this |
| concatenate | Concatenate strings together eg: \`-&gt;beforeCreating\(\)-&gt;concatenate\("user\_",$input\_name\)\`  @param n number of params @return $this |
| getFirstCharacter | Get first character eg: \`-&gt;beforeCreating\(\)-&gt;getFirstCharacter\("Hello"\)-&gt;storeAs\($first\_char\)-&gt;succeedWith\($first\_char\)\`  @param $string  @return $this |
| getSecondCharacter | Get second character eg: \`-&gt;beforeCreating\(\)-&gt;getSecondCharacter\("Hello"\)-&gt;storeAs\($second\_char\)-&gt;succeedWith\($second\_char\)\`  @param $string  @return $this |
| getThirdCharacter |  Get third character eg: \`-&gt;beforeCreating\(\)-&gt;getThirdCharacter\("Hello"\)-&gt;storeAs\($third\_char\)-&gt;succeedWith\($third\_char\)\` @param $string  @return $this |
| getLastCharacter | Get last character eg: \`-&gt;beforeCreating\(\)-&gt;getLastCharacter\("Hello"\)-&gt;storeAs\($last\_char\)-&gt;succeedWith\($last\_char\)\`  @param $string  @return $this  ||
| getCharacter | Get nth character eg: \`-&gt;beforeCreating\(\)-&gt;getCharacter\(5, "Hello"\)-&gt;storeAs\($nth\_char\)-&gt;succeedWith\($nth\_char\)\`  @param $nth @param $string  @return $this  |
| getLastButOneCharacter | Get last but one character eg: \`-&gt;beforeCreating\(\)-&gt;getLastButOneCharacter\("Hello"\)-&gt;storeAs\($last\_but\_one\_char\)-&gt;succeedWith\($last\_but\_one\_char\)\`  @param $string  @return $this |
| reverseString | Reverse a string eg: -&gt;beforeQuerying\(\)-&gt;assign\("nan"\)-&gt;to\($string\)-&gt;reverseString\(\)-&gt;storeAs\($reverseString\) -&gt;whenever\(assertIts::equal\($string, $reverseString\)\)-&gt;succeedWith\("Its a palindrome :\)"\) -&gt;otherwise\(\)-&gt;failWith\("Its not a palindrome :\("\) @param $string @return $this  |
| findNReplace | replace a string with another eg \`-&gt;beforeCreating\(\)-&gt;findNReplace\("{{name}}", $input\_name, $input\_message\)-&gt;storeAs\($input\_message\)\`  @param $string  @param $replacement  @param $subject  @return $this |
| convertToUpperCase | change string to uppercase eg: \`-&gt;beforeCreating\(\)-&gt;convertToUpperCase\($input\_name\)-&gt;storeAs\($input\_name\)\`  @param $string  @return $this |
| convertToLowerCase | change string to lowercase eg: \`-&gt;beforeCreating\(\)-&gt;convertToLowerCase\($input\_name\)-&gt;storeAs\($input\_name\)\`  @param $string  @return $this |
| truncateString | Truncate a string to some length eg \`-&gt;beforeCreating\(\)-&gt;truncateString\(4, $input\_desc\)-&gt;getResults\($trucatedString\)-&gt;storeAs\($stub\)\`  @param $len @param $string  @param $trimMaker  @return $this |
| countWords | Count the number of words in a sentence eg: \`-&gt;beforeCreating\(\)-&gt;onTable\('users'\)-&gt;countWords\($input\_description\)-&gt;storeAs\($desc\_length\)-&gt;whenever\($desc\_length &lt;= 5\)-&gt;failWith\("Your product description is very short"\)\`  @param $sentence @return $this |
| countCharacters | Find the number of characters in a word or sentence eg: \`-&gt;beforeCreating\(\)-&gt;onTable\('users'\)-&gt;countCharacters\($input\_name\)-&gt;storeAs\($name\_length\)-&gt;whenever\($name\_length &lt;= 0\)-&gt;failWith\("name seems to be empty"\)\`  @param word  @return $this |
| getTimestamp | The \`getTimestamp\` method returns the current timestamp. eg: beforeQuerying\(\)-&gt;getTimestamp\(\)-&gt;storeAs\($timestamp\)-&gt;succeedWith\($timestamp\)  @return $this |
| getCurrentYear | Get the current year using the \`getCurrentYear\` method eg:beforeQuering\(\)-&gt;getCurrentYear\(\)-&gt;storeAs\($currentYear\)succeedWith\($currentYear\)  @return $this |
| getCurrentMonth | Get the current month using the \`getCurrentMonth\` method eg:beforeQuering\(\)-&gt;getCurrentMonth\(\)-&gt;storeAs\($currentMonth\)-&gt;succeedWith\($currentMonth\) @return $this |
| getCurrentDay | Get the current day using the \`getCurrentDay\` method eg:beforeQuering\(\)-&gt;getCurrentDay\(\)-&gt;storeAs\($currentDay\)-&gt;succeedWith\($currentDay\)  @return $this |
| getCurrentHour | Get the current hour using the \`getCurrentHour\` method eg:beforeQuering\(\)-&gt;getCurrentHour\(\)-&gt;storeAs\($currentHour\)-&gt;succeedWith\($currentHour\)  @return $this |
| getFormattedDate | Get the human readable date using the \`getFormattedDate\` method eg: beforeQuering\(\)-&gt;getFormattedDate\(\)-&gt;storeAs\($formattedDate\)-&gt;succeedWith\($formattedDate\) @return $this |
| generateRandomInteger | generates random integers. This may be used for invitation or promotional code generation. eg \`-&gt;beforeCreating\(\)-&gt;generateRandomInteger\(10\)-&gt;storeAs\($promo\_code\)-&gt;assign\($promo\_code\)-&gt;to\($input\_promo\)\`  @param $length  @return $this |
| generateRandomAlphanums | generates random alphanumeric values. This may be used for generating order Ids. eg \`-&gt;beforeCreating\(\)-&gt;generateRandomAlphanums\(\)-&gt;storeAs\($order\_id\)-&gt;assign\($order\_id\)-&gt;to\($input\_order\_id\)\`  @return $this |
| generateRandomString |  generates random string.This generates random string codes. eg \`-&gt;beforeCreating\(\)-&gt;generateRandomInteger\(10\)-&gt;storeAs\($promo\_code\)-&gt;assign\($promo\_code\)-&gt;to\($input\_promo\)\`  @param $length  @return $this |
| generateUniqueId | generates unique Id.This generates unique Id . eg \`-&gt;beforeCreating\(\)-&gt;generateUniqueId\(\)-&gt;storeAs\($user\_id\)-&gt;assign\($user\_id\)-&gt;to\($input\_id\)\`  @param $length @return $this ", |
| mutateStatusCode | mutate response status code. This will change the status code that is being outputed eg: \`-&gt;afterQuering\(\)-&gt;mutateStatusCode\(1111\)\`. NB: you should only change the status code if you know what you doing. Changing it might cause some of the official SDKs to malfunction.  @param $newCode  @return $this |
|mutateResponseMessage| mutate response message. This will change the Message within the response body sent back to the client. eg `->afterQuering()->mutateResponseMessage("new response message")`  @param $newMessage @return $this|
|mutateResponsePayload| mutate response payload. This will Replace the Response payload being sent back to the client eg: `->afterQuering()->mutateResponsePayload(["name"=>"Edmond"])` @param $newPayload  @return $this
| getResponse | get the output response message. This will fetch the Message about to be sent back to the client . eg `->afterQuering()->getResponse($status_code, $message, $payload)` now the variable $status_code, $message, $payload will be available to use within Rules.  @param $status_code @param $message @param $payload @return $this |
| getStatusCode |  get the output status code. This will fetch the status code about to be sent back to the client . eg `->afterQuering()->getStatusCode()->storeAs($status_code)` now the variable $status_code will be available to use within Rules. @param $status_code @return $this |
| getResponseMessage | get the output message. This will fetch the message about to be sent back to the client . eg `->afterQuering()->getResponseMessage()->storeAs($message)` now the variable $message will be available to use within Rules. @param $message @return $this|
| getResponsePayload |   get the output payload. This will fetch the payload about to be sent back to the client . eg `->afterQuering()->getResponsePayload()->storeAs($payload)` now the variable $paylaod will be available to use within Rules.@param $payload @return $this|





## Assertions

Assertions are a special kind of rule, which asserts that some condition holds. In a programming language, these would be functions returning booleans. Assertions are best used together with the conditional functions, such as `whenever`. 

| Assertion | Description |
| :--- | :--- |
| anInteger | check if $value is an integer. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :anInteger\(3\)\) - &gt;then - &gt;stopAndOutput\(1001,'message','its an integer'\)\` @param $value |
| aString | check if $value is a string. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\( assertIts: :aString\( "Hello"\)\) - &gt;then - &gt;stopAndOutput\(1001,'message', 'its a string'\)\` @param $value |
| aBoolean | check if $value is a boolean. eg: \` - &gt;beforeCreating\(\) - &gt; whenever\( assertIts : : aBoolean\(true\)\) - &gt; stopAndOutput\(1001,'message', 'its a boolean'\)\` @param $value |
| aFloat | check if $value is a float. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts : :aFloat\(3.034\)\) - &gt;then - &gt;stopAndOutput\(1001, 'message', 'its a float'\)\` @param $value |
| withinRange | check if $value is within the range $min $max. eg: \`beforeCreating\(\) - &gt;whenever\( assertIts : :withinRange\($input\_value, 1,4\)\) - &gt;then - &gt;stopAndOutput\(1001, 'message', 'its within range'\)\` @param $value @param $min @ param $max |
| upperCase | check if $value is uppercase eg: \` - &gt;beforeCreating\(\) - &gt;whenever\( assertIts: :upperCase\("HELLO"\)\) - &gt;then- &gt;stopAndOutput\(1001, 'message', 'its upper case'\)\`\` @param $value |
| lowerCase | check is $value is lowercase. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :lowerCase\("hello"\)\) - &gt;then- &gt;stopAndOutput\(1001, 'message', 'its lower case'\)\` @param $value |
| alphanumeric | check if $value is alphanumeric. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :\("E23D"\)\)- &gt;stopAndOutput\(1001, 'message', 'its alphanumeric'\)\` @param $value |
| alphabets | check if $value are alphabets eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :alphabet\("abcd"\)\) - &gt;then- &gt;stopAndOutput\(1001, 'message', its alphabets'\)\` @param $value |
| startsWith | check if $value starts with $prefix eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :startsWith\("E23D", "E"\)\)- &gt;then- &gt;stopAndOutput\(1001, 'message', 'it starts with an E'\)\` @param $value @param $prefix |
| endsWith | check if $value ends with $suffix eg: \` - &gt;beforeCreating\(\)- &gt;whenever\(assertIts : :endsWith \("E23D","D"\)\)- &gt;then - &gt;stopAndOutput\(1001,'message', 'it ends with D'\)\` @param $value @param $suffix |
| matchesRegex | check if $value is matched regex eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIt: :matchesRegex\("edmond@devless.io", "email-regex-goes-here"\)\)- &gt;then - &gt;stopAndOutput\(1001,'message', 'it matches the email regex'\)\` @param $value @param $pattern |
| anEmail | check if $value is an email eg: \`beforeCreating\(\)- &gt;whenever\(assertIts: :email\("edmond@devless.io"\)\)- &gt;then- &gt;stopAndOutput\(1001, 'message', 'its an email'\)\` @param $value |
| notEmpty | check if $value is not an empty array or empty string eg: \` - &gt;beforeCreating\(\)- &gt;whenever\(assertIts: :notEmpty\("some text"\)\)- &gt;then -&gt;stopAndOutput\(1001, 'message', 'its not empty'\)\` @param $value |
| contains | check if $value contains $subString  eg:  \`beforeCreating\(\) - &gt;whenever\(assertIts: :contains\( "edmond@devless.io", "edmond"\)\) - &gt;then- &gt;stopAndOutput\( 1001, 'message', 'email contains edmond'\) @param $value @param $substring |
| equal | check if $value equals $value1 eg: \` - &gt;beforeCreating\(\)- &gt;whenever\(assertIts : :equal\("a", "a"\)\)- &gt;then-&gt;stopAndOutput\( 1001,'message', 'a is equal to a : \)'\)\` @param $value @param $value1 |
| notEqual | check if $value is not equal to $value1 eg: \`beforeCreating\(\)- &gt;wheneverassertIts: :notEqual\("a", "b"\)\)- &gt;then-&gt;stopAndOutput\(1001, 'message', 'a is not equal to b'\) @param $value @param $value1 |
| greaterThan | check if $value is greater than $value1 eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :greaterThan\(45, 12\)\)- &gt;then- &gt;stopAndOutput\(1001,'message', '45 is greater than 12'\)\` @param $value @param $value1 |
| lessThan | check if $value is less than $value1 eg:\`- &gt; beforeCreating\(\) - &gt;whenever\(assertIts: :lessThan\(12, 45\)\)- &gt;stopAndOutput\(1001, 'message', '12 is less than 45'\)\` @param $value @param $value1 |
| greaterThanOrEqualTo | check if $value is greater than or equal to $value1 eg: \`- &gt;beforeCreating whenever\(assertIts: :greaterThanOrEqualTo\(45, 45\)\)- &gt;then stopAndOutput\(1001, 'message', '45 is greater than or equal to 45'\)\` @param $value @param $value1 |
| lessThanOrEqualTo | check if $value is less than or equal $value1 \`- &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :lessThanOrEqualTo \(45, 45\)\)- &gt;then- &gt;stopAndOutput\(1001, 'message', '45 is less than or equal to 45'0\` @param $value @param $value1 |
| 
| getAllUsers | Get the list of all registed DevLess users  \`- &gt;beforeCreating\(\) - &gt;run\('devless', 'getAllUsers', []\)- &gt;storeAs($users)- &gt;then- &gt;stopAndOutput\(1000, 'All DevLess Users',$users)` @param $value @param $value1 |
| 
| queryData | Query Data from a service table  ->run('devless', 'queryData', ['service_name', 'table_name'])->storeAs($users)->stopAndOutput(3, '', $users)|
| addData | add Data to a service table  `->run('devless', 'addData', ['service_name', 'table_name', ['field_name'=>'field_value'] ])->storeAs($output)->stopAndOutput(1, 'message', $output)`|
| updateData | update Data from a service table  ->run('devless', 'updateData', ['service_name', 'table_name', 'id', 2 , ['field_name'=>'field_value'] ])->storeAs($output)->stopAndOutput(1, 'message', $output)|
| deleteData | Delete Data from a service table  `->run('devless', 'addData', ['service_name', 'table_name', 2])->storeAs($output)->stopAndOutput(1, 'message', $output)`|
| getUserProfile | Get the profile of a specific user   ->run('devless', 'getUserProfile', [1])->storeAs($output)->stopAndOutput(1, 'message', $output)|
| getUserProfile | Get the profile of a specific user   ->run('devless', 'getUserProfile', [1])->storeAs($output)->stopAndOutput(1, 'message', $output)|
| deleteUserProfile | Delete user profile   ->run('devless', 'deleteUserProfile', [1])->storeAs($output)->stopAndOutput(1, 'message', $output)|
| updateUserProfile | Update user profile   ->run('devless', 'updateUserProfile', ['id', 'email', 'password', 'username', 'phonenumber', 'firstname', 'lastname'], '<email>', '<password>', '<username>', '<phonenumber>', '<firstname>', '<lastname>'])->storeAs($output)->stopAndOutput(1, 'message', $output)|









































































































































































































k
 
Also although PHP variables starts with a `$` prefix you may choose to omit this when working with rules , eg `beforeQuering()->assign(input_name)->to(name)` and `beforeQuering()->assign($input_name)->to($name)` will both work fine with DevLess Rules.
### Database events
### Getting Inputs in Rules
 




## Keywords

Keywords are methods/functions that allow you to control what happens with your data before storing or after reading. This is the full reference:

| Keywords | Description |
| :--- | :--- |
| also | \`also\` filler word like all other filler words does not come with any side effects but is used sorely for readability purposes.  eg: \`-&gt;beforeCreating\(\)-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;convertToUpperCase\(\)-&gt;reverseString\(\)-&gt;storeAs\($text\)-&gt;succeedWith\($text\)\` this can be rewritten as \`beforeCreating\(\)-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;convertToUpperCase\(\)-&gt;also-&gt;reverseString\(\)-&gt;storeAs\($text\)-&gt;succeedWith\($text\)\` notice the introduction of the \`also\` keyword, this does not change the end results but just makes it more readable. @return $this |
| then | \`then\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes. eg: \`-&gt;beforeCreating\(\)-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;convertToUpperCase\(\)-&gt;reverseString\(\)-&gt;storeAs\($text\)-&gt;succeedWith\($text\)\` this can be rewritten as \`beforeCreating\(\)-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;convertToUpperCase\(\)-&gt;then-&gt;reverseString\(\)-&gt;storeAs\($text\)-&gt;succeedWith\($text\)\` notice the introduction of the \`then\` keyword, this does not change the end results but just makes it more readable. @return $this |
| firstly | \`firstly\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`.  @return $this |
| Secondly | \`secondly\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`. @return $this |
| thirdly | \`thirdly\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`. @return $this |
| lastly | \`lastly\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`. @return $this |
| beSureTo | \`beSureTo\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;then-&gt;beSureTo-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`.  @return $this |
| next | \`next\` filler word like all other filler words does not come with any side effects but is used solely for readability purposes eg: \`-&gt;beforeCreating\(\)-&gt;firstly-&gt;assign\("hello"\)-&gt;to\($text\)-&gt;secondly-&gt;convertToUpperCase\(\)-&gt;next-&gt;storeAs\($text\)-&gt;lastly-&gt;succeedWith\($text\)\`.  @return $this |
| authenticateUser | Appending the authenticateUser method to any query action will force users to login to gain access to the system eg: -&gt;beforeQuerying\(\)-&gt;onTable\('subscriptions'\)-&gt;autheticateUser\(\)  @return $this |
| denyExternalAccess | In the event you do not want to grant external access to a table to any users you should use \`denyExternalAccess\` method . \`-&gt;beforeQuerying\(\)-&gt;onTable\('subscriptions'\)-&gt;denyExternalAccess\(\)\` In this case no user will be able to access the \`subscriptions\` table  @return $this |
| allowExternalAccess | You may decide to set all your table access to either \`authenticated\` or \`private\` from within the \`privacy tab\` but then decide to make one or more tables public in this case use the \`allowExternalAccess\` method \`-&gt;beforeQuerying\(\)-&gt;onTable\('news', 'tariffs'\)-&gt;allowExternalAccess\(\)\`  @return $this |
| grantOnlyAdminAccess | You may decide to provide access of some tables to just the admin instead locking them up totally . To do this use the \`grantOnlyAdminAccess\` method.\`-&gt;beforeQuerying\(\)-&gt;onTable\('news', 'tariffs'\)-&gt;grantOnlyAdminAccess\(\)\` @return $this |
| onQuery | Checks if a table is being queried for data then the code attached to it will run. This will run twice before and after the data is being queried.  @return $this |
| onUpdate | Checks if data is being updated on a table then the code attached to it will run.This will run twice before and after the data is being updated.  @return $this |
| onCreate | Checks if data is being added to table then the code attached to it will run. This will run twice before and after the data is being added  @return $this |
| onDelete | Check if data is being deleted from a table. This will run code attached to it before and after its being run  @return $this |
| beforeQuerying | Checks if a table is about to be queried for data .The code attached to it will run before the data is queried  @return $this |
| beforeCreating | Checks if data is about to be added to a table and runs the code attached to it before this happens @return $this |
| beforeUpdating | Checks if table data is about to be updated, and will run the code attached to it before the action is performed  @return $this |
| beforeDeleting | Check if data is about to be deleted from table. Then run the code attached to the action before its being performed. This allows you to perform actions such as \`afterQuering\(\)-&gt;mutateResponseMessage\("the response message has been altered"\)  @return $this |
| afterQuerying | Runs code attached to this method just before the response is being returned to the client. This allows you to perform actions such as \`afterQuering\(\)-&gt;whenever\($rules-&gt;status\_code == 625\)-&gt;mutateResponseMessage\("the response message has been altered"\)  @return $this |
| afterCreating | Runs code attached to this method after the data has been added to the DB. This allows you to perform actions such as \`afterCreating\(\)-&gt;whenever\($rules-&gt;status\_code == 625\)-&gt;mutateResponseMessage\("the response message has been altered"\) @return $this |
| afterUpdating | Run code attached to this method after the data has been updated . This allows you to perform actions such as \`afterUpdating\(\)-&gt;whenever\($rules-&gt;status\_code == 619\)-&gt;mutateResponseMessage\("the response message has been altered"\)  @return $this |
| afterDeleting | Runs code attached to the \`afterDeleting\(\)\` method. This allows you to perform actions such as \`afterDeleting\(\)-&gt;whenever\($rules-&gt;status\_code == 636\)-&gt;mutateResponseMessage\("the response message has been altered"\) @return $this |
| onAnyReequest | Code attached to the \`onAnyRequest\(\)\` will run regardless of whether its a query, create delete or update action  @return $this |
| whenever | This is the equivalence of if and is mostly used together with assertions to alter execution flows. eg beforeQuerying\(\)-&gt;whenever\(assertIts::equal\($input\_name, "edmond"\)\)-&gt;then-&gt;succeedWith\("yes the names are same"\)  @param $assertion @return $this |
| elseWhenever | This is the equivalence of elseif and is mostly used together with assertions to alter execution flows eg.beforeQuerying\(\)-&gt;whenever\(assertIts::equal\($input\_name, "edmond"\)\)-&gt;then-&gt;succeedWith\("yes the names are same"\)-&gt;elseWhenever\(assertIts::equal\($input\_name, "charles"\)\)\)-&gt;then-&gt;succeedWith\("yes the name is charles"\) @param $assertion @return $this |
| otherwise | This is the equivalence of else eg.beforeQuerying\(\)-&gt;whenever\(assertIts::equal\($input\_name, "edmond"\)\)-&gt;then-&gt;succeedWith\("yes the names are same"\)-&gt;elseWhenever\(assertIts::equal\($input\_name, "charles"\)\)\)-&gt;then-&gt;succeedWith\("yes the names are charles"\)-&gt;otherwise\(\)-&gt;succeedWith\("Its some other name"\) @return $this |
| onTable | Checks if the db action to be conducted around one of the tables. Eg beforeQuerying\(\)-&gt;onTable\('register', 'subscription'\)-&gt;succeedWith\("yes"\). In the example above yes will be outputted in case data is being queried from either register or subscription table.  @param string $expectedTableName  @return mixed\|
| succeedWith | Stop execution with an exception and output the message provided. Eg. afterQuering\(\)-&gt;succeedWith\("I will show up after quering"\)  @param null $msg  @return mixed\|
| failWith | Stop execution with an exception and output the message provided. Eg. afterQuering\(\)-&gt;failWith\("I will show up after quering"\). Difference between this and succeedWith is the status code  @param string  $msg  @return mixed\|
| run | DevLess provides developers with ready to use code and one of the ways to access this is via the run statement. After installing an external service you may call it within the rules portion of your app using run eg: beforeCreating\(\)-&gt;run\('businessMath','discount',\[10, $input\_price\]\)-&gt;getResults\($input\_price\) @param $service  @param $method  @param array $params  @return mixed\|
| makeExternalRequest | In the event where you need to make say an api call, the \`makeExternalRequest\` method becomes handy. eg beforeUpdating()->makeExternalRequest('GET', 'https://www.calcatraz.com/calculator/api?c=3%2A3')->storeAs($ans)  ->succeedWith($ans)  @param STRING $method  @param STRING $url @param JSON $data (opt)  @param JSON $headers (optional) @return $this  @param STRING $method  @param STRING $url  @param JSON $data \(opt\)  @param JSON $headers \(optional\)  @return $this |
| getResults | one of the ways to store results from a method with output is by using the \`getResults\` method. This will allow you to the output of a method  @param $input\_var  @return $this |
| storeAs | Get results from the last executed method and assign to a variable. eg: beforeQuerying\(\)-&gt;sumUp\(1.2,3,4,5\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001, 'summed up numbers successfully' $ans\)  @param $input\_var  @return $this |
| assign | Assigns one variable to another just like \`$sum2 = $sum\` . This method works together with \`to\` method to achive this. eg: afterQuering\(\)-&gt;sumUp\(1,2,3,4,5\)-&gt;storeAs\($sum\)-&gt;assign\($sum\)-&gt;to\($sum2\)-&gt;succeedWith\($sum2\)  @param $input\_var  @return $this |
| usingService | Set the name of service from which you want to use method from.  eg: usingService\('devless'\)-&gt;callMethod\('hello'\)-&gt; withParams\(\)-&gt;getResults\($output\)-&gt;succeedWith\($output\)  @param $serviceName  @return $this |
| callMethod | Set the name of the method after setting the service from which you will like to run. eg:usingService\('devless'\)-&gt;callMethod\('hello'\)-&gt; withParams\(\)-&gt;getResults\($output\)-&gt;succeedWith\($output\)  @param $methodName  @return $this |
| withParams | Set parameters for method from which you will like to run eg: usingService\('devless'\)-&gt;callMethod\('getUserProfile'\)-&gt; withParams\(1\)-&gt;getResults\($output\)-&gt;succeedWith\($output\)  @param can have n number of parameters  @return $this |
| withoutParams | Use this in place of \`params\(\)\` in case the service method you want to run has no parameters  eg: usingService\('devless'\)-&gt;callMethod\('hello'\)-&gt;withoutParams\(\)-&gt;getResults\($output\)-&gt;succeedWith\($output\)  @return $this |
| to | \`to\` keyword should be used together with \`assign\` to assign either variables or values to a new variable  @param $output  @return $this |
| assignValues | Behaves just like the \`assign\` method but has a much shorter construct. where you will assign say the string "edmond" to variable $name using \`assign\` \`assign\("edmond"\)-&gt;to\($name\)\` , \`assignValue\("edmond", $name\)\` provides a much shorter construct but loses its redability.  @param $input  @param $output  @return $this |
| stopAndOutput | Should you perform some rules and based on that will like to exit earlier with a response before the actual db command completes, you will want to use \`stopAndOutput\` eg: beforeQuerying\(\)-&gt;usingService\('devless'\)-&gt;callMethod\('getUserProfile'\)-&gt;withParams\(1\)-&gt;storeAs\($profile\)-&gt;stopAndOutput\(1000, 'got profile successfully', $profile\). @param $status\_code  @param $message  @param $payload  @return $this |
| help | List out all methods as well as get docs on specific method eg: -&gt;help\('stopAndOutput'\)  @param $methodToGetDocsFor  @return $this |
| calculate | Perform mathematical operations eg: \`-&gt;beforeQuerying\(\)-&gt;calculate\(3\*5\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @param mathematical expression  @return $this |
| evaluate | Evaluate PHP expressions eg:beforeQuering\(\)-&gt;evaluate\("\DB::table\('users'\)-&gt;get\(\)"\)-&gt;storeAs\($output\)-&gt;stopAndOutput\(1001, 'got users successfully', $output\)  @param $expression  @return $this |
| sumUp | find the sum of numbers. eg: \`-&gt;beforeQuerying\(\)-&gt;sumUp\(3,4,5,6,7\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @param integer $num @return $this |
| subtract | subtract a bunch of numbers. eg: \`-&gt;beforeQuerying\(\)-&gt;subtract\(3,4,5,6,7\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\` also \`-&gt;beforeQuerying\(\)-&gt;from\(5\)-&gt;subtract\(3\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @return $this |
| multiply | find the product of numbers .eg: \`-&gt;beforeQuerying\(\)-&gt;multiply\(3,4,5,6,7\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @return $this  |
| divide | divide a range of numbers.eg: \`-&gt;beforeQuerying\(\)-&gt;divide\(6,2\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @return $this |
| divideBy | This picks results from your earlier computation and divides it by a given number. eg: \`-&gt;beforeQuerying\(\)-&gt;sumUp\(3,4,5,6,7\)-&gt;divdeBy\(6\)-&gt;storeAs\($ans\)-&gt;stopAndOutput\(1001,'got answer successfully', $ans\)\`  @param $number @return $this |
| findSquareRootOf |  Find the square root of a number. eg:\`-&gt;beforeQuerying\(\)-&gt;findSquareRoot\($number\)-&gt;storeAs\($input\_root\)\`  @param integer $num  @return $this |
| getSquareRoot |  Get the squareRoot of the result of the preceeding computation eg: \`-&gt;beforeQuerying\(\)-&gt;divide\(20, 40\)-&gt;getSquareRoot\(\)-&gt;storeAs\($output\)-&gt;succeedWith\($output\)\`  return $this |
| roundUp | round up a number. eg: \`-&gt;beforeCreating\(\)-&gt;roundUp\($input\_milage, 1\)-&gt;storeAs\($input\_milage\)\`  @param integer $number  @param integer $precision  @return $this |
| percentOf |  Find the percent of a number eg: \`-&gt;beforeQuerying\(\)-&gt;find\(10\)-&gt;percentOf\(200\)-&gt;storeAs\($input\_discount\)\`  @param $number  @return $this |
| concatenate | Concatenate strings together eg: \`-&gt;beforeCreating\(\)-&gt;concatenate\("user\_",$input\_name\)\`  @param n number of params @return $this |
| getFirstCharacter | Get first character eg: \`-&gt;beforeCreating\(\)-&gt;getFirstCharacter\("Hello"\)-&gt;storeAs\($first\_char\)-&gt;succeedWith\($first\_char\)\`  @param $string  @return $this |
| getSecondCharacter | Get second character eg: \`-&gt;beforeCreating\(\)-&gt;getSecondCharacter\("Hello"\)-&gt;storeAs\($second\_char\)-&gt;succeedWith\($second\_char\)\`  @param $string  @return $this |
| getThirdCharacter |  Get third character eg: \`-&gt;beforeCreating\(\)-&gt;getThirdCharacter\("Hello"\)-&gt;storeAs\($third\_char\)-&gt;succeedWith\($third\_char\)\` @param $string  @return $this |
| getLastCharacter | Get last character eg: \`-&gt;beforeCreating\(\)-&gt;getLastCharacter\("Hello"\)-&gt;storeAs\($last\_char\)-&gt;succeedWith\($last\_char\)\`  @param $string  @return $this  ||
| getCharacter | Get nth character eg: \`-&gt;beforeCreating\(\)-&gt;getCharacter\(5, "Hello"\)-&gt;storeAs\($nth\_char\)-&gt;succeedWith\($nth\_char\)\`  @param $nth @param $string  @return $this  |
| getLastButOneCharacter | Get last but one character eg: \`-&gt;beforeCreating\(\)-&gt;getLastButOneCharacter\("Hello"\)-&gt;storeAs\($last\_but\_one\_char\)-&gt;succeedWith\($last\_but\_one\_char\)\`  @param $string  @return $this |
| reverseString | Reverse a string eg: -&gt;beforeQuerying\(\)-&gt;assign\("nan"\)-&gt;to\($string\)-&gt;reverseString\(\)-&gt;storeAs\($reverseString\) -&gt;whenever\(assertIts::equal\($string, $reverseString\)\)-&gt;succeedWith\("Its a palindrome :\)"\) -&gt;otherwise\(\)-&gt;failWith\("Its not a palindrome :\("\) @param $string @return $this  |
| findNReplace | replace a string with another eg \`-&gt;beforeCreating\(\)-&gt;findNReplace\("{{name}}", $input\_name, $input\_message\)-&gt;storeAs\($input\_message\)\`  @param $string  @param $replacement  @param $subject  @return $this |
| convertToUpperCase | change string to uppercase eg: \`-&gt;beforeCreating\(\)-&gt;convertToUpperCase\($input\_name\)-&gt;storeAs\($input\_name\)\`  @param $string  @return $this |
| convertToLowerCase | change string to lowercase eg: \`-&gt;beforeCreating\(\)-&gt;convertToLowerCase\($input\_name\)-&gt;storeAs\($input\_name\)\`  @param $string  @return $this |
| truncateString | Truncate a string to some length eg \`-&gt;beforeCreating\(\)-&gt;truncateString\(4, $input\_desc\)-&gt;getResults\($trucatedString\)-&gt;storeAs\($stub\)\`  @param $len @param $string  @param $trimMaker  @return $this |
| countWords | Count the number of words in a sentence eg: \`-&gt;beforeCreating\(\)-&gt;onTable\('users'\)-&gt;countWords\($input\_description\)-&gt;storeAs\($desc\_length\)-&gt;whenever\($desc\_length &lt;= 5\)-&gt;failWith\("Your product description is very short"\)\`  @param $sentence @return $this |
| countCharacters | Find the number of characters in a word or sentence eg: \`-&gt;beforeCreating\(\)-&gt;onTable\('users'\)-&gt;countCharacters\($input\_name\)-&gt;storeAs\($name\_length\)-&gt;whenever\($name\_length &lt;= 0\)-&gt;failWith\("name seems to be empty"\)\`  @param word  @return $this |
| getTimestamp | The \`getTimestamp\` method returns the current timestamp. eg: beforeQuerying\(\)-&gt;getTimestamp\(\)-&gt;storeAs\($timestamp\)-&gt;succeedWith\($timestamp\)  @return $this |
| getCurrentYear | Get the current year using the \`getCurrentYear\` method eg:beforeQuering\(\)-&gt;getCurrentYear\(\)-&gt;storeAs\($currentYear\)succeedWith\($currentYear\)  @return $this |
| getCurrentMonth | Get the current month using the \`getCurrentMonth\` method eg:beforeQuering\(\)-&gt;getCurrentMonth\(\)-&gt;storeAs\($currentMonth\)-&gt;succeedWith\($currentMonth\) @return $this |
| getCurrentDay | Get the current day using the \`getCurrentDay\` method eg:beforeQuering\(\)-&gt;getCurrentDay\(\)-&gt;storeAs\($currentDay\)-&gt;succeedWith\($currentDay\)  @return $this |
| getCurrentHour | Get the current hour using the \`getCurrentHour\` method eg:beforeQuering\(\)-&gt;getCurrentHour\(\)-&gt;storeAs\($currentHour\)-&gt;succeedWith\($currentHour\)  @return $this |
| getFormattedDate | Get the human readable date using the \`getFormattedDate\` method eg: beforeQuering\(\)-&gt;getFormattedDate\(\)-&gt;storeAs\($formattedDate\)-&gt;succeedWith\($formattedDate\) @return $this |
| generateRandomInteger | generates random integers. This may be used for invitation or promotional code generation. eg \`-&gt;beforeCreating\(\)-&gt;generateRandomInteger\(10\)-&gt;storeAs\($promo\_code\)-&gt;assign\($promo\_code\)-&gt;to\($input\_promo\)\`  @param $length  @return $this |
| generateRandomAlphanums | generates random alphanumeric values. This may be used for generating order Ids. eg \`-&gt;beforeCreating\(\)-&gt;generateRandomAlphanums\(\)-&gt;storeAs\($order\_id\)-&gt;assign\($order\_id\)-&gt;to\($input\_order\_id\)\`  @return $this |
| generateRandomString |  generates random string.This generates random string codes. eg \`-&gt;beforeCreating\(\)-&gt;generateRandomInteger\(10\)-&gt;storeAs\($promo\_code\)-&gt;assign\($promo\_code\)-&gt;to\($input\_promo\)\`  @param $length  @return $this |
| generateUniqueId | generates unique Id.This generates unique Id . eg \`-&gt;beforeCreating\(\)-&gt;generateUniqueId\(\)-&gt;storeAs\($user\_id\)-&gt;assign\($user\_id\)-&gt;to\($input\_id\)\`  @param $length @return $this ", |
| mutateStatusCode | mutate response status code. This will change the status code that is being outputed eg: \`-&gt;afterQuering\(\)-&gt;mutateStatusCode\(1111\)\`. NB: you should only change the status code if you know what you doing. Changing it might cause some of the official SDKs to malfunction.  @param $newCode  @return $this |
|mutateResponseMessage| mutate response message. This will change the Message within the response body sent back to the client. eg `->afterQuering()->mutateResponseMessage("new response message")`  @param $newMessage @return $this|
|mutateResponsePayload| mutate response payload. This will Replace the Response payload being sent back to the client eg: `->afterQuering()->mutateResponsePayload(["name"=>"Edmond"])` @param $newPayload  @return $this
| getResponse | get the output response message. This will fetch the Message about to be sent back to the client . eg `->afterQuering()->getResponse($status_code, $message, $payload)` now the variable $status_code, $message, $payload will be available to use within Rules.  @param $status_code @param $message @param $payload @return $this |
| getStatusCode |  get the output status code. This will fetch the status code about to be sent back to the client . eg `->afterQuering()->getStatusCode()->storeAs($status_code)` now the variable $status_code will be available to use within Rules. @param $status_code @return $this |
| getResponseMessage | get the output message. This will fetch the message about to be sent back to the client . eg `->afterQuering()->getResponseMessage()->storeAs($message)` now the variable $message will be available to use within Rules. @param $message @return $this|
| getResponsePayload |   get the output payload. This will fetch the payload about to be sent back to the client . eg `->afterQuering()->getResponsePayload()->storeAs($payload)` now the variable $paylaod will be available to use within Rules.@param $payload @return $this|





## Assertions

Assertions are a special kind of rule, which asserts that some condition holds. In a programming language, these would be functions returning booleans. Assertions are best used together with the conditional functions, such as `whenever`. 

| Assertion | Description |
| :--- | :--- |
| anInteger | check if $value is an integer. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :anInteger\(3\)\) - &gt;then - &gt;stopAndOutput\(1001,'message','its an integer'\)\` @param $value |
| aString | check if $value is a string. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\( assertIts: :aString\( "Hello"\)\) - &gt;then - &gt;stopAndOutput\(1001,'message', 'its a string'\)\` @param $value |
| aBoolean | check if $value is a boolean. eg: \` - &gt;beforeCreating\(\) - &gt; whenever\( assertIts : : aBoolean\(true\)\) - &gt; stopAndOutput\(1001,'message', 'its a boolean'\)\` @param $value |
| aFloat | check if $value is a float. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts : :aFloat\(3.034\)\) - &gt;then - &gt;stopAndOutput\(1001, 'message', 'its a float'\)\` @param $value |
| withinRange | check if $value is within the range $min $max. eg: \`beforeCreating\(\) - &gt;whenever\( assertIts : :withinRange\($input\_value, 1,4\)\) - &gt;then - &gt;stopAndOutput\(1001, 'message', 'its within range'\)\` @param $value @param $min @ param $max |
| upperCase | check if $value is uppercase eg: \` - &gt;beforeCreating\(\) - &gt;whenever\( assertIts: :upperCase\("HELLO"\)\) - &gt;then- &gt;stopAndOutput\(1001, 'message', 'its upper case'\)\`\` @param $value |
| lowerCase | check is $value is lowercase. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :lowerCase\("hello"\)\) - &gt;then- &gt;stopAndOutput\(1001, 'message', 'its lower case'\)\` @param $value |
| alphanumeric | check if $value is alphanumeric. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :\("E23D"\)\)- &gt;stopAndOutput\(1001, 'message', 'its alphanumeric'\)\` @param $value |
| alphabets | check if $value are alphabets eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :alphabet\("abcd"\)\) - &gt;then- &gt;stopAndOutput\(1001, 'message', its alphabets'\)\` @param $value |
| startsWith | check if $value starts with $prefix eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :startsWith\("E23D", "E"\)\)- &gt;then- &gt;stopAndOutput\(1001, 'message', 'it starts with an E'\)\` @param $value @param $prefix |
| endsWith | check if $value ends with $suffix eg: \` - &gt;beforeCreating\(\)- &gt;whenever\(assertIts : :endsWith \("E23D","D"\)\)- &gt;then - &gt;stopAndOutput\(1001,'message', 'it ends with D'\)\` @param $value @param $suffix |
| matchesRegex | check if $value is matched regex eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIt: :matchesRegex\("edmond@devless.io", "email-regex-goes-here"\)\)- &gt;then - &gt;stopAndOutput\(1001,'message', 'it matches the email regex'\)\` @param $value @param $pattern |
| anEmail | check if $value is an email eg: \`beforeCreating\(\)- &gt;whenever\(assertIts: :email\("edmond@devless.io"\)\)- &gt;then- &gt;stopAndOutput\(1001, 'message', 'its an email'\)\` @param $value |
| notEmpty | check if $value is not an empty array or empty string eg: \` - &gt;beforeCreating\(\)- &gt;whenever\(assertIts: :notEmpty\("some text"\)\)- &gt;then -&gt;stopAndOutput\(1001, 'message', 'its not empty'\)\` @param $value |
| contains | check if $value contains $subString  eg:  \`beforeCreating\(\) - &gt;whenever\(assertIts: :contains\( "edmond@devless.io", "edmond"\)\) - &gt;then- &gt;stopAndOutput\( 1001, 'message', 'email contains edmond'\) @param $value @param $substring |
| equal | check if $value equals $value1 eg: \` - &gt;beforeCreating\(\)- &gt;whenever\(assertIts : :equal\("a", "a"\)\)- &gt;then-&gt;stopAndOutput\( 1001,'message', 'a is equal to a : \)'\)\` @param $value @param $value1 |
| notEqual | check if $value is not equal to $value1 eg: \`beforeCreating\(\)- &gt;wheneverassertIts: :notEqual\("a", "b"\)\)- &gt;then-&gt;stopAndOutput\(1001, 'message', 'a is not equal to b'\) @param $value @param $value1 |
| greaterThan | check if $value is greater than $value1 eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :greaterThan\(45, 12\)\)- &gt;then- &gt;stopAndOutput\(1001,'message', '45 is greater than 12'\)\` @param $value @param $value1 |
| lessThan | check if $value is less than $value1 eg:\`- &gt; beforeCreating\(\) - &gt;whenever\(assertIts: :lessThan\(12, 45\)\)- &gt;stopAndOutput\(1001, 'message', '12 is less than 45'\)\` @param $value @param $value1 |
| greaterThanOrEqualTo | check if $value is greater than or equal to $value1 eg: \`- &gt;beforeCreating whenever\(assertIts: :greaterThanOrEqualTo\(45, 45\)\)- &gt;then stopAndOutput\(1001, 'message', '45 is greater than or equal to 45'\)\` @param $value @param $value1 |
| lessThanOrEqualTo | check if $value is less than or equal $value1 \`- &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :lessThanOrEqualTo \(45, 45\)\)- &gt;then- &gt;stopAndOutput\(1001, 'message', '45 is less than or equal to 45'0\` @param $value @param $value1 |
| 
| getAllUsers | Get the list of all registed DevLess users  \`- &gt;beforeCreating\(\) - &gt;run\('devless', 'getAllUsers', []\)- &gt;storeAs($users)- &gt;then- &gt;stopAndOutput\(1000, 'All DevLess Users',$users)` @param $value @param $value1 |
| 
| queryData | Query Data from a service table  ->run('devless', 'queryData', ['service_name', 'table_name'])->storeAs($users)->stopAndOutput(3, '', $users)|
| addData | add Data to a service table  `->run('devless', 'addData', ['service_name', 'table_name', ['field_name'=>'field_value'] ])->storeAs($output)->stopAndOutput(1, 'message', $output)`|
| updateData | update Data from a service table  ->run('devless', 'updateData', ['service_name', 'table_name', 'id', 2 , ['field_name'=>'field_value'] ])->storeAs($output)->stopAndOutput(1, 'message', $output)|
| deleteData | Delete Data from a service table  `->run('devless', 'addData', ['service_name', 'table_name', 2])->storeAs($output)->stopAndOutput(1, 'message', $output)`|
| getUserProfile | Get the profile of a specific user   ->run('devless', 'getUserProfile', [1])->storeAs($output)->stopAndOutput(1, 'message', $output)|
| getUserProfile | Get the profile of a specific user   ->run('devless', 'getUserProfile', [1])->storeAs($output)->stopAndOutput(1, 'message', $output)|
| deleteUserProfile | Delete user profile   ->run('devless', 'deleteUserProfile', [1])->storeAs($output)->stopAndOutput(1, 'message', $output)|
| updateUserProfile | Update user profile   ->run('devless', 'updateUserProfile', ['id', 'email', 'password', 'username', 'phonenumber', 'firstname', 'lastname'], '<email>', '<password>', '<username>', '<phonenumber>', '<firstname>', '<lastname>'])->storeAs($output)->stopAndOutput(1, 'message', $output)|










































































































































































































