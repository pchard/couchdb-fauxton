## Nightwatch Functional Browser Test

Hello.

[Nightwatch Homepage](http://nightwatchjs.org/)   
[Nightwatch GitHub Repo](https://github.com/beatfactor/nightwatch)

There are several nightwatch tests already written for Fauxton.

Before running the browser tests, you need to first set it up. Editing your `settings.json` file (or
`settings.json.default` or whatever you're using) and find the following section near the bottom:  

```javascript
    "nightwatch": {
      "username": "",
      "password": "",
      "launch_url": "",
      "custom_commands_path": "test/nightwatch_tests/custom-commands",
      "globals_path": "test/nightwatch_tests/helpers/helpers.js"
    }
```

There, add the `username`, `password` and `launch_url` values. The username and password needs to be an admin
account already setup on your CouchDB installation. The `launch_url` should be the base URL of your local installation,
minus the "http://" prefix, e.g. `localhost:8000` or `thomas.dev:8000`.

To run the test:

    $ npm install
    $ grunt nightwatch


You can run individual single tests by running

    $ grunt nightwatch --file="createsDatabase"

if you want to run the testfile `createsDatabase.js`.

## Writing Tests
You can contribute by writing tests for Fauxton as well.  

Please take a look at the existing examples in the `app/addons/documents/tests` or `app/addons/databases/tests` folders.

Below are a few caveats to writing tests.

1. When you write a new test, if the component you are testing doesn't have any nightwatch tests already, you will
need to create a folder for it, and add its file path to the list of test folders in `tests/nightwatch_test/nightwatch.json`.
1. Before each test is run, a database called'fauxton-selenium-test' is created, then deleted after each test.
2. Use `.verify()` instead of `.assert()` in your tests. This will allow the tests to continue, even if the browser
has failed, and will not exit or skip subsequent tests.
3. `.waitForElementNotPresent()`, `.waitForElementNotVisible()`, `.waitForElementPresent()`, `.waitForElementVisible()`,
will exit testing by default if the Element is not found. There is a third argument, 'abortOnFailure', if you set this
to 'false', the rest of the tests will continue even if this assertion fails.
> there is a custom function called clickWhenReady which will wait for an element to visible, then click on it

4. Sometimes `.click()` doesn't work reliably (most likely if the element you are clicking on doesn't have an
individual ID selector). You can use dom selectors to simulate a click by using `.execute('document.querySelector("#CSS Selector.HERE").click();')`.  
5. The function `.pause(time)` is sometimes necessary, although we have tried to avoid excessive use of a hard coded 
pausing. Instead try and make use of the `.waitForElement` functions instead of `.pause(time)`.

## Nightwatch Files in Fauxton
There are several Nightwatch related files spread out in the couchdb-fauxton folder.

1. Individual browser tests are kept in

        couchdb-fauxton/app/addons/[component]/test/nightwatch

    where [component] is the area you are testing. For example, there is an individual test located in:

        couchdb-fauxton/app/addons/documents/tests/nightwatch/createsDocument.js

2. Custom Commands, Global helper functions, and the Configuration are kept in

        couchdb-fauxton/tests/nightwatch_tests

   The configuration file is `nightwatch.json`. Please note that this file is auto-generated by grunt every time you
   run the nightwatch task. It examines the content of your `settings.json` file to figure out what tests to run.
   Global helper functions are in `helpers/helpers.js`.  
   Custom commands are `custom-commands/functionName.js`.
   -  The custom command name is the name of the file itself, and it needs to follow either the `loginToGUI.js`
   pattern, or the `createDatabase.js` pattern (for async commands).    

   Selenium related files are kept in the `selenium` folder

3. Node  

   The folder `node_modules/nightwatch` contains the program files. You will not need to edit these files, but there are some provided examples in there to look at.
