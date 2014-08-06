# mocha-webdriver
[![NPM version](https://badge.fury.io/js/mocha-webdriver.png)](http://badge.fury.io/js/grunt-mocha-webdriver) [![Build Status](https://travis-ci.org/jmreidy/grunt-mocha-webdriver.svg?branch=master)](https://travis-ci.org/jmreidy/grunt-mocha-webdriver) [![david-dm-status-badge](https://david-dm.org/jmreidy/grunt-mocha-webdriver.png)](https://david-dm.org/jmreidy/grunt-mocha-webdriver#info=dependencies&view=table)
 [![david-dm-status-badge](https://david-dm.org/jmreidy/mocha-webdriver/dev-status.png)](https://david-dm.org/jmreidy/grunt-mocha-webdriver#info=devDependencies&view=table)
> A test harness for running Mocha-based functional tests against
a Webdriver-enabled source: specifically, PhantomJS for local testing and Sauce Labs
for comprehensive cross-browser testing.

This code was extracted from jmreidy/grunt-mocha-webdriver, so that it could be
used independently of Grunt.

##Getting Started
Connecting to Sauce Labs requires java.

Install the plugin with:

```shell
npm install mocha-webdriver --save-dev
```

###Using mocha-webdriver with Phantomjs
Phantom is included via its NPM module. In order
to run tests against phantom, simply add the `usePhantom` flag to the options hash.
The plugin defaults to hitting port 4444, but you can specify your own port via
the `phantomPort` option.

###Using mocha-webdriver with your own Selenium server
 You can run your tests against your own Selenium server instance.
 To do so, use ``hostname`` and ``port`` options.
 Don't forget to remove ``username`` and ``key``.
 Note that the Selenium server should be started and ready before starting the tests.

##Documentation
wd [provides the ability](https://github.com/admc/wd#adding-custom-methods)
to add test methods to its default set of capabilities. `grunt-mocha-webdriver`
exposes the `wd` instance in the same way that `browser` is exposed, so that
you can easily add your own test methods to wd.

Also, Mocha options are exposed to tests. This is especially helpful if you want to reuse the
defined Mocha timeout for your Webdriver tests. For example you can do this in your
Webdriver based E2E tests:

```js
this.browser.waitForElementByCss('.aClass', this.mochaOptions.timeout, cb);
```

Please look at this project's Gruntfile and tests to see all that in action.

###Options
The usual Mocha options are passed through this task to a new Mocha instance.
Please note that while it's possible to specify the Mocha reporter for
tests running on Phantom, there's only one reporter currently supported
for tests against Sauce Labs. This restriction is in place to handle
concurrent Sauce Labs testing sessions, which could pollute the log.

The following options can be supplied to the task:

####usePhantom
Type: Boolean

Specifies whether the task should test against a PhantomJS instance instead
of Sauce Labs. Defaults to false. If true, the tests will run against Phantom
INSTEAD of running against Sauce Labs.

####phantomPort
Type: Int (Default: 4444)

if testing against PhantomJS with the `usePhantom` flag, specify the port
to test against.

####phantomCapabilities
Type: Object (Default: {})

if testing against PhantomJS with the `usePhantom` flag, specify the
[browser capabilities](https://github.com/detro/ghostdriver#what-extra-webdriver-capabilities-ghostdriver-offers).

####phantomFlags
Type: array (Default: [])

if testing against PhantomJS with the `usePhantom` command-line options, specify start additional flags to use. Check [here](http://phantomjs.org/api/command-line.html) or type `phantomjs -h` for complete list of flags.

####usePromises
Type: Boolean

Specifies whether to use the Promise chain version of the WD.js API. Defaults to
false (the callback version).

####ignoreSslErrors
Type: Boolean

A passthrough to the Phantom CLI runner to ignore errors with SSL certs.

####require
Type: Array <String>

An array of paths for requiring before running Mocha tests. Useful for
pre-requires that manipulate Mocha's global environment (e.g.g making Sinon
globally available).

####username
Type: String

The Sauce Labs username to use. Defaults to value of env var `SAUCE_USERNAME`.

####key
Type: String

The Sauce Labs API key to use. Defaults to value of env var `SAUCE_ACCESS_KEY`.

####secureCommands
Type: Boolean (Default: false)

If true, it will use saucelabs, with default `hostname` set to `127.0.0.1` and `port` set to `4445`
in order to send selenium commands through Sauce Connect tunnel (more info
[here](https://saucelabs.com/docs/connect#selenium-relay)).

####autoInstall
Type: Boolean

If `true` this will download Selenium and Chrome Driver to run tests locally.

####hostname
Type: String

If specified, it will connect that selenium server instead of `ondemand.saucelabs.com`.

####port
Type: Int

Selenium server port. Should be used in conjonction with ``hostname``.

####identifier
Type: Number

A Unique identifier for the generated tunnel to Sauce Labs. Will be automatically
generated if not specified. Useful for connected to existing Sauce tunnels.

####concurrency
Type: Int

The number of concurrent browser sessions to spin up on Sauce Labs. Defaults to 1.

####tunnelFlags
Type: Array
An array of option flags for Sauce Connect. See the list of available options
 [here](https://saucelabs.com/docs/connect#connect-flags).

####testName
Type: String

The name of the test, as reported to Sauce Labs.

####testTags
Type: [String]

An array of tags to associate with the test, as reported to Sauce Labs.

####browsers
Type: [Object]

An array of objects specifying which browser options should be passed to Sauce Labs.
Unless a test is run against Phantom (e.g. `usePhantom` is true), this option
*must* be specified. Each browser hash should specify: `browserName`. For saucelabs tests, `platform`,
and `version` are also required.

##Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add
unit tests for any new or changed functionality. Lint and test your code.

##Release History
See History.md

##Contributors
 - Author: [Justin Reidy](https://github.com/jmreidy)
 - You?

##License
Copyright (c) 2014 Justin Reidy

Licensed under the MIT license.
