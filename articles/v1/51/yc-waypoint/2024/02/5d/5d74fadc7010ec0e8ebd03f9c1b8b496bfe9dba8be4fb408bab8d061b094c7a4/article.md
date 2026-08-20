---
schema_version: "1.0.0"
document_id: "5d74fadc7010ec0e8ebd03f9c1b8b496bfe9dba8be4fb408bab8d061b094c7a4"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-news-import-ffde4d661374"
canonical_url: "https://www.usewaypoint.com/blog/how-to-send-branded-emails-on-tangram"
published_at: "2024-02-19T00:00:00+00:00"
first_seen_at: "2026-07-22T19:31:17.685303+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:cd4a40b44a494387fccd9f42130ba66fd56c4a2bee12deb4b009531b69f077c5"
---

# How to send data-rich marketplace emails on Tangram

[Tangram](https://www.tangram.co/) is a flexible no-code platform for designing, building, and hosting marketplace applications – from service marketplaces like Airbnb to product marketplaces like Etsy. Tangram offers a wide set of templates and integrations that allow builders to create an MVP marketplace in 1-2 weeks instead of months (and 100x cheaper than custom development).


Since marketplaces are transactional by nature, emails are often the['glue of UX' for marketplaces](https://www.usewaypoint.com/blog/transactional-emails-the-glue-of-ux-for-marketplaces) . While Tangram has a built-in set of email notifications, teams may want more flexibility around email design and sending. Luckily, this can easily be done by taking advantage of Tangram's built-in 'webhooks' and connecting them to Waypoint's[transactional email platform](https://www.usewaypoint.com/) .


Below is a step-by-step guide on setting up an integration between Tangram and Waypoint. If you haven't created an account yet, be fore to head to[Tangram](https://www.tangram.co/) and[Waypoint](https://www.usewaypoint.com/) first.


## 1. Build the dynamic email template on Waypoint


In this example, we'll pretend that we run a marketplace connecting teachers and students and want to send a confirmation email to the student after they purchase a class.


To build our template, we'll use[Waypoint](https://www.usewaypoint.com/) 's block-based builder which allows us to create a template like the one below in minutes. For a step-by-step guide on getting started on Waypoint, check out the[quick start guide](https://www.usewaypoint.com/docs/quickstart) .


For now, we'll create the static version of this template and then hook up dynamic data in the following steps.


## 2. Setup webhook URL on Waypoint


Before we can hook up dynamic data on the template, we want to see what kind of data we're working with from Tangram. To do this, we'll setup a URL that Tangram can send data to – we can do this with Waypoint's workflows.


From your workspace dashboard, click on "Workflows" and then "New":


Within the new workflow:


1.


Set the trigger to 'HTTP Post'. This will give us the 'webhook URL' that we'll be able to use.


2.


Copy the URL to the clipboard.


## 3. Setup Webhook on Tangram


Now that we have the webhook URL, let's head over to our admin dashboard on Tangram and go to our Webhooks settings:


1.


Click on the settings floating icon on the bottom left.


2.


Click on the 'Settings' link on the sidebar.


3.


Click on the 'Webhooks' tab.


From the Webhooks tab, click 'New' and fill out the form with the following:


-


Trigger this webhook for: **Transactions**


-


Label: **New transaction**


-


Webhook URL: **\[paste the webhook URL from Waypoint\]**


-


Trigger this webhook when the following condition is met: **Transaction created**


-


For the following listing type: **Services**


With this webhook setup, this will trigger our Waypoint workflow every time a new transaction (purchase) on our service (class) has been created.


Click 'Save' to finish creating the webhook on Tangram.


Now, let's go back to the Webhooks tab and on the row for our new webhook, click on the '…' button and then 'Send Test Payload'.


Back on our Waypoint workflow, click on the refresh data icon. If everything is connected, we'll see a sample set of data for us to work with.


This data should look something like this:


```text
{
"event"  :    "transaction_created"  ,
"created_at"  :    "2023-11-09 16:14:07 UTC"  ,
"id"  :    "tyzeep6iaa"  ,
"customer_name"  :    "Paris Mielke"  ,
"customer_email"  :    "paris@tangram.co"  ,
"customer_phone_number"  :    "1234567890"  ,
"listing_creator_name"  :    "Jane's Healthy Bites"  ,
"listing_creator_email"  :    "test+123@tangram.co"  ,
"listing_creator_phone_number"  :    ""  ,
"listing_name"  :    "Organic Smoothie Blend Class"  ,
"listing_id"  :    "whiteboard-cross-media-networks"  ,
"listing_description"  :    "Experience the difference with 'Transformive Web Design Development Experience', the innovative solution for your online presence. Our service aims to exceed your expectations by creating visually stunning, fully interactive, and optimized websites that drive business growth. By combining cutting-edge design techniques and state-of-the-art development tools, we transform your ideas into digital realities. Get ready for a transformative evolution of your online presence with our unparalleled web design development service."  ,
"price"  :    "9.99"  ,
"promo_id"  :    null  ,
"promo_amount"  :    0  ,
"promo_code"  :    null  ,
"raw_promo_code"  :    null  ,
"total"  :    "9.99"  ,
"quantity"  :    "1"  ,
"start_time"  :    "2023-11-24 22:30:00 UTC"  ,
"end_time"  :    ""  ,
"custom_fields"  :    [  ]  ,
"location"  :    "see in app at https://servicexclone.tangram.co/tx_instances/tyzeep6iaa"  ,
"airtable_record_id"  :    ""  ,
"listing_airtable_record_id"  :    ""  ,
"link"  :    "https://servicexclone.tangram.co/tx_instances/tyzeep6iaa"  ,
"listing"  :    {
"event"  :    "transaction_created"  ,
"created_at"  :    "2023-10-18 17:14:52 UTC"  ,
"id"  :    "whiteboard-cross-media-networks"  ,
"price"  :    "9.99"  ,
"listing_name"  :    "Organic Smoothie Blend Class"  ,
"listing_id"  :    "whiteboard-cross-media-networks"  ,
"creator_name"  :    "Jane's Healthy Bites"  ,
"creator_email"  :    "test+123@tangram.co"  ,
"creator_phone_number"  :    ""  ,
"creator_id"  :    "jetta-terry"  ,
"Price"  :    "9.99"  ,
"Physical Location"  :    ""  ,
"Meeting Link (if virtual)"  :    "https://meet.google.com"  ,
"Description"  :    "A mix of organic fruits and vegetables for a healthy morning boost."  ,
"Title"  :    "Organic Smoothie Blend Class"  ,
"Category"  :    "Food/Nutrition"
}
}
```


## 4. Setup dynamic variables on the template


Now that we have a a test data payload to work with within our workflow, let's copy and paste this into the 'Data' tab on our template that we created in Step 1.


With the test data in place, we can now reference the data from the content blocks using[LiquidJS templating](https://liquidjs.com/tutorials/intro-to-liquid.html) (eg.` {{listing_name}}` ) and see a live preview of the referenced variables.


Similarly, we can fill out the rest of the template settings (eg. subject line and preheader) on the General tab while referencing variables.


## 5. Link Waypoint template to workflow and test


Now that we have our dynamic template in place, we can link this template to be the action on our workflow.


From our workflow in Step 2:


1.


Set the template (from Step 1 and Step 3).


2.


Set the 'To:' address to the customer email using Liquid templating. \`{{customer_email}}\`. Note: you may want to set this to your email address while testing.


3.


Click 'Run workflow with test data'.


Running the workflow outputs the workflow run results:


And sends the actual email through Waypoint:


## 6. Monitor emails on Waypoint


From the Waypoint dashboard, we can also see a log of the message sent along with the preview, template data, metadata, and full timeline of delivery events.


Additionally, we can monitor all of the sent emails and it's deliverability metrics from Waypoint's dashboard ([learn more](https://www.usewaypoint.com/) ).


##
Wrap up


Since[email notifications play a critical role within marketplaces](https://www.usewaypoint.com/blog/transactional-emails-the-glue-of-ux-for-marketplaces) , by using Waypoint with Tangram, marketplace builders can provide an even better experience to their users.


If you want to build your own email templates, get started on Waypoint today with a[free trial workspace](https://www.usewaypoint.com/pricing) .


Further reading:


-


[Waypoint overview](https://www.usewaypoint.com/)


-


[Waypoint quick start guide](https://www.usewaypoint.com/)


-


[Tangram homepage](https://www.tangram.co/)


-


[Tangram tutorials](https://www.tangram.co/tutorial-marketplace)
