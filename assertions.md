
##Assertions 

```

Assertion::alnum($value);

Assertion::between($value, $lowerLimit, $upperLimit);

Assertion::betweenExclusive($value, $lowerLimit, $upperLimit);

Assertion::betweenLength($value, $minLength, $maxLength);

Assertion::boolean($value);

Assertion::choice($value, $choices);

Assertion::choicesNotEmpty($values, $choices);

Assertion::classExists($value);

Assertion::contains($string, $needle);

Assertion::count($countable, $count);

Assertion::date($value, $format);

Assertion::digit($value);

Assertion::directory($value);

Assertion::e164($value);

Assertion::email($value);

Assertion::endsWith($string, $needle);

Assertion::eq($value, $value2);

Assertion::false($value);

Assertion::file($value);

Assertion::float($value);

Assertion::greaterOrEqualThan($value, $limit);

Assertion::greaterThan($value, $limit);

Assertion::implementsInterface($class, $interfaceName);

Assertion::inArray($value, $choices);

Assertion::integer($value);

Assertion::integerish($value);

Assertion::interfaceExists($value);

Assertion::ip($value, $flag = null);

Assertion::ipv4($value, $flag = null);

Assertion::ipv6($value, $flag = null);

Assertion::isArray($value);

Assertion::isArrayAccessible($value);

Assertion::isCallable($value);

Assertion::isInstanceOf($value, $className);

Assertion::isJsonString($value);

Assertion::isObject($value);

Assertion::isTraversable($value);

Assertion::keyExists($value, $key);

Assertion::keyIsset($value, $key);

Assertion::keyNotExists($value, $key);

Assertion::length($value, $length);

Assertion::lessOrEqualThan($value, $limit);

Assertion::lessThan($value, $limit);

Assertion::max($value, $maxValue);

Assertion::maxLength($value, $maxLength);

Assertion::methodExists($value, $object);

Assertion::min($value, $minValue);

Assertion::minLength($value, $minLength);

Assertion::noContent($value);

Assertion::notBlank($value);

Assertion::notEmpty($value);

Assertion::notEmptyKey($value, $key);

Assertion::notEq($value1, $value2);

Assertion::notInArray($value, $choices);

Assertion::notIsInstanceOf($value, $className);

Assertion::notNull($value);

Assertion::notSame($value1, $value2);

Assertion::null($value);

Assertion::numeric($value);

Assertion::range($value, $minValue, $maxValue);

Assertion::readable($value);

Assertion::regex($value, $pattern);

Assertion::same($value, $value2);

Assertion::satisfy($value, $callback);

Assertion::scalar($value);

Assertion::startsWith($string, $needle);

Assertion::string($value);

Assertion::subclassOf($value, $className);

Assertion::true($value);

Assertion::url($value);

Assertion::uuid($value);

Assertion::writeable($value);

```