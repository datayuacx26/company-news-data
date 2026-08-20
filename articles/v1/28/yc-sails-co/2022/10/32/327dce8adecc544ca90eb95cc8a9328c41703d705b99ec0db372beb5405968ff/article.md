---
schema_version: "1.0.0"
document_id: "327dce8adecc544ca90eb95cc8a9328c41703d705b99ec0db372beb5405968ff"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/setting-up-api-monitoring-with-treblle-in-sails/"
published_at: "2022-10-28T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:ea39492c91900208c3207e6b2c7138880a7bab320b882ebad82f642286d247ee"
---

# Setting up API monitoring with Treblle in Sails

[Treblle](https://treblle.com/) came on my radar on[Twitter](https://twitter.com/Dominus_Kelvin) a few weeks ago and I thought “wow this is much better than looking at my cloud provider console logs” as it provides very rich insights into production APIs.


I tried integrating Treblle in the[Sailscasts](https://sailscasts.com/) codebase and seeing the SDK for Node was originally intended for Express(and Sails is built on Express), I decided to create a Sails hook to make the integration super easy.


Let me walk you through how to set up Treblle in your Sails powered APIs using the[treblle-sails](https://github.com/DominusKelvin/treblle-sails) hook I made.


## Installation


To get started with the setup, install the` treblle-sails` hook by running the below command in your terminal:


```text
npm   i   --save   treblle-sails
```


## Getting Treblle credentials


So the` treblle-sails` hook needs you to provide your Treblle API Key and Project ID, both info you can get on your Treblle Dashboard after you create a Treblle[free account](https://treblle.com/register) .


To get your Treblle API key, click on your avatar on the Treblle Dashboard and click on` Settings` .


To get the Project ID, after creating a project, open up the project and click on the` Settings` .


## Setting up credentials


Assuming you’ve gotten both the API key and Project ID, in your` config/local.js` you can set it up like so:


```text
// config/local.js
treblle  : {
apiKey  :   '<YOUR_TREBLLE_API_KEY>'  ,
projectId  :   '<YOUR_TREBLLE_PROJECT_ID>'
}
```


Now of course you won’t be able to use this in production yet because` config/local.js` is not being pushed to deployment.


` treblle-sails` will smartly look for the following environment variables:


- ` TREBLLE_API_KEY`
- ` TREBLLE_PROJECT_ID`


So if you don’t want to set them in` config/local.js` you can skip that step and just use the environment variables mentioned above in your service provider like Render or Heroku.


And that’s it you are done. When you deploy your API, you will get rich and complete API analytics on your Treblle Dashboard amongst other useful info like auto-generated API docs.


Pretty cool right?


## Masking fields


Treblle mask user passwords by default when logging responses your API made to requests. However you can tell Treblle to also mask additional fields. To do so, create a` config/treblle.js` file and then pass in the following:


```text
// config/treblle.js
module  .  exports  .treblle   =   {
additionalFieldsToMask: [  'key1'  ,   'key2'  ],   // optional
}
```


Take note to replace` key1` and` key` with the actual field names you want masked.


## Turning off errors


Maybe you don’t want to show errors your API faced during a requests(I don’t see why you won’t want to see them but…).


You can turn of erros by using the` showErrors` property and passing false to it. So in the same` config/treblle.js` file, you can do:


```text
// config/treblle.js
module  .  exports  .treblle   =   {
showErrors:   false   // Optional, defaults to true
}
```


## Conclusion


And that’s it, enjoy rich API monitoring and insights on your production Sails API with Treblle using the` treblle-sails` hook.


I have been using Treblle with the Sailscasts API and the insights are just great. Let me know if you gave it a try or have any questions.
