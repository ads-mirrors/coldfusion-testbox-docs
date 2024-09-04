---
icon: octagon-check
---

# Assertions

TestBox supports the concept of [assertions](http://en.wikipedia.org/wiki/Assertion\_\(software\_development\)) to allow for validations and for legacy tests. We encourage developers to use our BDD expectations as they are more readable and fun to use (Yes, fun I said!).

The assertions are modeled in the class `testbox.system.Assertion`, so you can visit the [API](http://apidocs.ortussolutions.com/testbox/current/?testbox/system/Assertion.html) for the latest assertions available. Each test bundle will receive a variable called `$assert` which represents the assertions object. Here are some common assertion methods:

```javascript
assert( expression, [message] )
between( actual, min, max, [message] )
closeTo(expected, actual, delta, [datePart], [message])
deepKey( target, key, [message] )
fail( [message] )
includes( target, needle, [message] )
includesWithCase( target, needle, [message] )
instanceOf( actual, typeName, [message] )
isEmpty( target, [message] )
isEqual(expected, actual, [message])
isEqualWithCase(expected, actual, [message])
isFalse( actual, [message] )
isGT( actual, target, [message])
isGTE( actual, target, [message])
isLT( actual, target, [message])
isLTE( actual, target, [message])
isNotEmpty( target, [message] )
isNotEqual(expected, actual, [message])
isTrue( actual, [message] )
key( target, key, [message] )
lengthOf( target, length, [message] )
match( actual, regex, [message] )
matchWithCase( actual, regex, [message] )
notDeepKey( target, key, [message] )
notIncludes( target, needle, [message] )
notIncludesWithCase( target, needle, [message] )
notInstanceOf( actual, typeName, [message] )
notKey( target, key, [message] )
notLengthOf( target, length, [message] )
notMatch( actual, regex, [message] )
notNull( actual, [message] )
notThrows(target, [type], [regex], [message])
notTypeOf( type, actual, [message] )
null( actual, [message] )
skip( message, detail )
throws(target, [type], [regex], [message])
typeOf( type, actual, [message] )
```
