---
icon: computer-classic
---

# Writing Test Bundles

No matter what style you decide to use, you will still end up building a Testing Bundle Class. This class will either contain BDD style suites and specs or xUnit style method tests.  You can create as many as you need and group them as necessary in different folders (packages).

The class can extend our base class: `testbox.system.BaseSpec` or not.  If you do, then the tests will be faster and you will get IDE introspection.  However, TestBox doesn't enforce the inheritance.

{% tabs %}
{% tab title="BoxLang" %}
```java
class{
     // easy right?
}

class extends="testbox.system.BaseSpec"{
     // easy right?
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
component{
     // easy right?
}

component extends="testbox.system.BaseSpec"{
     // easy right?
}
```
{% endtab %}
{% endtabs %}

The CLI can assist you when creating new bundles:

```bash
# Create a BDD Bundle
testbox create bdd --help
# Create an xUnit Bundle
testbox create unit --help

# Examples
testbox create bdd CalculatorTest --open
testbox create unit name=SecurityTest directory="test/specs/unit/"
```

## Optional Inheritance

At runtime we provide the inheritance via mixins so you don't have to worry about it. However, if you want to declare the inheritance you can do so and this will give you the following benefits:

* Some IDEs will be able to give you introspection for methods and properties
* You will be able to use the HTML runner by executing directly the runRemote method on the CFC Bundle
* Your tests will run faster

## Location

Where do you place your test bundles?  Great question.  We recommend the following location for your projects: `(tests/specs)`

```
/tests
  + /specs
```

This is also what the TestBox CLI commands use as a convention.  To make things easier you can use the CommandBox commands to generate a test harness for you:

```bash
testbox generate harness
```

This will generate your testing harness:

* `/tests` - The test harness
  * `/resources` - Where you can place any kind of testing helper, data, etc
  * `/specs` - Where your test bundles can go
  * `Application.bx|cfc` - Your test harness applilcation file
  * `runner.bx|cfm` - A runner template that executes ALL your tests with tons of different options.  You can create as many as you like.
  * `test.xml` - An ANT task to do JUNIT testing.

## Injected Variables

At runtime, TestBox will inject several public variables into your testing bundle to help you with your testing.&#x20;

* `$mockbox` : A reference to MockBox
* `$assert` : A reference to our Assertions library
* `$utility` : A utility CFC
* `$customMatchers` : A collection of custom matchers registered
* `$exceptionAnnotation` : The annotation used to discover expected exceptions, defaults to expectedException
* `$testID` : A unique identifier for the test bundle
* `$debugBuffer` : The debugging buffer stream

## Injected/Inherited Methods

Wether you inherit or not, your bundles will have a plethora of methods that will help you in your testing adeventure.  Here is a link to the API Docs for the `BaseSpec` class:

{% embed url="https://s3.amazonaws.com/apidocs.ortussolutions.com/testbox/current/index.html?testbox/system/BaseSpec" %}
[https://s3.amazonaws.com/apidocs.ortussolutions.com/testbox/current/index.html?testbox/system/BaseSpec](https://s3.amazonaws.com/apidocs.ortussolutions.com/testbox/current/index.html?testbox/system/BaseSpec)
{% endembed %}

### Quick Assertion Methods

The BaseSpec has a few shortcut methods for quick assertions.  Please look at the [Assertions](../digging-deeper/assertions/) and [Expectations](../digging-deeper/expectations/) library for more in-depth usage.

```javascript
// Assert that the expression is truthy
assert( expression, [message=""] )
// Start an expectation with an actual value, returns the Expectation object
expect( actual ) : Expectation
// Fail now!
fail( message )
```

### Extension Methods

These methods allow you to extend TestBox with custom [assertions](../digging-deeper/assertions/custom-assertions.md) and expectation [matchers](../digging-deeper/expectations/custom-matchers.md).

```java
// extensions
addMatchers( struct|path matchers )
addAssertions( struct|path assertions )
```

### Environment Methods

These methods assist you with identifying environment conditions.

```java
// Which language/engine are you running on
isAdobe()
isLucee
isBoxLang()

// What OS are we on
isLinux()
isMac()
isWindows()

// Get the Environment Class
getEnv()
```

### Java Environment

You can use the `getEnv()` to get access to our Environment utility object.  From there you can use the following methods:

```java
// Get a java property or environment setting or a default value
getSystemSetting( required key, [defaultValue] )

// Get a java system property ONLY
getSystemProperty( required key, [defaultValue] )

// Get a java environment setting ONLY
getEnv( required key, [defaultValue] )

// Get the java.lang.System
getJavaSystem()
```

### Utility Methods

These methods are here to help you during the testing process.  Every bundle also get's a debug buffer attached to its running results.  This is where you as the developer can send pretty much any variable (simple or complex) and attach it to the debug buffer. This will then be presented acordingly in the test reporters or facilities.

```java
// Send variables to the output console
console(any var, [numeric top='9999'], [boolean showUDFs='false'], [string label=''])
// Send data to the TestBox debug buffer
debug([any var], [string label=''], [boolean deepCopy='false'], [numeric top='999'], [boolean showUDFs='false'])
// Clear the buffer
clearDebugBuffer()
// Get the debug buffer
getDebugBuffer()

// Writes to the Output Buffer (CLI, Browser)
// Remember that if your test runs in the CLI, the buffer is the console
// If you are in a webserver, the buffer is the browser output
print( message )
printLn( message )
```

### Mocking Methods

TestBox is bundles with two amazing mocking facilities:

* **MockBox** - Mocks objects and stubs
* **MockDataCFC** - Allows you to mock data and JSON data - [https://www.forgebox.io/view/mockdatacfc](https://www.forgebox.io/view/mockdatacfc)

```java
createMock([string className], [any object], [boolean clearMethods='false'])
createEmptyMock([string className], [any object], [boolean callLogging='true'])
prepareMock([any object], [boolean callLogging='true'])
createStub([boolean callLogging='true'], [string extends=''], [string implements=''])

// Create a mock query of data
querySim( queryData )
// Call the `mockData()` method on the MockDataCFC
mockData( arguments )

// Make a private/package method on a class public
makePublic(any target, string method, [string newName=''])
// Get the value of a private property on a class
getProperty( target, name, scope, defaultValue )

// Get the MockBox class
getMockBox( [string generationPath=''] )
```

### BDD Methods

These methods are used to create the BDD style of testing.  You will discover them more in the [BDD Tests](testbox-bdd-primer/) section.

```java
// Life-cycle methods
afterEach(any body, [struct data='[runtime expression]'])
aroundEach(any body, [struct data='[runtime expression]'])
beforeEach(any body, [struct data='[runtime expression]'])

// Suite/Grouping Methods
describe(string title, any body, [any labels='[runtime expression]'], [boolean asyncAll='false'], [any skip='false'], [boolean focused='false'])
feature(string title, any body, [any labels='[runtime expression]'], [boolean asyncAll='false'], [any skip='false'], [boolean focused='false'])
story(string title, any body, [any labels='[runtime expression]'], [boolean asyncAll='false'], [any skip='false'], [boolean focused='false'])
given(string title, any body, [any labels='[runtime expression]'], [boolean asyncAll='false'], [any skip='false'], [boolean focused='false'])
when(string title, any body, [any labels='[runtime expression]'], [boolean asyncAll='false'], [any skip='false'], [boolean focused='false'])

// skip the suite
xdescribe()
xfeature()
xgiven()
xstory()
xwhen()

// focus the suite
fdescribe()
ffeature()
fgiven()
fstory()
fwhen()


// Specs/Testing Methods
it(string title, any body, [any labels='[runtime expression]'], [any skip='false'], [struct data='[runtime expression]'], [boolean focused='false'])
then(string title, any body, [any labels='[runtime expression]'], [any skip='false'], [struct data='[runtime expression]'], [boolean focused='false'])
test(string title, any body, [any labels='[runtime expression]'], [any skip='false'], [struct data='[runtime expression]'], [boolean focused='false'])

// Skip the tests
xit()
xthen()
xtest()

// Focus the tests
fit()
fthen()
ftest()

// Start an expectation expression
expect( actual )
```
