| KEYWORD | DESCRIPTION |
| :--- | :--- |
| anInteger | check if $value is an integer. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts: :anInteger\(3\)\) - &gt;then - &gt;stopAndOutput\(1001,'message','its an integer'\)\` @param $value |
| aString | check if $value is a string. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\( assertIts: :aString\( "Hello"\)\) - &gt;then - &gt;stopAndOutput\(1001,'message', 'its a string'\)\` @param $value |
| aBoolean | check if $value is a boolean. eg: \` - &gt;beforeCreating\(\) - &gt; whenever\( assertIts : : aBoolean\(true\)\) - &gt; stopAndOutput\(1001,'message', 'its a boolean'\)\` @param $value |
| aFloat | check if $value is a float. eg: \` - &gt;beforeCreating\(\) - &gt;whenever\(assertIts : :aFloat\(3.034\)\) - &gt;then - &gt;stopAndOutput\(1001, 'message', 'its a float'\)\` @param $value |
| withinRange | check if $value is within the rane $min $max. eg: \`beforeCreating\(\) - &gt;whenever\( assertIts : :withinRange\($input\_value, 1,4\)\) - &gt;then - &gt;stopAndOutput\(1001, 'message', 'its within range'\)\` @param $value @param $min @ param $max |
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
|  |  |



