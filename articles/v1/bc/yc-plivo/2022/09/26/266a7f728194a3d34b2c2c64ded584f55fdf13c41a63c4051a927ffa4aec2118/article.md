---
schema_version: "1.0.0"
document_id: "266a7f728194a3d34b2c2c64ded584f55fdf13c41a63c4051a927ffa4aec2118"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/forward-voicemail-to-email-pipedream/"
published_at: "2022-09-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:7ea99e1bce8ff7eee711eaeeb9798fb7a33370a660bc7ca3aa08d915b9bb439b"
---

# How to Forward Plivo Voicemail to Email Using Pipedream

By using Plivo and an integration platform, you can forward your voicemail messages to your email address without doing any coding. Here’s how to set up voicemail on Plivo and forward it to email using[Pipedream](https://pipedream.com/apps/plivo) .


To get started, you need a Plivo account —[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. To receive incoming messages, you must have a Plivo phone number that supports both voice and SMS; you can rent numbers from the[Numbers](https://console.plivo.com/active-phone-numbers/) page of the Plivo console or by using the[Numbers API](https://www.plivo.com/docs/numbers/) .


You also need a free Pipedream account.


#### Step 1: Create a PHLO to forward voicemail


We’ll use Plivo’s low-code visual design tool[PHLO](https://www.plivo.com/docs/phlo/) to set up voicemail and attach it to a Plivo number.


On the Plivo console, go to the PHLO page and click[Create New PHLO](https://console.plivo.com/phlo/list/) , then click **Build My Own** . Drag the Record Audio component onto the canvas, then draw a line from the Incoming Call trigger of the Start node to the Record Audio node. In the Configuration panel at the right, enter the text you want the caller to hear when the call gets connected, then click **Validate** . Click in the upper left of the page to give your PHLO a name, then click **Save** to create the PHLO.


To associate the PHLO with a phone number, go to the Phone Numbers page of the console. Click on the number for which you want to forward voicemail. In the Number Configuration, click the **Application Type** dropdown and choose PHLO. From the **PHLO Name** dropdown, choose the name of the PHLO you just created, then click **Update Number** .


#### Step 2: Configure Pipedream


Now go to your Pipedream console and create a Pipedream workflow. Click **New HTTP/Webhook Requests** , choose **HTTP Requests with a Body** , then **Save and Continue** . Pipedream will display a URL, which you need to copy.


Return to your PHLO on the Plivo console and click on the Record Audio node. Paste the URL you copied from Pipedream into the Event Callbacks box, then click **Validate** , then **Save** . This sets up the webhook to which we will post the voicemail recording details, which we’ll use when we send the email.


#### Step 3: Test the webhook


Now you’re ready to test out your integration. Make a call to your Plivo number and leave yourself a message. Pipedream will capture the HTTP response.


#### Step 4: Add email connector to receive email


Once you have the response you can set up your Pipedream workflow to forward the recording to your email. Return to the Pipedream console and click the “+” icon below the trigger pane. Click on **Send Email** . In the new pane that appears, enter Subject and Text values as shown below. Click **Test** to test the workflow, then **Deploy** to make it live.


Your integration should now be complete. Whenever someone leaves voicemail at the number you specified, Pipedream will send an email message that contains the from and to numbers and the voicemail recording to the address you used when you signed up for your Pipedream account.


### Integrations for all kinds of applications


Pipedream offers integrations for many applications, so you could use a similar process to send voicemail messages from a Plivo number to Slack, Discord, or a Google Drive.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
