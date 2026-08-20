---
schema_version: "1.0.0"
document_id: "947d13060abd53ff0d22f7d79537773965fdbd1aa70d7878c8f6a31f5916c030"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/how-i-setup-a-web-socket-client-in-nuxt/"
published_at: "2022-03-10T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:0192ab5c76cef8da3eca84bc8b2ee94dd187b9757aa1b5308bebdea719868a08"
---

# How I setup a WebSocket client in Nuxt

As of the time of writing this article, there has been some questions and more friction with setting up WebSocket clients for a Sails backend.


This article aims to be a reference to provide clarity on how to setup WebSocket clients for something like an SPA.


Though we will be focusing on Nuxt for the article, I believe with little adjustments you can use a similar setup for other UI frameworks. Let’s get started…


## Prerequisites


For this article you’ll need to have a Nuxt 2 project as we won’t be diving into creating one and just assume you have one already.


You will also need to have a Sails project with WebSocket enabled(which should be enabled by default)


> Note: This setup might work for Nuxt 3 but I am assuming it may need some tweaking so bear that in mind.


## The dependencies


Sails provides an official socket client that is meant to be used as a co-dependency with the socket.io client. However as of the time of this article, **you have to install a precise version of both packages because the Sails WebSocket server don’t support newer version of` socket.io-client`** . So in your Nuxt application run the below NPM command to install both[sails.io.js](https://www.npmjs.com/package/sails.io.js) and[socket.io-client](https://www.npmjs.com/package/socket.io-client) :


```text
npm   i   --save    [email protected]     [email protected]
```


## The setup


The way I approach this setup is with the thinking that I want my WebSocket client to run once and establish a connection once the Nuxt application is running. Also I took into consideration that I want a single WebSocket client for my usecase. With that in mind, I decided to define the start up code for the WebSocket client in a Nuxt plugin.


In Nuxt, plugins are JavaScript code you want to run before instantiating the root Vue.js application.


Which makes sense as I want to be able to access the WebSocket client from my Vue components.


With that said, I created a Nuxt plugin called` sails-io.js` in the` plugins/` directory and here is the content of the plugin:


```text
const   io   =   require  (  "sails.io.js"  )(  require  (  "socket.io-client"  ));


io.sails.url   =   `${  process  .  env  .  NUXT_ENV_API_URL  }`  ;


export   default   (  context  ,   inject  )   =>   {
// You can listen to WebSocket events here
inject  (  "io"  , io);
};
```


Okay let’s dissect the above code…


```text
```


In the above line we are creating the` io` object from` sails.io.js` and passing in` sails.io-client` as the only argument.


> Note: It is recommended to do this like I’m doing above as sails.io.js wants to have access sails.io-client package as soon as possible!


```text
io.sails.url   =   `${  process  .  env  .  NUXT_ENV_API_URL  }`  ;
```


Here, you can I am passing the the URL of the Sails server(API) to the` io` object we created previously. Note I am using an environment variable here to be flexible enough to not have to change that line for development, production or staging environments but you can just pass in[http://localhost:1337](http://localhost:1337/) .


Finally…


```text
export   default   (  context  ,   inject  )   =>   {
// You can listen to WebSocket events here
inject  (  "io"  , io);
};
```


So this is the Nuxt plugin interface. I am using the` inject` function provided by the Nuxt plugins API to “inject” the` io` object to the Vue instance. This means I can access the` io` object in my components via` this.$io` . Cool!


Another thing I like to mention is that the exported function above, is a good place to listen for WebSocket events and dispatch Vuex actions(like I did for this setup). So for example are building a Twitter clone and want to implement the follow notification, you can do:


```text
export   default   (  context  ,   inject  )   =>   {
io.socket.  on  (  "user"  , (  message  )   =>   {
switch   (message.verb) {
case   "followed"  :
context.store.  dispatch  (  "userFollowed"  , message.notification);
break  ;
}
});
inject  (  "io"  , io);
};
```


Of course your Sails API have to implement the broadcasting of the notification.


## Registering the plugin


Now that the plugin has been defined, Nuxt don’t know about it yet. Let’s tell Nuxt that we have a new plugin by adding our plugin to the plugins array in` nuxt.config.js` :


```text
plugins  : [  '~/plugins/sails-io'  ],
```


And that’s it, do well to restart your Nuxt application if it was running previously. If everything went successfully, your Nuxt application is now a WebSocket client ready to receive and send WebSocket requests to your Sails API.


## Sending WebSocket requests


Because Sails is super cool and abstracts away emitting WebSocket events, you can send WebSocket requests using the virtual` POST` and` GET` provided by the` sails.io.js` client. Let’s see an example. Imagin I have a chat application and I want to send a chat message, I can use the virtual` POST` method like so:


```text
this  .$io.socket.  post  (
`${  BASE_URL  }/chats`  ,
{
textContent:   this  .textContent,
counterpartId:   this  .$route.params.counterpartId,
},
async   (  _  ,   jwRes  )   =>   {
this  .chats.  push  (jwRes.body.data);
this  .textContent   =   ""  ;
}
);
```


Of course I will run the above code when either enter key is hit or the “send” button is clicked in my chat application.


You can see[this](https://www.youtube.com/watch?v=QwF2QiIVD7I&list=PLzn32d5NSCl_DtZ6rcTAwbmunAfvsP5io&index=6) Sailsconf 2021 talk by Rachael McNeil on Building realtime DMs with Sails to learn more on building chats with Sails.


## Conclusion


Alright, I hope this is serves as a good reference when you are on and about implementing a WebSocket client for your Sails backend.
