---
schema_version: "1.0.0"
document_id: "025d6fd9149a0203e3ee9966daa0928cddf409a4e0df2cbf64914def2906ad1f"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-news-import-ffde4d661374"
canonical_url: "https://www.usewaypoint.com/blog/how-to-send-branded-emails-on-webflow"
published_at: null
first_seen_at: "2026-07-22T19:31:17.685303+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:7798611f8ac68c042aa8fcab7e6b52d2f1d1dfc93edb88e2fd813dffb714a400"
---

# How to send branded emails on Webflow (using Webflow Logic)

[Webflow](https://webflow.com/) is a flexible no-code platform for designing, building, and launching websites. It can power simple marketing pages to sites with much more complexity with it's built-in CMS.


One limitation, however, is when it comes to sending emails based on actions on a Webflow site (eg. form submission). Webflow's built-in email system is geared for internal collaborator notifications rather than external communication. However, with Webflow's new[Logic tool](https://university.webflow.com/lesson/webflow-logic-overview?topics=logic) , it's now possible to orchestrate this kind of email communication by connecting[email API platforms](https://www.usewaypoint.com/) .


In the example below, we'll connect Webflow to[Waypoint](https://www.usewaypoint.com/) , an email platform with a powerful no-code builder for building and sending emails.


## 1. Build the dynamic email template on Waypoint


Let's get started by building the email template we want to send. In this example, let's pretend we want to send a confirmation email to a user after they submit a form on our Webflow website.


For simplicity, we'll just send a copy of the message after a form submission. However, this same method could be used for more advanced use cases such as notifying a user after they have posted their first listing on a marketplace (learn more about[marketplace emails](https://www.usewaypoint.com/blog/transactional-emails-the-glue-of-ux-for-marketplaces) ).


To build our template, we'll use[Waypoint](https://www.usewaypoint.com/) 's block-based builder which allows us to create a template like the one below in minutes.


For a step-by-step guide on getting started on Waypoint, check out the[quick start guide](https://www.usewaypoint.com/docs/quickstart) .


Now that we have a static version of the template, let's mock some data that we'll expect to be received from a Webflow form. From the 'Data' tab, set add a JSON payload for the name, email, and message.


```text
{
"name"  :    "Jordan Isip"  ,
"email"  :    "jordan@usewaypoint.com"  ,
"message"  :    "Love your work. Any chance you are available for contract work next month? Happy to tell you more about the project."
}
```


With the test data in place, let's reference the data from the content blocks using[LiquidJS templating](https://liquidjs.com/tutorials/intro-to-liquid.html) . Eg.` {{message}}` .


Now let's fill out the rest of the template settings (eg. subject line and preheader).


## 2. Create a form on Webflow


Now let's create a form on Webflow to capture the data inputs. In this case, we'll just have a couple text fields for the name and email address, a textarea for the message, and a submit button.


We'll also want to add a name in the form block settings that we'll reference in the next step (eg. 'Email form').


For more information on building forms, head to the[Webflow University](https://university.webflow.com/lesson/intro-to-forms) .


## 3. Setup Webflow Logic to send emails


With a form in place, we'll now want to setup an automated workflow so that the when someone submits our form, it will pass data to Waypoint to send the templated email that we setup in step 1.


For this automation, we'll use[Webflow's Logic](https://university.webflow.com/lesson/webflow-logic-overview?topics=logic) tool.


### Create Logic Flow


First, switch to the Logic tab on the Webflow builder.


Click on "+ New flow" and give it a name. Eg. "Email on Waypoint".


### Create Flow trigger


Select a trigger and click 'Form submission'.


In the trigger settings dropdown, select the form that we had created in step 2 (eg. 'Email form').


### Create Flow action


Now that we have a trigger setup on our form, we'll want to create an action in the Flow to send the email.


While there is a built-in action to 'Send email notification' through Webflow, this option won't work for us since it can only be sent to **internal project collaborators** (also it can only send plain text emails).


So in our case, we'll connect Webflow to our Waypoint template by using an 'HTTP request' action (learn more about[HTTP requests on Webflow Logic](https://university.webflow.com/lesson/make-http-requests-with-logic?topics=logic) ).


Now let's fill out the HTTP request form to point to the[Waypoint API](https://www.usewaypoint.com/) .


1.


Give the action a 'block name' (eg. 'Email on Waypoint')


2.


For Authentication, we'll select "Username & password"


3.


For Credential, we'll add our Waypoint API key ([learn more](https://www.usewaypoint.com/) ).


4.


For request method: use "POST"


5.


For the URL, we'll use the API endpoint url for sending templated messages "https://live.waypointapi.com/v1/email_messages"


6.


Add the header: "Content Type" : "application/json"


7.


For the body, we'll use the JSON payload below and map Webflow data to the variables (hint: click the dot/plus on the top left of the field to add a dynamic variable).


```text
{
"templateId"  :    "wptemplate_vSPeSv4pNRF5YaBK"  ,
"to"  :    "jordan@usewaypoint.com"  ,
"variables"  :    {
"email"  :    WEBFLOW_VARIABLE_EMAIL  ,
"name"  :    WEBFLOW_VARIABLE_NAME  ,
"message"  :    WEBFLOW_VARIABLE_MESSAGE
}
}


```


Click "Run test to complete setup" and input test data. Eg.


-


Message: "Hello from Webflow"


-


Name: Jordan Isip


-


Email: jordan@usewaypoint.com


Click 'Run test'. Assuming everything was setup correctly, the test will show as successful.


Click 'Apply data' to commit the data scheme to the Flow.


Behind the scenes, that test request is sent through the Waypoint API, the data variables are passed through the template we created, and the resulting email is sent and delivered.


From the Waypoint dashboard, we can also see a log of the message sent along with the preview, template data, metadata, and full timeline of delivery events.


## 4. Deploy form and test


At this point, we can publish the form on our Webflow site to give it a go with live data.


Now when users fill out this form, they will receive the confirmation email that we had setup.


Additionally, we can monitor all of the sent emails and it's deliverability metrics from Waypoint's dashboard ([learn more](https://www.usewaypoint.com/) ).


## Wrap up


Since emails can be seen as the '[glue of UX](https://52weeksofux.com/post/780958988/email-the-glue-of-ux) ', Waypoint with Webflow (website builder, CMS, and Logic tool) can be a powerful combination for an end-to-end experience for users.


If you want to build your own email templates, get started on Waypoint today with a[free trial workspace](https://www.usewaypoint.com/pricing) .


Further reading:


-


[Waypoint overview](https://www.usewaypoint.com/)


-


[Waypoint quick start guide](https://www.usewaypoint.com/)


-


[Webflow University](https://university.webflow.com/)


-


[Email template gallery](https://www.usewaypoint.com/templates)
