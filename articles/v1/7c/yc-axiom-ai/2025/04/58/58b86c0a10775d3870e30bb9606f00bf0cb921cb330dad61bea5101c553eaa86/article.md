---
schema_version: "1.0.0"
document_id: "58b86c0a10775d3870e30bb9606f00bf0cb921cb330dad61bea5101c553eaa86"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/how-axiom-ai-helper-was-created/"
published_at: "2025-04-29T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:d496bb94165be80d56c816dc5fa779f74a7176aa1f1af6dd3c0272a12797ca83"
---

# How axiom.ai Helper was created

[← All blog posts](https://axiom.ai/blog)


# How axiom.ai Helper was created


April 29, 2025


The axiom.ai[API](https://axiom.ai/docs/developer-hub/api) can be super powerful when used to trigger your automations, pass data to them, and check the status of the automations. The API has a wide variety of users, such as being used to trigger your automations from third-party services such as[Zapier](https://axiom.ai/guides/zapier) ,[Make](https://axiom.ai/guides/post-data-to-make) ,[cURL](https://axiom.ai/guides/curl) ,[Power Automate](https://axiom.ai/guides/power-automate) and[much more](https://axiom.ai/guides) .


When considering different methods of using the API an idea came up regarding the use of axiom.ai as 'middleware' - a piece of software that can be used to connect two other services. The initial idea of this was using axiom.ai to capture form data from a website and pass it onto your automation to do one of the following operations:


1. Send an email notification containing the form data.
2. Logging the form data in a Google Sheet.
3. Performing operations on the data.
4. Pass the data onto another service.


Or all of the above - all of these options could be added into a single automation, meaning that with a single function call to the` axiom-ai-helper` package you could have the data propagate throughout your system.


> 💡 Note, this package was created by the author, an employee of axiom.ai, but is not an official product of axiom.ai. Issues, questions and feature requests should be directed to the author through the repository:[npmjs.com](https://www.npmjs.com/package/axiom-ai-helper) .


## Installing the package


` axiom-ai-helper` is available to download on[npmjs.com](https://www.npmjs.com/package/axiom-ai-helper) . To install, navigate to your project and install the package using the following command:


` npm install axiom-ai-helper`


You can then import the required functions individually, at time of writing, the` trigger` and` checkStatus` functions are available - there are plans to continue development if new API features become available in the future.


## Usage


I wanted to make this package as easy as possible to use, and as such, only included the standard requirements of the axiom.ai API. You'll need the following to be able to use the package, all can be acquired from axiom.ai:


1. An account
2. An API key


You'll also need to have an automation to use with your code - for triggering automations, you'll need to start your automation with a[Receive data from another app](https://axiom.ai/docs/no-code-tool/reference/steps/receive-data-from-another-app) step. *Tip: ensure that you have the 'test data' filled out, it'll help with testing and debugging, I promise!*


## Functions


The package is built using JavaScript and is based on simple HTTP requests, this code is open source and is available on Github:[axiom-ai-helper](https://github.com/Karljoones/axiom-ai-helper) .


The functions directly relate to the options that are available within the axiom.ai API.


Function Signature Endpoint


Trigger trigger(name, data, key) /trigger


Status checkStatus(name, key) /check-status


I'd recommend storing the API key in an` .env` file to safeguard it and ensure that it's not uploaded to a public repository.


### Triggering your automations


The` trigger` function can be used to trigger an automation that you have previously build in axiom.ai, this accepts 3 parameters:


Name Details Optional


name Name of the automation No


key API key No


data Data to send to the automation, formatted as an array of arrays Yes


This will return an object containing 3 values that have been returned from the API, see below:


```text
{
status  :   ""  ,
link  :   ""  ,
message  :   ""
}


```


Key Details Values


status The status of your automation "success""error"


link A link to the running automation "{link}"/null


message Any error messages "{error}"/{"Automation successfully triggered"}


An example of running this is included below:


```text
import   { trigger }   from   'axiom-ai-helper'  ;


...


const   result   =   await   trigger  (name, key, data);
console.  log  (result.message);


```


### Checking the status of your automations


The` checkStatus` function can be used to check the status of an automation that you have previously run in axiom.ai, this accepts 2 parameters:


Name Details Optional


name Name of the automation No


key API key No


```text
{
status  :   ""  ,
message  :   ""  ,
data  : {  "google-sheet-data"  : [[],[]]}
}


```


Key Details Values


status The status of your automation "success""error"


message Any error messages "{error}"/{"Automation successfully triggered"}


data Data written to a Google Sheet {"google-sheet-data: \[\[\],


\]"}/null


An example of running this is included below:


```text
import   { checkStatus }   from   'axiom-ai-helper'  ;


...


const   result   =   await   checkStatus  (name, key);
console.  log  (result.message);


```


## Using the data in axiom.ai


To trigger an automation, you'll need to have an automation created. Once this has been created, set a[Receive data from another app](https://axiom.ai/docs/no-code-tool/reference/steps/receive-data-from-another-app) step as the first step, this will allow the automation to be able to receive the webhook that we are sending. In this step, ensure that you have the "Test data" filled in with the same format that you expect to receive the data, this will help with testing. For example:


` name, email`


Once this has been done, you can then access the data that you send to the automation using the` webhook-data` data token.


## Potential use cases


There are various use cases that you could make use of while using the axiom.ai API, such as:


### Contact forms


Using the package to send along the data from a webform can allow you to build completely custom contact or feedback forms. You could use the following code to send along the data:


```text
const   result   =   await   trigger  (  "Contact form"  ,   "KEY"  , [[  "Karl Jones"  ,   "example@example.com"  ,   "Hi, I'm looking to enquire about a recent order."  ]]);


```


Jump into the axiom.ai extension and create a new automation called "Contact form". As the first step, add aReceive data from another app step, with the following test data set, this will help with creating your automation:


` name, email, message`


You can use this data by accessing the` webhook-data` data token. You could then:


- Use a[Send an email](https://axiom.ai/docs/no-code-tool/reference/steps/send-an-email) step to email this data to your team.
- Store using the[Write data to a Google Sheet](https://axiom.ai/docs/no-code-tool/reference/steps/write-data-to-a-google-sheet-step) step.
- Automatically craft a response using the[Generate text with ChatGPT](https://axiom.ai/docs/no-code-tool/reference/steps/generate-text-with-chatgpt) step.


### Automating actions or track errors in your web app


Whether it's a standard web app or a mobile web app, you can use the API to automation actions or track errors within your web app. This can be helpful for various use cases, such as:


- Tracking newsletter sign ups.
- Logging errors into a Google Sheet.
- Logging attributes about users, such as browser, or language.


Other tools do offer these features, however, this may be worth a look before committing to a larger, and often more expensive, option.


## Wrapping up


Implementing the API into your projects is relatively simple, and if you have the skills or time you likely won't need to use this library - this is purely designed to be a time saver to ensure that you are meeting the requirements of the API without having to write the code yourself.


Future plans for this library will ensure that this will be kept updated with the latest developments in the axiom.ai API.


You can get started with axiom-ai-helper over on npm:[https://www.npmjs.com/package/axiom-ai-helper](https://www.npmjs.com/package/axiom-ai-helper)


> 💡 Reminder, this package was created by the author, an employee of axiom.ai, but is not an official product of axiom.ai. Issues, questions and feature requests should be directed to the author through the repository:[npmjs.com](https://www.npmjs.com/package/axiom-ai-helper) .


[Join discussion on r/axiom_ai](https://reddit.com/r/axiom_ai)


###### On this page


- Installing the package
- Usage
- Functions
- Using the data in axiom.ai
- Potential use cases
- Wrapping up


By


[Karl Jones](https://axiom.ai/authors/karl-jones) , Technical writer


Karl is a technical writer with axiom.ai with a computer science background and 10+ years of customer support experience. In his spare time he…
