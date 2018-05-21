# Advanced Concepts

The HTML Kit provides developers with easy ways to work with their data. It also provides more advanced functionalities.

## Advanced Queries

Adding the class `dv-get-all:inventory:products` will get you data from the table `products` under the service `inventory` . This is well explained [here ]() . Its possible to craft more advanced queries using the query parameters below:

Lets assume we have an `inventory` service which has a table `products` from which a query is being made

| Query Keyword | Usage | Explanation |
| :--- | :--- | :--- |
| size | class="dv-param:size:1:get-all:inventory:products " | The size  parameter makes it possible to get a limited number of records |
| where | class="dv-param:where:id^2:get-all:inventory:products " | The where  parameter  allows for querying data where a key contains a specific value. In this case where the id is 2. |
| orWhere | class="dv-param:where:id^1-param:orWhere:price^1:get-all:inventory:products " | The orWhere parameter  is mostly used together with the where parameter. This allows one to query for data where the column contains a value orWhere it another column matches a certain value |
| greaterThan | class="dv-param:greaterThan:id^1:get-all:inventory:products | The greaterThan parameter  will get all records where the key is greater than the given value. In this case all products with ids greater than 1 |
| lessThan | class="dv-param:lessThan:id^2:get-all:inventory:products" | The lessThan parameter  will get all records where the key is greater than the given value. In this case all products with ids less than 2 |
| offset | class="dv-param:offset:2-param:size:1:get-all:inventory:products" | An offset of 2 will ignore the first two records in the database , returning data from the third record in the database. Combining the offset with size gives  pagination |
| search | class="dv-param:search:name^shoes:get-all:inventory:products" | The search parameter allows to search a table column that contains a keyword. The match may be an exact one or a partial match |
| randomize | class="dv-param:randomize:1-get-all:inventory:products" | This parameter allows records to  be populated in a more random form |
| orderBy | class="dv-param:orderBy:price-get-all:inventory:products dv-label:front\_page" | This allows for records to be ordered in descending order based on a particular field |

**NB:** Its possible to pair up multiple parameters for a query eg: `class="dv-param:search:name^shoes-param:randomize:1-get-all:inventory:products` This will return results containing the name shoes in a random order. Also parameter values may be passed in via URL query strings . eg `class="dv-param:search:searchQuery-get-all:inventory:products"` to make this more dynamic you may pass the searchQuery via the URL using a query string eg: `mydevless.herokuapp.com/?searchQuery=name^shoes` this will replace `searchQuery` with `name^shoes`

### Intercepting  Queried Data

Getting the records from a table and rendering it is straight forward. Sometimes you might want to pre-process this data before rendering. To do this you will have to intercept the query using the `dvInterceptQueryResponse` hook and Javascript .

Eg: Lets say we will like to capi talize every name before rendering to screen

```markup
<div class="dv-get-all:inventory:products dv-label:front_page">
    <span class="var-name"></span>
    <span class="var-price"></span>
    <br>
</div>

<script type="text/javascript">
    dvInterceptQueryResponse = function(resp) {
        for( each in resp.front_page) {
            resp.front_page[each].name = resp.front_page[each].name.toLocaleUpperCase();
        }
        return resp;
    }
</script>
```

The `dv-label` class serves as an identifier for the query data action and makes it possible to access the record from within JavaScript

### Intercepting submitted Data

```markup
<div class="dv-notify"></div>
<form class="dv-add-oneto:inventory:products dv-label:admin">
    <input type="text" name="name"><br>
    <input type="number" name="price"><br>
    <button type="submit">add</button>
</form>

<script type="text/javascript">
    dvInterceptSubmission = function(resp) {
            resp.admin.name = resp.admin.name.toLocaleUpperCase();
        return resp;
    }

</script>
```

The example above describe the interception of data about to be submitted to DevLess and capitalizing the name before it finally gets to DevLess.

