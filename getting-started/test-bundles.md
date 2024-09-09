---
icon: computer-classic
---

# Writing Test Bundles

No matter what style you decide to use, you will still end up building a Testing Bundle CFC. This CFC will either contain BDD style suites and specs or xUnit style method tests. These components are simple components with no inheritance that can contain several different life-cycle callback methods. Internally we do the magic of making this CFC inherit all capabilities and methods from our BaseSpec class (`testbox.system.BaseSpec`).

> **Caution** We highly encourage you to use the inheritance approach so you can get introspection, faster execution and ability to execute CFC directly.

```javascript
component{
     // easy right?
}

component extends="testbox.system.BaseSpec"{
     // easy right?
}
```

### Optional Inheritance

At runtime we provide the inheritance via mixins so you don't have to worry about it. However, if you want to declare the inheritance you can do so and this will give you the following benefits:

* Some IDEs will be able to give you introspection for methods and properties
* You will be able to use the HTML runner by executing directly the runRemote method on the CFC Bundle
* Your tests will run faster

```javascript
component extends="testbox.system.BaseSpec"{
}
```

### Injected Variables

At runtime, TestBox will inject several public variables into your testing bundle CFCs to help you with your testing:

* `$mockbox` : A reference to MockBox
* `$assert` : A reference to our Assertions library
* `$utility` : A utility CFC
* `$customMatchers` : A collection of custom matchers registered
* `$exceptionAnnotation` : The annotation used to discover expected exceptions, defaults to expectedException
* `$testID` : A unique identifier for the test bundle
* `$debugBuffer` : The debugging buffer stream

### Injected Methods

At runtime, TestBox will also decorate your bundle CFC with tons of methods to help you in your testing adventures. Depending on your testing style you will leverage some more than others. For the latest methods please visit the [API Docs](http://apidocs.ortussolutions.com/testbox/current) for more information.

```javascript
// quick assertion methods
assert( expression, [message] )
fail(message)

// bdd methods
beforeEach( body )
afterEach( body )
describe( title, body, labels, asyncAll, skip )
xdescribe( title, body, labels, asyncAll )
it( title, body, labels, skip )
xit( title, body, labels )
expect( actual )

// extensions
addMatchers( matchers )
addAssertions( assertions )

// runners
runRemote( testSpecs, testSuites, deubg, reporter );

// utility methods
console( var, top )
debug( var, deepCopy, label, top )
clearDebugBuffer()
getDebugBuffer()
print( message )
printLn( message )

// mocking methods
makePublic( target, method, newName )
querySim( queryData )
getmockBox( generationPath )
createEmptyMock( className, object, callLogging )
createMock( className, object, clearMethods, callLogging )
prepareMock( object, callLogging )
createStub( callLogging, extends, implements )
getProperty( target, name, scope, defaultValue )
```
