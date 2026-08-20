---
schema_version: "1.0.0"
document_id: "989ca9379b4079ed9a18e59a7b78622f05c38df06f1f2c9f6b8156d83c0b2d8a"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/testing-your-sails-apps-with-japa/"
published_at: "2022-05-20T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:adf74b46f76242424fc09a29d9ca4c1f7e86f5a5f421d7cd803ede7e08b48e4c"
---

# Testing your Sails apps with Japa

I had a[TKYT](https://youtu.be/_MSQY3lqhCo) session with Harminder Virk, the creator of both AdonisJS and[Japa](https://japa.dev/) and I got intrigued at how Japa is API-first, simple to use and easily extensible to suit testing needs. I decided to see how it fairs in a Sails application and boy was I impressed.


I went ahead to create a Japa plugin called[preset-sails](https://github.com/japa/preset-sails) that comes bundled with everything you will need to quickly setup testing with Japa in your Sails codebase.


In this article I will both show you how to use preset-sails in your Sails codebase. Let’s sails…


## Using preset-sails


To use preset-sails, first run the below command in your Sails project directory to download both preset-sails and the Japa runner:


```text
npm   i   @japa/runner   @japa/preset-sails   --save-dev
```


Next, let’s create a test environment in config/env. preset-sails expects you to have this environment and I think its a cool practice to have a seperate test environment. With that said, create` test.js` in` config/env` and add the following code:


```text
module  .  exports   =   {
port:   3333  ,
hooks: {
// Uncomment if you have the apianalytics hook and you don't want to see logs
// from it during testing
// apianalytics: false
},
log: {
level:   'error'
},
models: {
migrate:   'drop'  ,
},
datastores: {
default: {
adapter:   'sails-disk'
}
}
}
```


You can customize the above file to your preference. The above content is a safe minimal config for a testing environment.


Alright, following so far? Great! Next let’s create a tests directory in the root of the Sails codebase. In this directory create a bootstrap.js file. This will be the file we setup Japa and it’s also the entry point Japa will use when running our tests. Once you have created this file, add the following content:


```text
const   {   processCliArgs  ,   configure  ,   run   }   =   require  (  '@japa/runner'  )
const   {   presetSails  ,   assert  ,   runFailedTests  ,   specReporter  ,   apiClient   }   =   require  (  '@japa/preset-sails'  )


configure  ({
...  processCliArgs  (process.argv.  slice  (  2  )),
...  {
plugins: [  presetSails  (),   assert  (),   runFailedTests  (),   apiClient  (  'http://localhost:3333'  )],
reporters: [  specReporter  ()],
importer  : (  filePath  )   =>   require  (filePath),
timeout:   50000  ,
suites: [
{
name:   'functional'  ,
files: [  'tests/controllers/**/*.spec.js'  ],
},
{
name:   'unit'  ,
files: [  'tests/helpers/**/*.spec.js'  ,   'tests/models/**/*.spec.js'  ],
}
]
},
})


run  ()
```


I’d like to draw your attention to two things; you see the` apiClient` function we are passing to the` plugins` array? First, notice it takes a URL that corresponds with the port we set in` config/env/test.js` . Make sure they both correspond if you update any one of them.


Second, see the` suites` array? Notice we are defining two types of test suites: functional and unit and they both have corresponding globs to where Japa will look for those files.


The idea with the functional and unit suites is that you do functional testing on your controllers and unit test your models and helpers. You can change this convention by editing the files glob or adding more globs to suit your preference.


For now lets create the directories in which Japa will look for tests in. In the tests directory create the` controllers/` ,` helpers/` and` models/` directories.


All right we are almost done. The last thing is to edit the script section of your package.json. Add a script to run Japa like so:


```text
"scripts"  : {
"test"  :   "node tests/bootstrap.js"
}
```


And that’s it. Now when you run` npm t` or` npm test` , you will call the Japa runner which will use the config in` bootstrap.js` and then run all tests accordingly.


You can also choose to run your test suites separately by running either` npm test functional` or` npm test unit` to run functional or unit tests.


## Your first test


So we’ve setup Japa but we don’t have any tests yet, let’s add both a functional and a unit test to see the testing workflow with Japa. Let’s start up with some unit test.


### Unit testing


Let’s say you have a helper for generating slugs called` generate-slug.js` in your Sails app. Let’s test this helper.


Create a` generate-slug.spec.js` file in` tests/helpers/` . Add the following code to it:


```text
const   {   test   }   =   require  (  '@japa/runner'  )


test.  group  (  'generate-slug helper'  , ()   =>   {
test  (  'returns expected slug'  , ({   assert   } )   =>   {
const   slug   =   'hello-world'
const   helperSlug   =   sails.helpers.  generateSlug  (  'hello world'  )
assert.  equal  (helperSlug, slug)
})
})
```


Pretty neat right! Japa syntax will seemm familiar if you have written tests with either Jest or[Mocha](https://blog.sailscasts.com/testing-sails-applications-with-mocha-and-super-test/) .


Notice the` { assert }` ? We are destructuring something called the[Test Context](https://japa.dev/test-context) in Japa. You will come to appreciate the TestContext as we will use it to provide a helper when doing functional testing. Speaking about functional testing, let’s write one now!


## Functional testing


Let’s test a controller that returns a course by its ID or slug. To do this create a` get-course.spec.js` file in` tests/controllers/course` .


Add the following code:


> Note we are mirroring the file structure of the` api/` folder in the Sails app.


```text
const   {   test   }   =   require  (  '@japa/runner'  )


test.  group  (  'GET /screencasts/:courseIdOrSlug'  , ()   =>   {
test  (  'Return 404 when ID or slug is not specified'  ,   async   ({   client  ,   assert  ,   route  } )   =>   {
const   response   =   await   client.  get  (  route  (  'course/get-course'  ))
response.  assertStatus  (  404  )
assert.  property  (response.  body  (),   'message'  )
})


test  (  'Return screencast by slug if it is published'  ,   async   ({   client  ,   assert  ,   route   } )   =>   {
const   course   =   await   sails.helpers.  vane  (  'course'  , { published:   true   })
const   response   =   await   client.  get  (  route  (  'course/get-course'  , { courseIdOrSlug: course.slug })).  setup  (()   =>   {})
response.  assertStatus  (  200  )
assert.  property  (response.  body  (),   'data'  )
})


test  (  'Return course by ID if it is published'  ,   async   ({   client  ,   assert  ,   route   } )   =>   {
const   response   =   await   client.  get  (  route  (  'course/get-course'  , { courseIdOrSlug: course.id }))
response.  assertStatus  (  200  )
assert.  property  (response.  body  (),   'data'  )
})


test  (  'Returns 404 if course is not published'  ,   async   ({   client  ,   assert  ,   route   } )   =>   {
const   course   =   await   sails.helpers.  vane  (  'course'  , { published:   false   })
const   response   =   await   client.  get  (  route  (  'course/get-course'  , { courseIdOrSlug: course.id })).  setup  (()   =>   {})
response.  assertStatus  (  404  )
assert.  property  (response.  body  (),   'message'  )
})
})
```


So the above is a more realistic test. Let me draw your attention to a couple of this.


Notice the` client` object being destructured from the context? That was injected by the` apiClient` plugin we passed to the` plugins` array in` bootstrap.js` You can read up on the[apiClient](https://japa.dev/plugins/api-client) in the Japa docs.


Next, notice the` route` function exposed in the Test Context, this is injected by the` presetSails` plugin we also passed in the` plugins` array.` route` is a helper that lets you pass in your actions and it will return the associated URL.


This is handy as you don’t have to remember the URLs of your routes and it acts as a safeguard in case you update the routes in` routes.js` if you are using the routes function, you don’t have to update the endpoint accross your tests. Pretty cool!


You can also pass an optional second argument which is used for interpolating routes params. In yhe example above, the endpoint in` routes.js` is` courses/:courseIdOrSlug` .


By passing the object with the value for` courseIdOrSlug` , the route function will interpolate the value and then return the route. Powerful!


Finally, you may have noticed the` sails.helpers.vane('course', { published: true })` bit. We are using the[captain-vane](https://vane.sailscasts.com/) package which lets you create factories in your Sails application.


## Conclusion


There you have it! you have now seen how you can setup and use Japa to power your functional and unit testing with the` preset-sails` plugin making it super easy. I use this setup personally in my codebase and I really like the workflow.


Let me know what you think on Twitter([@Dominus_Kelvin](https://twitter.com/Dominus_Kelvin) ) and happy coding!
