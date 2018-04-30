# Query Parameters

Query Keyword

| Usage | Arguments | Explanation |
| :--- | :--- | :--- |
| size | @param **integer:** key eg 3  | The size  parameter makes it possible to get a limited number of records |
| where | @param **string:** key eg id             @param **mixed:** value eg 2   | The where  parameter  allows for querying data where a key contains a specific value. In this case where the id is 2. |
| orWhere | @param **string:** key eg id             @param **mixed:** value eg 2 | The orWhere parameter  is mostly used together with the where parameter. This allows one to query for data where the column contains a value orWhere another column matches a certain value |
| greaterThan | @param **string:** key eg id             @param **mixed:** value eg 1 | The greaterThan parameter  will get all records where the key is greater than the given value. In this case all products with ids greater than 1 |
| lessThan | @param **string:** key eg id             @param **mixed:** value eg 2 | The lessThan parameter  will get all records where the key is greater than the given value. In this case all products with ids less than 2 |
| offset | @param **integer:** key eg 2              | An offset of 2 will ignore the first two records in the database , returning data from the third record in the database. Combining the offset with size gives  pagination |
| search | @param **string:** key eg name             @param **mixed:** value eg mark | The search parameter allows to search a table column that contains a keyword. The match may be an exact one or a partial match |
| randomize | @param **any: **polar eg 1 | This parameter allows records to  be populated in a more random form |
| orderBy | @param **string: **key age | This allows for records to be ordered in descending order based on a particular field |
| asc | @param **mixed: **key age | Orders records in ascending order based on the age field  |
| desc | @param **mixed: **key age | Orders records in descending  order based on the age field |



