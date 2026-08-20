---
schema_version: "1.0.0"
document_id: "9ad492296a4ffcac58e6dfa930c6608e4299c9e4abaee6bf4edca57643cc5c7d"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/how-to-specify-a-different-sails-port/"
published_at: "2021-08-12T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:d85d86a92fdcc17b22d54d456f94012af3556f4cc403c97e8b3a72c45ffa02b3"
---

# How to specify a different Sails port

When you start the development Sails server by running` sails lift` or` sails l` , Sails will make your application available on port` 1337` so when you visit` localhost:1337` you can see your Sails application.


However port` 1337` is not the default port for your Sails application. Surprised? Well the thing is port` 1337` is more of a fallback port than it is a default port for your Sails application. Let me explain…


## How Sails determines the port to run on during development


When you run` sails lift` , Sails will default to look for the TCP port to run on by checking the` port` setting which is a top-level setting on the` sails.config` object.


If` sails.config.port` is not set, Sails will then check if the` PORT` environment variable is set and use that instead. However if neither the` sails.config.port` setting or the` PORT` environment variable is set, then Sails will fallback to using the port` 1337`


So now you know how Sails chooses the port to run on during development, let’s look at how you can specify a port…


## Specifying a port


So from the above section, you will see Sails will look for the port in the` sails.config.port` setting or the` PORT` environment variable. Let’s see how we can set these starting from` sails.config.port` .


## sails.config.port


This setting can be set in a couple of ways. Let’s see them


### config/local.js


In your config/local.js file, you can add a port property to specify the port you want sails to run on. Like so:


```text
// config/local.js
module  .  exports   =   {
port:   9000
}
```


I mostly prefer this way as it allows the port I specify to be for just my machine so other developers working on the project can have the liberty of running Sails on any port they deem fit or even the fallback 1337 port if they want.


### config/misc.js


The misc.js is mostly use to specify globally the top-level Sails settings and since` sails.config.port` is one of them we can create a` misc.js` file in the` config/` folder and add a` port` property. Like so


```text
// config/misc.js
module  .  exports   =   {
port:   3000
}
```


Using this method however will force every developer working on the project to run Sails on that port. If that’s the intent then you can go for it!


### config/env/


The` confg/env/` folder contains various environment-specific settings and one of those settings is the` port` to run on per each enviroment. I like this method as it can be used to specify a different port to run Sails on during tests for example. Let’s see specifying the port to run Sails on in the test environment:


```text
// config/env/test.js
module  .  exports   =   {
port:   8080
}
```


As mentioned above, this method comes in handy for mostly test environment overriding of the port as let’s say you want to run your[Sails test](https://blog.sailscasts.com/testing-sails-applications-with-mocha-and-super-test/) while your Sails application is still lifted, you can specify to use another port during testing to have that sort of behavior.


### PORT enviroment variable


Now we have seen all the possible ways to set the` sails.config.port` setting above, let’s finally look at specifying the PORT environment variable. To specify the port simply pass a` --port` option when running` sails lift` like so:


```text
sails   lift   --port   8000
```


I mostly use this method for one-offs where I just want to run my Sails project momentarily on a different. This comes in handy if I already have a Sails application running on 1337 and I want to run another Sails application quickly.


Remember Sails will first look for the port on the` sails.config.port` setting so for this to work,` sails.config.port` must not have been set.


## Conclusion


In this article we burst the myth that Sails run on 1337 by default and then went on to see multiple ways to specify a running port for Sails during development which can be handy for test scenarios or running more than one Sails application on your machine at a time.
