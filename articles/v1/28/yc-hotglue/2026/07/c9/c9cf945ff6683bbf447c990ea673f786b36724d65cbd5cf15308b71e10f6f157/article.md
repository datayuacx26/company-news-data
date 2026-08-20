---
schema_version: "1.0.0"
document_id: "c9cf945ff6683bbf447c990ea673f786b36724d65cbd5cf15308b71e10f6f157"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/custom-mapping-for-crm-integrations-why-its-better-and-how-to-do-it"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:600c000e731a37137b15475254a1c32d594b3bd1b4e582fc8db4cce01cd63305"
---

# Custom Mapping for CRM Integrations: Why It’s Better and How to Do It

In one of my last blog posts, I walked through[why Unified APIs don’t work for CRM integrations](https://hotglue.com/blog/why-unified-apis-dont-work-for-crm-integrations) – in short, CRMs revolve around **custom data** , and any CRM integration needs to handle that custom data in order to be useful. Note that when I am referring to custom data, I’m talking about user-added custom fields and objects. For example, users may have a custom “Closed Lost Category” field on a Deal in HubSpot, or a custom “Order” object in Salesforce.


This is where **custom mapping** becomes extremely useful: rather than having your customer success team dive into how a users CRM is set up and determining which objects/fields are relevant to the integration, you empower the users to configure all of this themselves. By doing this you can significantly reduce onboarding time, while offloading a lot of the work to your user.


Here’s a quick demo of how a **custom mapping** experience with Salesforce could work:


# How to implement custom mapping


If you’re looking to build this whole workflow in-house, there are three steps you need to implement.


## 1. Authorization


The first step is to allow your user to connect their CRM by implementing an OAuth flow. This prompts the user to log in with their Salesforce/HubSpot credentials and grant your application access to their data. In the demo video above, I showed how this works for Salesforce.


Generally, implementing OAuth involves three steps:


1.


Register an OAuth app with the CRM (one-time setup)


Both Salesforce and HubSpot allow you to register for a free developer account to[configure an OAuth app](https://docs.hotglue.com/connectors/salesforce#credentials-setup) . This process involves giving your app a name, logo, setting a **redirect url,** and optionally defining the relevant permissions your app needs. When you do this you will receive a **client id** and **client secret.**


2.


Send the user to the authorization url


Now that you have an OAuth app registered, you can construct what’s called an authorization url. The structure of this url varies slightly based on the CRM you’re integrating with, but generally they look something like:


` ⁠https://login.salesforce.com/services/oauth2/authorize?response_type=code&client_id=<CLIENT ID>&redirect_uri=<REDIRECT URI>&state=<OPTIONAL STATE VAR>`
This is the URL you will send your user to when they want to link their CRM – it will automatically prompt them to log in with their Salesforce/HubSpot credentials + grant access to your app, and then redirect them back to the specified **redirect_uri** that you configured in your OAuth app settings.


3. Exchange the OAuth code for an` access_token` /` refresh_token` pair


When the CRM redirects them back to the **redirect_uri** , it will include two query params in the URL: a` code` and the same` state` that you provided when you sent the user to the original authorization url.


The` code` can then be exchanged for an` access_token` /` refresh_token` pair on your backend using what’s called a **token** endpoint. For Salesforce, this looks something like:


` POST


<


https:


//login.salesforce.com/services/oauth2/token>


{


"client_id"


:


<


CLIENT ID


>


,


"client_secret': <CLIENT SECRET>,


"


grant_type


': "authorization_code",


"redirect_uri'


:


<


REDIRECT URI


>


,


"code':


<


CODE


FROM


QUERY PARAMS


>


}`


You can then use the resulting` access_token` to make requests to the CRM’s API on behalf of your user!


## 2. Discovery: find what custom objects and fields are available


Now that you have access to the CRM API, you need to make requests to determine what objects are available in your users account, and what fields are available in each object. In hotglue, we call this the **discover** step.


The implementation of how this works will have to vary for each CRM based on how their APIs work. For Salesforce, they have two endpoints responsible for this:


1. [List Objects](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_describeGlobal.htm) → this returns a list of every object available in the Salesforce account (including custom objects)
2. [Describe Object](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_sobject_describe.htm) → this will allow you to query the available fields for a specific object (including custom fields)


In the demo above, these requests are made immediately after the user links their CRM in the background. As soon as the discover process is done and we have loaded all the available objects/fields, we are ready to start mapping!


## 3. Mapping


To allow your user to map their CRM data to your product, you will want to give them a simple UI where they can see what objects they need to map, along with what fields they need to map in each field. In the demo above, we prompted the user to map the **Customer** object, which had the fields: ID, First Name, Last Name, Email, Created At, and Updated At.


You want to allow the user to select which object in their CRM corresponds to this data, and then map the relevant fields as desired.


Once the user defines the mapping in your product, you can now consume that mapping in your backend when you interact with data from the CRM. In the demo we did this both when reading and writing data: when we read **Contact** data from Salesforce, we automatically converted that into a **Customer** object with all the relevant fields. When we wanted to write an update to a **Customer** back to Salesforce, we transformed it to a **Contact** first.


# How hotglue simplifies it


With hotglue, the setup of a CRM integration with custom mapping is considerably easier. Let’s walk through the same steps:


## 1. Authorization


hotglue comes with embeddable front-end components that handle every step of the authorization process out of the box, from sending the user to the authorization url, to exchanging the` code` for an` access_token` /` refresh_token` pair. The code to do this is simple:


` // launch the OAuth workflow


HotGlue.link( "<TENANT ID>", // the ID of your user "<FLOW ID>", // your hotglue flow ID "salesforce" // the ID of the connector to link, in this case Salesforce );`


Once this` link` function is called and the user completes the OAuth workflow, a new **tenant** is created in hotglue with the Salesforce connector linked. This makes standing up authorization very simple – all you need to do is register for an OAuth app and save your` client_id` /` client_secret` pair in the hotglue admin panel.


## 2. Discovery


Every hotglue connector implements discovery out of the box and generates a **catalog** , which is a JSON object defining all the available objects + fields from the CRM. This catalog follows the standard[Singer.io format](https://github.com/singer-io/getting-started/blob/master/docs/DISCOVERY_MODE.md#schemas) . Here’s a snippet from a Salesforce catalog:


` {


"streams"


:


\[


{


"stream"


:


"Contact"


,


"tap_stream_id"


:


"Contact"


,


"schema"


:


{


"type"


:


"object"


,


"additionalProperties"


:


false


,


"properties"


:


{


"Id"


:


{


"type"


:


"string"


}


,


"AccountId"


:


{


"type"


:


\[


"null"


,


"string"


\]


}


,


"LastName"


:


{


"type"


:


\[


"null"


,


"string"


\]


}


,


"FirstName"


:


{


"type"


:


\[


"null"


,


"string"


\]


}


,


"About_This_Implementer__c"


:


{


"type"


:


\[


"null"


,


"string"


\]


}


,


"Account_Stage__c"


:


{


"type"


:


\[


"null"


,


"string"


\]


}


,


...


// more fields...


}


,


"metadata"


:


\[


...


\]


}


,


...


// more objects...


}


\]


}


`


Since hotglue implements this same format across all connectors, it makes it very easy to use across different CRMs – this data is also accessible via API in case you’re interested in implementing the mapping piece in-house.


## 3. Mapping


Finally, you can prompt the user to map their objects after link using the same frontend components mentioned in step one:


` // NOTE: this example is using React hooks


const


{


setOptions


,


setSchema


}


=


useHotglue


(


)


;


// the below indicates we want the user to begin the mapping process immediately


// after link


setOptions


(


{


nextStep


:


"mapping"


}


)


;


// the below defines what we want the user to map. in this case, we are prompting


// them to map a Salesforce object to the Customer object


setSchema


(


\[


{


flowId


:


HOTGLUE_DEMO_FLOW_ID


,


schema


:


\[


{


table


:


'Customer'


,


fields


:


\[


{


id


:


'id'


,


name


:


'ID'


,


}


,


{


id


:


'first_name'


,


name


:


'First Name'


,


}


,


{


id


:


'last_name'


,


name


:


'Last Name'


,


}


,


{


id


:


'email'


,


name


:


'Email'


,


}


,


{


id


:


'created_at'


,


name


:


'Created At'


}


,


{


id


:


'updated_at'


,


name


:


'Updated At'


}


\]


,


}


,


\]


,


}


,


\]


)


;


`


That’s it! Just like that you can implement the entire mapping workflow we outlined above.


The best part is that hotglue handles consuming this mapping out of the box – meaning when you read or write data, hotglue’s built-in transformation layer will manage converting the raw data for you. For example, you can ask hotglue to write a **Customer** to Salesforce (just like we did in the demo above) and it will automatically convert that to a valid **Contact** object in Salesforce using the user-defined mapping.


# Conclusion


By allowing users to fully leverage their custom objects + fields, custom mapping can make your CRM integrations 10x more powerful. The best part is allowing users to control how data is being mapped between your product and their CRM can speed up onboarding times and decrease load on your customer success team.


There is a lot more to explore with custom mapping for CRM integrations, such as how **external IDs** can be used to create related objects, but we’ll save that for another blog post! Thanks for reading :)


If you’re interested in learning more about hotglue, you can book a demo[here](https://hotglue.com/demo) .


TABLE OF CONTENTS


- How to implement custom mapping
- 1. Authorization
- 2. Discovery: find what custom objects and fields are available
- 3. Mapping
- How hotglue simplifies it
- 1. Authorization
- 2. Discovery
- 3. Mapping
- Conclusion


RECOMMENDED BLOGS


[Introducing Magic Link Imagine if you could just send a link to your users to connect all their integrations… now you can using our new Magic Link feature 🪄](https://hotglue.com/blog/introducing-magic-link)


[Announcing hotglue’s $4M Seed Round Announcing our $4 million seed round led by 8VC with participation from Y Combinator, Correlation Ventures, and others.](https://hotglue.com/blog/announcing-hotglues-4m-seed-round)


[The Stigma of Embedded iPaaS Too many embedded iPaaS tools underperform, creating skepticism in the market. The negative stigma isn't impacting hotglue. Read on to learn why.](https://hotglue.com/blog/the-stigma-of-embedded-ipaas)
