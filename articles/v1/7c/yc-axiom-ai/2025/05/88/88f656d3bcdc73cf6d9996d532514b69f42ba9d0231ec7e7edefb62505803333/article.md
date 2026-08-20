---
schema_version: "1.0.0"
document_id: "88f656d3bcdc73cf6d9996d532514b69f42ba9d0231ec7e7edefb62505803333"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/scraping-web-with-js/"
published_at: "2025-05-20T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:3ec0e984b1092b45404f7153a893a6033d67e57f525e6cc8eccdfdee4f843a2c"
---

# Scraping Web Content with JavaScript

[← All blog posts](https://axiom.ai/blog)


# Scraping Web Content with JavaScript


May 20, 2025


Scraping web content with JavaScript opens up a world of possibilities. This is aimed at developers who are comfortable with the basics of JavaScript and understand how to run scripts. We recommend knowing the basics of:


- JavaScript and Node.js
- DevTools


If you don't have experience with these, don't worry! We are going to attempt to make this article as accessibly as possible to all experience levels.


## Introduction to JavaScript & Node.js


JavaScript is a scripting language that powers most interactive features on a website that you use - some sites are even built purely in JavaScript using frameworks like React! When you click a button on a website, they will often use JavaScript to perform the expected action, such as submitting a form, or calling another resource.


Node.js provides a JavaScript runtime that is based on Chrome's V8 JavaScript engine that allows you to execute your JavaScript libraries outside of a browser context. This means that the scripts that you write no longer need to be included in a web page for them to run and perform actions. This is most useful for allowing JavaScript scripts to run on a server. You can learn more about it at[https://nodejs.org/](https://nodejs.org/) .


## Querying a website


HTTP clients can be used to send requests to a server and then receive a response from the server. When you load a webpage, resources are retrieved from the server of the site that you are viewing.


Ultimately, there are some considerations that you should take into account when choosing the method to use to query the data from a website, such as:


- Your current environment - you may find that the project that you are working on already has a library installed, or a preferred method of handling HTTP requests.
- Features - certain libraries such as Axios and Got offer additional features on top of the standard Fetch API implementation.
- Bundle size - any time we add new libraries to a project it can increase the bundle size. If you are concerned about bundle size, it may be best to stick with the Fetch API as a built in option.


Where possible, we would recommend attempting to find an API for the content that you are looking to scrape rather than scraping the content directly from the site. For example, rather than scraping a subreddit for the content that you are looking for, consider using their` .json` or` .csv` endpoints that can give you the data in a machine-readable format.


## Querying with pure JavaScript


The` fetch` function that is built into JavaScript offers a method of extracting web data from a website, provided it's in a specific format, such as JSON. This function returns a promise and should be dealt as such, for example to retrieve the data on axiom.ai's subreddit:


```text
const   fetch_data   =   async   ()   =>   {
const   res   =   await   fetch  (  "https://www.reddit.com/r/axiom_ai.json"  );


const   data   =   await   res.  json  ();
}


fetch_data  ();


```


As this returns a Promise, this will wait for the response from the server before continuing. You can then use the data stored in the` data` variable as required.


Node.js also has a built in option to handle HTTP clients to retrieve data from website. This is one of the simplest methods of getting started as it does not require the installation of additional libraries or dependencies.


```text
const   http   =   require  (  'http'  );


const   req   =   https.  request  (  'http://example.com'  ,   res   =>   {
const   data   =   [];


res.  on  (  'data'  ,   _   =>   data.  push  (_));
res.  on  (  'end'  , ()   =>   console.  log  (data.  join  ()));
})


req.  end  ();


```


## Querying using JavaScript libraries


There are various libraries available that can help speed up your development. Let's look into some of them.


### Axios


[Axios](https://axios-http.com/docs/intro) is a Nodejs library that extends the functionality of the native` http` module that comes built in. It's quite similar to` fetch` in that it is also promise based. This will need to be installed prior to use by calling:` npm install axios` .


To use Axios to retrieve data you can use the following snippet:


```text
const   axios   =   require  (  'axios'  );


axios.  get  (  'https://api.example.com'  )
.  then  (  response   =>   {
console.  log  (response.status);
console.  log  (response.data);
})
.  catch  (  error   =>   {
console.  error  (error);
})


```


### Got


[Got](https://www.npmjs.com/package/got) is a feature-rich HTTP request library that is specifically designed to be used with Nodejs. Under the hood it uses the fetch API to send your requests but also offers some advanced functionality that you might like to take advantage of. This will need to be installed prior to use by calling:` npm install got` .


To use Got to retrieve data you can use the following snippet:


```text
const   got   =   require  (  'got'  );


got  (  'https://api.example.com'  )
.  json  ()
.  then  (  data   =>   {
console.  log  (data);
})
.  catch  (  error   =>   {
console.  error  (error);
})


```


## Data Extraction with JavaScript


Once you have queried the site for that data that you wish to use within your script, you'll need to figure out how you want to extract that data to be able to actually *use* the data.


### JSON


If the data that you have retrieved from the website is in JSON format we have good news for you - JavaScript can handle this natively using the` JSON.parse()` function. When combined with the Fetch API this can be quite simple to do, for example:


```text
fetch  (  'https://api.example.com'  )
.  then  (  response   =>   {
return   response.  text  ();
})
.  then  (  data   =>   {
try   {
const   parsedData   =   JSON  .  parse  (data);
console.  log  (  "Parsed data:"  , parsedData);
}   catch   (error) {
console.  error  (error);
}
})


```


This will allow you to access the data stored within the` parsedData` variable. *Note, we've left out some error handling for brevity.*


### HTML - Regular expressions


If the data that you are retrieving is HTML content then the built in method of handling this would be to use regular expressions (RegEx) to parse the data. This is a bit cumbersome as RegEx can be quite a complex function to construct. We won't be diving into the ins and outs of RegEx during this article, but below is a short snippet on how to extract H1 titles from a string of HTML.


```text
const   results   =   htmlString.  match  (  /<h1>(  .  +  )<  \/  h1>/  );
console.  log  (results);


```


### HTML - jsdom


[jsdom](https://www.npmjs.com/package/jsdom) replicates jQuery's functionality in the browser DOM. This would work best if you are retrieving HTML data from a website, such as scraping the whole site. This library can also parse JavaScript within the HTML, allowing you to interact with basic JavaScript - though it should be noted that this may not work as expected with JS files that are loaded into the page, which most sites will do these days, this is disabled by default. To get started, you'll need to install jsdom in your project by running:` npm install jsdom` .


Let's look at an example:


```text
const   jsdom   =   require  (  'jsdom'  );
const   {   JSDOM   }   =   jsdom;


const   dom   =   new   JSDOM  (  `<!DOCTYPE html><h1>Hello, World!</h1></html>`  );


// Print the content of the H1 element.
console.  log  (dom.window.document.  querySelector  (  'h1'  ).textContent);


```


This can be used for larger websites, with the results stored in an array, for example:


```text
const   jsdom   =   require  (  'jsdom'  );
const   {   JSDOM   }   =   jsdom;


// Let's imagine we have a bunch of HTML in here like the example above.
const   dom   =   new   JSDOM  (  `...`  );


const   results   =   dom.window.document.  querySelector  (  'h1.title'  );


for   (  var   i   =   0  ; i   <   results.  length  ; i  ++  ) {
console.  log  (results[i]);
}


```


## Using these methods within axiom.ai


The methods above can be used within axiom.ai to extend it's existing functionality. Let's dive into how.


### No-code extension


As standard, the axiom.ai extension offers a range of scraping steps that can be used in order to scrape data from a website. These require very little set up and gives you control over the selection of elements on the page without the need to add code.


In addition to this, it's possible to use *some* of the methods above within theWrite Javascript step within your automations. It's important to note that the importing of libraries is not supported so it would only be possible to use features that make use of the Fetch API.


### Code


As part of our future updates to axiom.ai, it will be possible to build your automations with code and thus would enable all of the methods above within your automations, including the ability to install your own libraries to run your scripts.


## Wrapping up


We covered a lot in this article that has been centred around scraping web content with JavaScript, including some tips on some libraries that you can use in order to parse the data that you have extracted. The above article is by no means exhaustive so you may find other solutions that fit your project better than those above - but hopefully we have pointed you in the right direction!


[Join discussion on r/axiom_ai](https://reddit.com/r/axiom_ai)


###### On this page


- Introduction to JavaScript & Node.js
- Querying a website
- Querying with pure JavaScript
- Querying using JavaScript libraries
- Data Extraction with JavaScript
- Using these methods within axiom.ai
- Wrapping up


By


[Karl Jones](https://axiom.ai/authors/karl-jones) , Technical writer


Karl is a technical writer with axiom.ai with a computer science background and 10+ years of customer support experience. In his spare time he…
