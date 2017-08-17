#  Data model
DevLess is a plug-and-play back-end that can do many things, but one of the core components is the ability to model and persist data. DevLess uses the relational model for data. This is the same model as any relational database such as mysql or postgresql. 

DevLess uses a relational database as it's backing database and can expose as much as you want to clients. Controlling what a user is allowed to access or write can be done by using the rules engine. The same is true for manipulation of data before writing or after reading. 

## Field Types

When creating fields in DevLess, you can chose what field type that field has. The field type is used for both determining efficient storing, as well as for validation. E.g. the `email` type is stored in the same way as `text`, but is also validated.

| DevLess field type | Stored as | Use for | 
| :-- | :-- | :--- | 
| `text`  |  `string` | Shorter strings/text, e.g. names | 
| `textarea`  |  `longText` | Longer texts, e.g. comments or blog posts |
| `integer`  |  `integer` | Integers (whole numbers) | 
| `decimals`  |  `double` | Decimal numbers |
| `password`  |  `string` | Password. Will be securely stored using a cryptographic hash. Validated to be alphanumeric. | 
| `percentage`  |  `integer` | Percentage values. |
| `url`  |  `string` | URLs. Will be validated as URLs. | 
| `timestamp`  |  `timestamp` | Points in time. | 
| `boolean`  |  `boolean` | Boolean (`true` or `false`) |
| `email`  |  `string` | Emails. Validated to be a valid email format | 
| `reference`  |  `integer` | A reference to another table. At field creation, the referred field is specified. The reference is to the generated `id` columm of that field. | 
| `base64`  | `base64` | Binary data, for example files and images. | 

## Table relationships 

When using the `reference` field types, you are creating relations to some specific `row` in another table. The table you refer to is specified at field creation. When inserting data in this field, you have to keep track of the `id` of the row you want to refer to in the referred table. 

Check out [this video](https://www.youtube.com/watch?v=ih63gDK_6EA) for a deeper dive into linking up data using relationships.