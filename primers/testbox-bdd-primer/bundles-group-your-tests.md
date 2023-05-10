# Bundles: Group Your Tests

TestBox relies on the fact of creating **testing bundles** which are basically CFCs. A bundle CFC will hold all the **suites** and **specs** a TestBox runner will execute and produce reports on.  Don't worry, we will cover what's a suite and a spec as well.

{% code title="tests/specs/MySpec.cfc" %}
```javascript
component extends="testbox.system.BaseSpec"{

     // executes before all suites
     function beforeAll(){}

     // executes after all suites
     function afterAll(){}

     // All suites go in here
     function run( testResults, testBox ){

     }

}
```
{% endcode %}

This bundle CFC can contain 2 life-cycle functions and a single `run()` function where you will be writing your test **suites** and **specs.**&#x20;

### Life-Cycle Methods

The `beforeAll()` and `afterAll()` methods are called life-cycle methods. They will execute once before the `run()` function and once after the `run()` function. This is a great way to do any kind of global setup or tear down in your tests.

### Execution

The `run()` function receives the TestBox `testResults` object as a reference and `testbox` as a reference as well. This way you can have metadata and access to what will be reported to users in a reporter. You can also use it to decorate the results or store much more information that can be picked up later by reports. You also have access to the `testbox` class so you can see how the test is supposed to execute, what labels was it passed, directories, options, etc.
