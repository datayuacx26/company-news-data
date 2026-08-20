---
schema_version: "1.0.0"
document_id: "4d3b5e0c5c5bebca810051c75dbfb95603ae8d79c0eaeb8f725d9611fc02744a"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/testing-sails-applications-with-mocha-and-supertest/"
published_at: "2021-01-26T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:43d85e7c65434b3b6b6b13c817007809256d56f2cf6fd586b077270e24e09919"
---

# Testing Sails Applications with Mocha and SuperTest

The goal of this article is to make setting up testing in your Sails application as simple as possible. Let’s get started


## A brand new Sails app


So we are going to start off on a clean slate by creating an API-only brand new Sails application. We do that by running:


```text
sails   new   sails-mocha-supertest-starter   --no-frontend
```


Note: for the above command to work, you must have installed Sails globally by running:


```text
npm   i   -g   sails
```


## Structuring our code base for testing


In Sails, you will want to test your controllers, models and helpers, so we can have our test structure mirror this. In the Sails application you just created, create a directory` test/` in the project root and inside it add the following directories:` controllers` ,` helpers` ,` models` .


This structure is a good starting point which you can extend to your heart content when the need arises. Let’s move on to the Master of Ceremony of our testing fiesta!


## Installing Mocha


Mocha is the testing framework we will be choosing for our setup. It’s a feature-rich Javascript test framework running on both Node.js and in the browser.


We will be installing Mocha as a development dependency by running:


```text
npm   i   mocha   --save-dev
```


After the installation is complete, edit the` test` script in the` scripts` section of your` package.json` to run Mocha. Like so:


```text
"test": "mocha"
```


## Configuring Mocha


So we’ve installed Mocha and made it that when we run` npm test` it will run Mocha and Mocha will in turn run our automated tests in the` test/` directory. By default, Mocha will run just the test files in the` test/` directory and not run test files within nested directories. Seeing our` test/` directory structure currently, we have got to do something about this.


Mocha comes with a number of ways for you to configure it, from passing configuration flags in the command line to using` .json` and` .yml` files. But we will be using a good ol’ JavaScript file to configure Mocha. So create at the root of your project a file called` .mocharc.js` .


In this file export a CommonJS module like so:


```text
module  .  exports   =   {}
```


Within the object you are exporting is where you will be setting a couple of Mocha configurations. The first one is to get Mocha to look for test files within subdirectories in the` test/` folder so add` recursive: true` within that exported object like so:


```text
module  .  exports   =   {
recursive:   true
}
```


So now Mocha will execute tests within subdirectories of` test/` . Next we need to be able to` lift` Sails before Mocha executes any test because our controllers, models and quite frankly all of Sails won’t function without that. Also after Mocha has ran all our tests, we should` lower` Sails. let’s see how Mocha help us achieve this.


## Mocha Root Hooks Plugins


Mocha provides some hooks in which you can run before or after each test suites but what we are really interested in is the Root Hooks Plugins.


A[Root Hook Plugin](https://mochajs.org/#root-hook-plugins) is a JavaScript file that we can require which will register one or more hooks to be used all accross our test files. This is brilliant! Because with this capability, we will add a hook to start Sails before any of our tests ever run and also lower Sails after all of our tests are done!


So let’s write the Root Hook Plugin. We will create a file in` test/` directory called` hooks.js` . In this file is where we will setup the hooks to be used accross our tests. So let’s right the content of the file. First of, we will require Sails:


```text
const   sails   =   require  (  'sails'  )
```


Then we will an object called` mochaHooks` like so:


```text
exports  .mochaHooks   =   {}
```


The hooks we will be registering in this plugin will be` beforeAll` and` afterAll` Mocha Hooks. So let’s stub both hooks out:


```text
const   sails   =   require  (  'sails'  )
exports  .mochaHooks   =   {
beforeAll  (  done  ) {
},
afterAll  (  done  ) {}
}
```


Before we finish setting up the Root Hook Plugin, we need to do two things: Install SuperTest and create a test environment for Sails. Let’s start with the low hanging fruit


## Installing SuperTest


SuperTest is an HTTP assertion agent from the folks at Vision Media. SuperTest is based off[superagent](https://github.com/visionmedia/superagent) . Basically what this means is that we can make requests to our Sails app endpoints with SuperTest and make assertions like, “does this endpoint return the status code 200?”, you get the idea!


Let’s install SuperTest as a development dependency by running:


```text
npm   i   supertest   --save-dev
```


Once that’s done, we move on to…


## Creating a test environment


Sails gives you the ability to define environments for your Sails application as JavaScript files found in` config/env/` . By default your app already have a` production.js` env file. So all we need do now is create a` test.js` file in` config/env` . We need to do this so we can provide some overrides for the environment Sails will be in when our tests runs. So create` config/env/test/js` and let’s start fleshing it out and explaining why we need override each setting with some comments:


```text
// config/env/test.js
module  .  exports   =   {
// Skip grunt hook as we won't be needing it for running our tests
hooks: {
grunt:   false
},
// Disable all logs except warnings and errors.
log: {
level:   'warn'
},
// We want to drop our database before run our tests to have a clean database each time
models: {
migrate:   'drop'
},
// We want to use the built in sails-disks storage for our tests.
// You can set up any database you want really in here.
datastores: {
default: {
adapter:   'sails-disk'
}
}
}
```


And that’s it for` config/env/test.js`


## Back to the Mocha Root Hook Plugin


So now that we have added SuperTest and have created and configured a test environment for Sails, Let’s finish our Mocha Root Hook Plugin. We will begin in the` beforeAll` hook.


```text
beforeAll  (done) {
sails.  lift  ({
environment:   'test'
}, (  error  ,   sails  )   =>   {
if   (error)   return   done  (error)
global.app   =   require  (  'supertest'  )(sails.hooks.http.app)
return   done  ()
})
}
```


Let’s zoom in into what we are doing here, we are lifting Sails by calling` sails.lift` this will start up Sails in development mode, however we are overriding that by passing an options object as the first argument to` sails.lift` and in that object we set the` environment` property to` test` . This tell Sails to use the configuations we have specified in` config/env/test.js` .


Next, in the callback passed to Sails, which will return an error if an` error` occured and a` sails` instance. We are then accessing our app via` sails.hooks.http.app` and passing it as an argument to` require('supertest')` .


We then went on to assign the call to SuperTest to a global variable called` app` like so` global.app` . This global object will make` app` available to all our test files.


Lastly, we return the call to the` done` callback with the` error` and` sails` instances.


## afterAll()


Now that we are done lifting Sails before we run any tests, we might as well lower it after all tests have been ran. We do so in the` afterAll` hook. Like so:


```text
afterAll   (done) {
try   {
sails.  lower  (done)
}   catch   (error) {
return   done  (error)
}
}
```


The implementation of the` afterAll` hook is quite straight forward: We try to call` sails.lower` passing in the done call back saying we are done callback since it’s an asynchronous operation. Then in the catch block, we are returning the call to` done(error)` which will pass the error to the caller if there is an error.


## require the hook please


So we have defined the hook but Mocha doesn’t know about it yet. We tell Mocha about the hook by setting a require property in the` .mocharc.js` configuration file and passing in a string which will be the path to our Root Hook plugin. Here is how your` .mocharc.js` file should look now:


```text
module  .  exports   =   {
recursive:   true  ,
require:   'test/hooks.js'  ,
}
```


## Timing out


Mocha has a default timeout of` 2000 milliseconds` . Our Sails app might need more time to lift than that so let’s increate the timeout to` 10000 milliseconds` ?


So in` .mocharc.js` we add:


```text
module  .  exports   =   {
recursive:   true  ,
require:   'test/hooks.js'  ,
timeout:   10000
}
```


And we are done, with the setup, let’s test if everything is in order by writing our very first test.


## Our very firt test


Recall we created a brand new Sails app with the` --no-frontend` flag? This mean we don’t have any pages at the moment so if you visit` /` when you run the app, you will get a 404. Let’s test if this is really true!


In` test/controller` create a` home` directory and inside it, add a file called` index.test.js` . Open that file and add the following:


```text
describe  (  'home'  , ()   =>   {
it  (  '/ - should return 404'  , (  done  )   =>   {
app.  get  (  '/'  ).  expect  (  404  , done)
})
})
```


So what we are doing here is that, we are creating a test suite with a call to Mocha’s` describe` and then within it, we create a test with` it` function which takes a string as the first argument describing what we are testing, and we are passing as the second argument, a callback function which contains the` done` callback since our test will be asynchronous.


We are then accessing the` app` global variable we set in` test/hooks.js` which we set here` global.app = require('supertest')(sails.hooks.http.app)` . And then we are accessing the` get` method, passing it the` /` home route and using` .expect` we are telling SuperTest to assert that it returns` 404` and we pass in` done` to` .expect` signifying we are done with that test case.


Now head over to your terminal and run


```text
npm   test
```


Or


```text
npm   t
```


And you should see Mocha executing one test and that test is passing ✅


## Test Coverage


You may need to configure Test Coverage check for your application which allows you know how many percent of your codebase has been covered by your test. For this we will be using[nyc](https://www.npmjs.com/package/nyc) - the Istanbul commandline interface


We will install it like so:


```text
npm   i   nyc   --save-dev
```


After installing we add a` coverage` script in package.json, like so:


```text
"coverage"  :   "nyc npm test"
```


So when you run` npm run coverage` nyc will output the coverage statistics of your application in your terminal. You can see in more ways how you can configure it \[here\].


## .gitignoring nyc output directories


nyc will output some files and directories when it run, we don’t want to commit those so we add them to` .gitignore` . Add these two directories to your` .gitignore`


```text
coverage
.nyc_output
```


## Conclusion


In a real world application, not testing your application is not an option, this article showed you how to setup automated testing in your Sails app using Mocha and SuperTest. We also covered adding test coverage with nyc. And we are done ✅


The[code](https://github.com/sailscasts/sails-mocha-supertest-starter) for this article is on GitHub you can check it out.
