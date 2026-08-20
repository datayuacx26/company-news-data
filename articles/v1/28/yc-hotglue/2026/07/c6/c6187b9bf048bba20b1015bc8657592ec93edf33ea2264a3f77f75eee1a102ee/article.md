---
schema_version: "1.0.0"
document_id: "c6187b9bf048bba20b1015bc8657592ec93edf33ea2264a3f77f75eee1a102ee"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/how-to-build-a-public-shopify-app"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:350c79152b3a9dd2081d57e14e03fbc786b8194782a9757e59e44c5864899287"
---

# How to build a public Shopify app

**TL;DR** : Building Shopify apps is a very open ended process that can be confusing for developers and painful to maintain. hotglue simplifies the entire process — from OAuth to syncing data.


# What is a Shopify app?


Shopify enables third-party developers to extend the functionality of Shopify through **apps** . There are currently three different kinds of Shopify apps according to Shopify’s developer docs:


- perhaps the most popular are Shopify apps that simply connect with Shopify’s web APIs to read and write data
- other apps add features inside parts of the Shopify admin portal
- and finally there are apps which update the way stores display information to customers


This article focuses on the first type of Shopify app which connects to Shopify’s REST and GraphQL APIs to read and write data.


# How to build a Shopify app?


To start building a Shopify app, you must register for a Shopify Partner account, which you can do for free here:[https://www.shopify.com/partners](https://www.shopify.com/partners)


Once you have your partner account configured, you can create your first app!


When you initialize a new app in the Shopify partner portal, you will be provisioned a` client_id` and` client_secret` pair. These are used to authenticate with Shopify’s APIs. Note that within the app settings, you can also request access to certain “restricted” scopes such as the ability to read all order history via the Shopify API.


When you enter your app configuration, you will see you need to set an **app name** , **app url** , and **redirection url** .


The app url is where Shopify will redirect users who want to install your app – users will be able to do this from within the Shopify app store.


Shopify[has a guide](https://shopify.dev/docs/apps/build/authentication-authorization/access-tokens/authorization-code-grant) on how to manually implement this app install request + authorization process, as well as a CLI and several templates for creating apps.


For example, their Shopify Node template linked below gives you a great scaffold to build your first app and eventually deploy it to[Heroku](https://www.heroku.com/) or[Fly.io](https://fly.io/) . This template provides sample functionality for the following out-of-the-box:


-


OAuth: Installing the app and granting permissions


-


GraphQL Admin API: Querying or mutating Shopify admin data


-


REST Admin API: Resource classes to interact with the API


-


Shopify-specific tooling:


- AppBridge (used for embedding your app inside of Shopify)
- Polaris
- Webhooks


Check out the[shopify-app-template-node repo on GitHub](https://github.com/Shopify/shopify-app-template-node) .


# How does a hotglue-powered Shopify app work?


If your use-case for a Shopify app centers around reading and writing via Shopify’s APIs rather than embedding your app in the Shopify UI, hotglue provides a super simple way to go from 0 to a fully functional app.


Watch a walk-through here:


## Breaking it down


There are two distinct ways to have hotglue manage your Shopify App. In both options, hotglue manages the OAuth process with Shopify, collects the` access_token` on your behalf, and saves it to a` tenant` in hotglue so you can run jobs to sync data.


### **Fully Managed**


With this method, your app url will point to a hotglue URL, which will receive the initial redirect from Shopify on your behalf. This is the easiest to configure.


` https://client-api.hotglue.xyz/shopify/{env_id}/{flow_id}


`


When Shopify sends a user to this URL, hotglue will:


1. Generate a tenant ID based on the shop name. For example, the shop` hotglue-testing-1.myshopify.com` will get a tenant id like` hotglue-testing-1_EfGrt`
2. Trigger the OAuth flow, prompting the tenant to grant your app access to their shopExchange the final` code` for an` access_token` and save this to the tenant in hotglue
3. Redirect the user back to your app (configured via the` complete_url` you define in hotglue)


### **Semi Managed**


With this method, you will set a custom app URL (to your webapp for example), which will then “forward” the request to hotglue.


The advantage to this method is that you receive the initial installation request, allowing you to inject your own logic – for example, prompting a new user to register for an account. With this method, you can supply the desired` tenant` id in the query params. Here’s a simplified example using Express to illustrate how the semi-managed flow works:


` app


.


get


(


'/app'


,


async


(


req


,


res


,


next


)


=>


{


// Get the query params


const


{


shop


}


=


req


.


query


;


// TODO: write some logic to decide what the tenant id should be


const


tenantId


=


"magic-ruffalo-123"


;


// Redirect the tenant to complete the Shopify OAuth process (notice we are forwarding the shop and tenant query params)


const


url


=


\`


${


HG_API_URL


}


/shopify/


${


HG_ENV_ID


}


/


${


HG_FLOW_ID


}


?shop=


${


shop


}


&tenant=


${


tenantId


}


\`


console


.


log


(


"Redirecting to: "


,


url


)


;


res


.


redirect


(


302


,


url


)


;


}


)


;


`


To recap, the steps in the semi-managed flow are:


1. Shopify sends the user to your configured app URL
2. Your custom logic runs, generates a` tenant` id, and redirects the user to the hotglue URL with the query params Shopify provided (` hmac` ,` shop` )
3. hotglue triggers the OAuth flow, same as in the fully managed flow, collects the` access_token` , and saves it to the tenant in hotglue
4. hotglue redirects the user back to your app (configured via the` complete_url` you define in hotglue)


To learn more about how to use hotglue to power your Shopify app,[read the docs](https://docs.hotglue.com/connectors/shopify#public-app-oauth) or[book a demo](https://hotglue.com/demo) .


## Read + write data


Once the tenant is created, hotglue allows you to sync data to and from Shopify via jobs. Jobs can be scheduled using cron expressions or triggered ad-hoc via the hotglue API or hotglue widget.


Jobs can both read and write data, and are highly scalable – designed to handle large amounts of data without issues. The hotglue admin panel allows you to manage all your users sync jobs, with built in logging and webhook functionality.


hotglue also includes a preprocessing layer based on Python. You can set a transformation script that processes the raw data from Shopify, and can even customize it on a user level.


Learn more about how transformation scripts work in hotglue in the docs:[https://docs.hotglue.com/docs/transformations-overview](https://docs.hotglue.com/docs/transformations-overview)


# Conclusion


Offering a public Shopify app can be a great way to:


- simplify the auth process for users – offer a “one-click” experience to connect their store to your product vs setting up a custom app and dealing with configuring API permissions
- expand visibility of your product – being listed in the Shopify App Store can become a source of leads and make your product easier to discover, especially if eCommerce stores are your core ICP. Being listed here also boosts your SEO, brand credibility, and gives users an opportunity to leave reviews of your product!


Leading eCommerce SaaS businesses like[Inventoro by Cin7](https://hotglue.com/case-studies/inventoro) already use hotglue to power their Shopify apps. If your Shopify integration centers around reading and writing data (for example, analyzing inventory levels or order history), using hotglue makes it fast and easy to launch a public Shopify app.


If you’re looking to build an app that embeds directly in Shopify, the official Shopify templates and SDKs are a great place to start!


To learn more, book a demo here:[https://hotglue.com/demo](https://hotglue.com/demo)


TABLE OF CONTENTS


- What is a Shopify app?
- How to build a Shopify app?
- How does a hotglue-powered Shopify app work?
- Breaking it down
- Read + write data
- Conclusion


RECOMMENDED BLOGS


[QuickBooks New API Fees Explained + How to Monitor & Minimize Usage Starting July 28, 2025, QuickBooks is launching its new QuickBooks App Partner Program, which introduces paid tiers and API fees. Without careful monitoring, this new pricing model can get expensive fast.](https://hotglue.com/blog/how-to-lower-your-coreplus-quickbooks-api-usage)


[hotglue melt: May 2025 Universal external id support, connector builder updates, subtenant symlinks, and more!](https://hotglue.com/blog/hotglue-melt-may-2025)


[hotglue melt: April 2025 Connector Builder, real-time write, streaming read jobs, accounting unified schema v2, and more!](https://hotglue.com/blog/hotglue-melt-april-2025)
