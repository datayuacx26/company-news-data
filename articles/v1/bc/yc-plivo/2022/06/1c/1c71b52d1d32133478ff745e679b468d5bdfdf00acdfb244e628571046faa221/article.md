---
schema_version: "1.0.0"
document_id: "1c71b52d1d32133478ff745e679b468d5bdfdf00acdfb244e628571046faa221"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/zapier-integration-send-mms-with-zapier-and-plivo/"
published_at: "2022-06-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:4a250b0e65628a9970dd2b0e997bf43ca6cf02ac2038373168f739b4bf5d12b0"
---

# Zapier Integration - Send MMS Messages Using Plivo and Zapier

Zapier, an online automation tool that connects applications, lets you integrate Plivo with other applications to[send SMS text messages](https://www.plivo.com/docs/integrations/zapier/) after a specified event. Until recently Plivo could only send SMS messages through Zapier, but we’ve added MMS functionality, so you can now trigger messages that send audio, image, or video along with some text.


Let’s see how to use Plivo with Zapier integration to send MMS messages. For our example we’ll send an MMS message upon successful registration for an event in a Google Form.


## Step 1: Create a Google Form


Create a Google Form that lets people register for an event. The form can have several fields - name, email address, phone number, and company name. Also specify the name of a Google Sheet to hold the information that’s captured when someone submits the form.


## Step 2: Specify a trigger


Open Zapier and click **Create Zap** . Specify Google Forms as the trigger application. Choose “New Response in Spreadsheet” as the trigger event, then click **Continue** .


If you haven’t done so already, connect your Google account with Zapier, then specify the spreadsheet and worksheet that holds your form information. Click **Continue** , then test the trigger.


## Step 3: Specify an action


Once you’ve successfully specified the trigger, Zapier takes you back to the Zap setup screen, where you can specify the action to take when the trigger fires. Click on or specify Plivo, then choose Send MMS as the event and click **Continue** . If you haven’t done so already, connect your Plivo account with Zapier by copying your Auth ID and Auth Token from the[console](https://console.plivo.com/dashboard/) and pasting them into the authentication popup. Click **Continue** .


Next, set up the action by specifying values for source and destination numbers, the text to be sent in the message, and URLs for the media you want to send. The source or from number must be a US or Canada number, because the Plivo[SMS API](https://www.plivo.com/sms/) supports sending MMS messages only from those countries. Choose an MMS-enabled number from the[numbers you have registered](https://console.plivo.com/active-phone-numbers/) on the console. Fetch the destination number from the Phone Number field of your Google Form, or rather that column in the Google Sheet that holds the data. Finally, click **Continue** .


Ta-da!🎉 You’ve created a Zap to send an[MMS message](https://www.plivo.com/blog/what-is-mms-messaging/) whenever someone registers through your form. Test it out to make sure it works the way you want it to.


You can use a similar process to send MMS messages for other events, such as birthday reminders, employee anniversaries, successful sales deals, and product debuts. You can also set triggers from any application Zapier integration supports, such as Google Calendar, an email marketing application, or a social media platform.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
