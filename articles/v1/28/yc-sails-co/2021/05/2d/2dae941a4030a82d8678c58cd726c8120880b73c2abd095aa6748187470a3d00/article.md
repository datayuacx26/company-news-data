---
schema_version: "1.0.0"
document_id: "2dae941a4030a82d8678c58cd726c8120880b73c2abd095aa6748187470a3d00"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/build-a-url-shortener-with-sails/"
published_at: "2021-05-04T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:6d684f1bffb2c3d4801cc4679b4a504d210ddc805ec07cbec1ac56cba38e061d"
---

# Build a URL shortener with Sails

Inspired by a YouTube tutorial by Kyle from Web Dev Simplified on using Express and Mongo DB to build a URL shortener, I hacked together an equivalent URL shortener powered by Sails and Sails-disks. You can find the demo of the completed project[here](https://www.youtube.com/watch?v=QgHBZ7ecXig)


I really had some fun building this project so I’d thought I’d share it with you. Before we dive right in, let’s look at the **Why** of building a URL shortener


## The purpose of a URL shortener


So at the basic level, a URL shortener mostly helps you shorten long URLs for easy sharing on the internet. You might have used a URL shortening service like[https://bit.ly](https://bit.ly/) .


## How a URL shortener works


The fundamental way a URL shortener works, is that you give it a URL say[https://sailscasts.com/courses](https://sailscasts.com/courses) and the service gives you back a shorter representative of the URL you passed in like` https://ur.l/FHRKHGDI`


Do note that the` https://u.rl` part of the URL given to you which is the TLD(Top Level Domain) is the URL of the address where the URL shortener can be found and the` FHRKHGDI` bit of the URL is the unique ID of the URL that has been shortened for you.


So when you give someone` https://u.rl/FHRKHGDI` , the URL shortening service i.e the URL shortener will check if it has a matching URL with the ID and if it does, it redirects to the long URL which in this case will be[https://sailscasts.com/courses](https://sailscasts.com/courses) .


## Let’s build it!


So now we’ve seen how a URL shortener should work at the basic level, let’s build one with Sails. To get started simply create a brand new Sails application by running the following in your terminal:


```text
sails   new   sails-url-shortener
```


When the Sails CLI prompts you for the template for your new Sails app, choose **Empty** .s


Then open up the newly generated project in your Text Editor of choice.


## The Frontend


To get started we need to flesh out the UI of our app, so head over to` views/pages` and rename` homepage.ejs` to` index.ejs` and also clear out the content inside and replace with the following HTML markup


```text
<!  DOCTYPE   html  >
<  html   lang  =  "en"  >


<  head  >
<  meta   charset  =  "UTF-8"  >
<  meta   http-equiv  =  "X-UA-Compatible"   content  =  "IE=edge"  >
<  meta   name  =  "viewport"   content  =  "width=device-width, initial-scale=1.0"  >
<  link   href  =  "https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css"   rel  =  "stylesheet"  >
<  title  >Sails URL Short'nr</  title  >
</  head  >
<  body  >
<  main   class  =  "py-6 px-24 flex items-center flex-col"  >
<  h1   class  =  "text-2xl text-gray-700"  >Short'n your URL mate!</  h1  >
<  form   action  =  "/shorturls"   method  =  "POST"   class  =  "my-4 flex space-x-2 w-1/2"   autocomplete  =  "off"  >
<  label   for  =  "url"   class  =  "sr-only"  >URL</  label  >
<  input   type  =  "url"   name  =  "url"   id  =  "url"   placeholder  =  "https://sailscasts.com"   required
class  =  "border px-4 py-2 rounded-full flex-1 focus:outline-none"  >
<  button   type  =  "submit"   class  =  "text-white px-4 py-2 rounded-full bg-blue-400 hover:bg-blue-500"  >Short'n</  button  >
</  form  >
<  table   class  =  "w-1/2"  >
<  thead   class  =  "text-left text-gray-600"  >
<  th  >URL</  th  >
<  th  >Short URL</  th  >
<  th  >Clicks</  th  >
</  thead  >
<  tr  >
<  td  >
<  a   href  =  "https://sailscasts.com/pricing"   class  =  "text-blue-400"   target  =  "_blank"  >
https://sailscasts.com/pricing
</  a  >
</  td  >
<  td  >
<  a   href  =  "GHERHJK>"   class  =  "text-blue-500"   target  =  "_blank"  >
GHERHJK
</  a  >
</  td  >
<  td  >
1
</  td  >
</  tr  >
</  table  >
</  main  >
</  body  >
</  html  >
```


That’s basically all the HTML you will need for the UI of our URL shortener. Notice we are pulling in Tailwind CSS from a CDN to help with styling the UI. So what’s happening in our UI? Well we are just simply giving the user a form with an input to enter the long URL and then a button that will submit the form to create the Short URL. We also present a table which will contain the long URL, the shorten URL(which the user can then share) and also we are showing the number of times the shorten URL was used or clicked.


Now before you lift Sails to see your UI, we need to do a couple of things: First we need to tell Sails we won’t be needing a layout for this app since we have only just a single page. To do this, head over to` config/views.js` and set the` layout` property to` false` . Secondly we need to configure the` /` (root) route of our app to point to an action(one we will be creating shortly). So let’s do that, head over to` config/routes.js` and edit the` /` route to the below code:


```text
'/'  :   'home/index'  ,
```


From the above, we are pointing the requests on` /` of our app to be handled by an action called` index.js` but this action doesn’t exists yet, let’s generate it. Run the below command in your terminal to generate the action:


```text
sails   generate   action   home/index
```


The above will create a` home/` directory under` api/controllers/` and inside it create the` index.js` action. Now edit the content of` index.js` to look like the below snippet:


```text
module  .  exports   =   {
friendlyName:   'Index'  ,
description:   'Index home.'  ,
exits: {
success: {
responseType:   'view'  ,
viewTemplatePath:   'pages/index'
}
},
fn  :   async   function   () {
}
};
```


And that’s it for the UI, you can now run` sails lift` on the terminal and see your UI.


## Creating the Short URLs


Now that we have the UI all setup, the next step is to make the button to shorten the URLs functional. To do this, we will start off by creating a model called` ShortUrl` this model will be used by Waterline to create a` shorturl` table in our database. But before then, since we want this app to be compatible with MongoDB(assuming we will be deploying it to production), we need to set the primary ID column of all documents to be` _id` as MongoDB requires. To do this, head over to` config/models.js` and find the line referencing` id` and set it to the following instead:


```text
id  : {   type  :   'string'  ,   columnName  :   '_id'   },
```


Also in` config/models.js` set the` migrate` property to` alter`


After you are done with the above configurations, generate the` ShortUrl` model by running the following command:


```text
sails   generate   model   ShortUrl
```


Sails we generate a new waterline model file in` api/models` called` ShortUrl.js` open this file and edit it to look like so:


```text
module  .  exports   =   {
attributes: {
url: {
type:   'string'  ,
description:   'URL to shorten'  ,
isURL:   true
},
short: {
type:   'string'  ,
},
clicks: {
type:   'number'  ,
isInteger:   true  ,
defaultsTo:   0
}
},
beforeCreate  :   async   function   (  shorturl  ,   cb  ) {
const   shortid   =   require  (  'shortid'  );
shorturl.short   =   shortid.  generate  ();
return   cb  ();
},
};
```


One notable thing about this model is that we are generating a` shortid` which will be a unique string to reference our short Urls inside the` beforeCreate` hook of Waterline. However we haven’t installed this dependency yet. So run the below command to install` shortid` :


```text
npm   i   --save   shortid
```


So with this model setup, anytime a new record is about being created using the` ShortUrl` model, the` beforeCreate` hook will run and assign a unique shortid to the` short` attribute of that record.


## shorturls action


Notice in our UI, the form element is set to submit the form to` /shorturls` but that endpoint doesn’t exist yet, so let’s create the action, setup the route in` config/routes.js` and add the logic to create the shorturl. Head over to the terminal and run:


```text
sails   generate   action   shorturl/create-shorturl
```


Open` config/routes.js` and add the entry for this action:


```text
'POST /shorturls'  :   'shorturl/create-shorturl'  ,
```


This means any` POST` request to` /shorturls` will be handled by` shorturl/create-shorturl` action.


Now that’s done let’s add the content for` create-shorturl.js` . Replace the content in that file to the one below:


```text
module  .  exports   =   {
friendlyName:   'Create shorturl'  ,
description:   ''  ,
inputs: {
url: {
type:   'string'  ,
isURL:   true  ,
required:   true
}
},
exits: {
success: {
responseType:   'redirect'
}
},
fn  :   async   function   ({   url   }) {
await   ShortUrl.  create  ({ url });
return   '/'  ;


}
};
```


Okay, so what’s going on in this action you might ask. Well we are telling the action to expect a parameter referenced by` url` to be submitted and then use it to create a new record using the` ShortUrl` model’s create method and after creating the record, redirect to the homepage i.e ’/’.


If you Sails application has been running, kill it and restart it again. Then try entering a URL in the form and click the button to shorten it. You will notice that you will be redirected to the homepage again. Now to verify if the record was created, kill your server and start the Sails console by running:


```text
sails   console
```


Then type inside the REPL` ShortUrl.find().log()` this should output a single record which is the one you just created now!


## Displaying the Short URLs in the table


So now that we can create Short URLs let’s display it in the table of our UI. To do this, we need to modify both` home/index.js` and` views/pages/index.ejs` . Let’s start with the action; what we need to do here is to use waterline to grab all the short URL and then pass it to the view. To do this, simply edit` fn` body in` home/index.js` to the below code snippet:


```text
fn  :   async   function   () {
const   shortUrls   =   await   ShortUrl.  find  ().  sort  (  'createdAt DESC'  );
return   { shortUrls };
}
```


So we are fetching all the records and sorting them by the` createdAt` attribute, then we pass the records we get back to the view as a Sails local with the ES6 object property shorthand which evaluates to:


```text
return   { shortUrls: shortUrls }
```


Okay! now we need to head over to our view and consume the` shortUrls` array. To do this, we will use the` .forEach` method to loop through every element in the array and then render a row in our table. So modify the table code that holds the placeholder row to the following snippet right after the closing \` tag:


```text
<  % shortUrls.forEach(shortUrl=> { %>
<  tr  >
<  td  ><  a   href  =  "  <  %= shortUrl.url %>"   class  =  "text-blue-400"   target  =  "_blank"  >
<  %= shortUrl.url %>
</  a  ></  td  >
<  td  ><  a   href  =  "  <  %= shortUrl.short %>"   class  =  "text-blue-500"   target  =  "_blank"  >
<  %= shortUrl.short %>
</  a  ></  td  >
<  td  >
<  % if (shortUrl.clicks) { %>
<  span   class  =  "text-green-400"  >
<  %= shortUrl.clicks %>
</  span  >
<  % } else { %>
<  span   class  =  "text-gray-600"  >
<  %= shortUrl.clicks %>
</  span  >
<  % } %>
</  td  >
</  tr  >
<  % }) %>
```


Also notice we are conditionally rendering a Span just for visual cue to the user feedback on when a url has been clicked at least once from when it has not been clicked before.


So save your files and lift Sails and you should see the short URL you entered.


### Making the short url redirect


Alright so we are almost done with our URL shortener but right now when you click the shortened link, you get a 404 because we haven’t written the logic for what happens when one of our short URLs are clicked. What we want is that when you click on the short URL you get redirected to the associated long URL and the click count is incremented by` 1` .


So let’s get started setting this up. First add a route entry below your other route entries in` config/routes.js` like so:


```text
'GET /:shortId'  :   'shorturl/get-shorturl'
```


Now create the` shorturl/get-shorturl` action by running the below command:


```text
sails   generate   action   shorturl/get-shorturl
```


Then add the following content to the` get-shorturl.js` file


```text
module  .  exports   =   {
friendlyName:   'Get shorturl'  ,
description:   ''  ,
inputs: {
shortId: {
type:   'string'  ,
description:   'The short url identifier'  ,
required:   true
}
},
exits: {
success: {
responseType:   'redirect'
},
notFound: {
responseType:   'notFound'
}
},
fn  :   async   function   ({   shortId   }) {
const   shortUrl   =   await   ShortUrl.  findOne  ({ short: shortId });
if   (  !  shortUrl) {  throw   'notFound'  ;}
const   newClicks   =   shortUrl.clicks   +   1  ;
await   ShortUrl.  updateOne  ({ id: shortUrl.id}).  set  ({
clicks: newClicks
});
return   shortUrl.url;
}
};
```


So basically in the above action, when a user makes a` GET` request using one of our short URLs, we look up if that Short URL exists in our DB, if it doesn’t exists, we bail by exiting with a` notFound` exit. However if it exists, we fetch it then increment its clicks by` 1` and then redirect to the long URL helpd by the` url` attribute.


Now save all your files and then lift sails and you should have a basic functioning URL shortener all powered by Sails and the Sails-disks in-memory database.


## Conclusion


In this article we took a step by step approach and built a basic URL shortener with Sails, you can extend this basic URL shortening service any way you like as you can fork the code on[GitHub](https://github.com/sailscasts/sails-url-shortener) . Here are some ideas:


- Implement a way for a user to only see the URLs he/she added
- Make the short URL to be easily copied and shared on the internet
- Host your customization of the project on your domain and use it as your URL shortener!


I’d be looking forward to see any customization you will be make atop this basic URL shortener powered by Sails. You can[tweet](https://twitter.com/Dominus_Kelvin) at me your awesome work and questions.
