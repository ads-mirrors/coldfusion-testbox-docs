---
icon: magnifying-glass-play
description: Test All Things!
---

# Running Tests

TestBox tests can be run from different sources from what we call **Runners.**  These can be from different sources:

* CLI
  * TestBox CLI
  * NodeJS
* Web Server
  * Runner
  * TestBundle Directly
* Custom

Your test harness already includes the web runner: `runner.bx or runner.cfm`.  You can execute that directly in your browser to get the results or run it via the CLI: `testbox run`.  We invite you to explore the different runners available to you.

{% content-ref url="cli-runner.md" %}
[cli-runner.md](cli-runner.md)
{% endcontent-ref %}

{% content-ref url="test-runner.md" %}
[test-runner.md](test-runner.md)
{% endcontent-ref %}

{% content-ref url="bundle-s-runner.md" %}
[bundle-s-runner.md](bundle-s-runner.md)
{% endcontent-ref %}

{% content-ref url="directory-runner.md" %}
[directory-runner.md](directory-runner.md)
{% endcontent-ref %}

{% content-ref url="ant-runner.md" %}
[ant-runner.md](ant-runner.md)
{% endcontent-ref %}

{% content-ref url="nodejs-runner.md" %}
[nodejs-runner.md](nodejs-runner.md)
{% endcontent-ref %}

{% content-ref url="global-runner.md" %}
[global-runner.md](global-runner.md)
{% endcontent-ref %}

{% content-ref url="test-browser.md" %}
[test-browser.md](test-browser.md)
{% endcontent-ref %}

### Custom Runners

However, you can create your own custom runners as long as you instantiate the `TestBox` class and execute one of it's runnable methods.  The main execution methods are:

```javascript
// Run tests and produce reporter results
testbox.run()

// Run tests and get raw testbox.system.TestResults object
testbox.runRaw()

// Run tests and produce reporter results from SOAP, REST, HTTP
testbox.runRemote()

// Run via Spec URL
http://localhost/tests/spec.cfc?method=runRemote

// Via CommandBox
testbox run
```

{% hint style="info" %}
We encourage you to read the [API docs](http://apidocs.ortussolutions.com/testbox/current) included in the distribution for the complete parameters for each method.
{% endhint %}

## `run()` Arguments

Here are the arguments you can use for initializing TestBox or executing the `run()` method

| Argument     | Required | Default | Type                      | Description                                                                                                                                                                                                                                                                                     |
| ------------ | -------- | ------- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bundles      | true     | ---     | string/array              | The path, list of paths or array of paths of the spec bundle CFCs to run and test                                                                                                                                                                                                               |
| directory    | false    | ---     | struct                    | The directory mapping path or a struct: \[ mapping = the path to the directory using dot notation (myapp.testing.specs), recurse = boolean, filter = closure that receives the path of the CFC found, it must return true to process or false to continue process ]                             |
| reporter     | false    | simple  | string/struct/instance    | The type of reporter to use for the results, by default is uses our 'simple' report. You can pass in a core reporter string type or an instance of a coldbox.system.reports.IReporter. You can also pass a struct with \[type="string or classpath", options={}] if a reporter expects options. |
| labels       | false    | false   | string/array              | The string or array of labels the runner will use to execute suites and specs with.                                                                                                                                                                                                             |
| options      | false    | {}      | struct                    | A structure of property name-value pairs that each runner can implement and use at its discretion.                                                                                                                                                                                              |
| testBundles  | false    | ---     | string/array              | A list or array of bundle names that are the ones that will be executed ONLY!                                                                                                                                                                                                                   |
| testSuites   | false    | ---     | string/array              | A list or array of suite names that are the ones that will be executed ONLY!                                                                                                                                                                                                                    |
| testSpecs    | false    | ---     | string/array              | A list or array of test names that are the ones that will be executed ONLY                                                                                                                                                                                                                      |
| callbacks    | false    | `{}`    | struct of closures or CFC | A struct of listener callbacks or a CFC with callbacks for listening to progress of the testing: `onBundleStart,onBundleEnd,onSuiteStart,onSuiteEnd,onSpecStart,onSpecEnd`                                                                                                                      |
| eagerFailure | false    | false   | boolean                   | If true, then after testing a bundle if there are any failures or errors no more testing will be performed.                                                                                                                                                                                     |

## `runRemote()` Arguments

Here are the arguments you can use for executing the `runRemote()` method of the TestBox object:

| Argument        | Required | Default | Type         | Description                                                                                                                                                              |
| --------------- | -------- | ------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| bundles         | true     | ---     | string       | The path, list of paths or array of paths of the spec bundle CFCs to run and test                                                                                        |
| directory       | false    | ---     | string       | The directory mapping to test: directory = the path to the directory using dot notation (myapp.testing.specs)                                                            |
| recurse         | false    | true    | boolean      | Recurse the directory mapping or not, by default it does                                                                                                                 |
| reporter        | false    | simple  | string/path  | The type of reporter to use for the results, by default is uses our 'simple' report. You can pass in a core reporter string type or a class path to the reporter to use. |
| reporterOptions | false    | {}      | JSON         | A JSON struct literal of options to pass into the reporter                                                                                                               |
| labels          | false    | false   | string       | The string array of labels the runner will use to execute suites and specs with.                                                                                         |
| options         | false    | {}      | JSON         | A JSON struct literal of configuration options that are optionally used to configure a runner.                                                                           |
| testBundles     | false    | ---     | string/array | A list or array of bundle names that are the ones that will be executed ONLY!                                                                                            |
| testSuites      | false    | ---     | string       | A list of suite names that are the ones that will be executed ONLY!                                                                                                      |
| testSpecs       | false    | ---     | string       | A list of test names that are the ones that will be executed ONLY                                                                                                        |
| eagerFailure    | false    | false   | boolean      | If true, then after testing a bundle if there are any failures or errors no more testing will be performed.                                                              |

### Notes

* The `bundles` argument which can be a single CFC path or an array of CFC paths or a directory argument so it can go and discover the test bundles from that directory.&#x20;
* The `reporter` argument can be a core reporter name like: json,xml,junit,raw,simple,dots,tap,min,etc or it can be an instance of a reporter CFC.&#x20;
* You can execute the runners from any cfm template or any CFC or any URL, that is up to you.
