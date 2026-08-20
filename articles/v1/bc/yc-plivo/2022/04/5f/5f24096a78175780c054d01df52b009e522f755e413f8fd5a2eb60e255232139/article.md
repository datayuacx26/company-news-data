---
schema_version: "1.0.0"
document_id: "5f24096a78175780c054d01df52b009e522f755e413f8fd5a2eb60e255232139"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-integrate-plivo-tasks-into-tray-workflow/"
published_at: "2022-04-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:fd96fd75c572d89cb5845dd6752750156b3b2fb0709937d788067140d5a16cee"
---

# How to Integrate Plivo Tasks into a Tray Workflow

Tray is an application integration platform that lets users connect applications to create complex workflows. It[supports Plivo](https://tray.io/documentation/connectors/service/plivo/) , so you can create workflows that trigger Plivo to send an SMS or MMS message or make a voice call and speak text.


To start using Plivo with Tray, you need a[Tray account](https://tray.io/lp/get/get-started) - sign up for a free trial - and a voice- or SMS-enabled Plivo phone number. You can purchase numbers from the[Phone Numbers](https://console.plivo.com/active-phone-numbers/) page of the Plivo console, or by using the[Phone Number API](https://www.plivo.com/docs/numbers/) .


## A sample Plivo/Tray integration


To see how Tray works, we’ll create a workflow that integrates Plivo and Salesforce. In this workflow, every time a Salesforce Opportunity record is updated, a trigger gets fired. If the change that fired the trigger was to update the stage name to “Closed Won,” then Tray will call Plivo to send an MMS message with the opportunity details.


To get started, open Tray’s Workflow Editor and click **New workflow** . Give your workflow a name (we used Salesforce Opp Trigger), then click **Next** . On the next screen you can choose how the integration gets triggered. Tray offers eight options, or you can choose an external application. That’s what we need - choose Salesforce, then click **Create workflow** .


- Click on the Salesforce trigger to configure it. At the top of the properties panel at the right of the screen select “On Record Update” as the action.
- Click **Describe** and rename the tool Salesforce Opp Trigger.
- Click **Authenticate** and enter your Salesforce credentials.
- Click **Input data** , and from the Record type drop-down, select “Opportunity.”


With that, we’ve told Tray to trigger the workflow every time a Salesforce Opportunity record is updated.


When the workflow is triggered, we want to check whether the new value of the Stage field is “Closed Won,” and if it is, we want to take another action - namely, send a text message.


To check the value, click the plus sign under the connector to add another Salesforce service connector, but this time set the action at the top of the properties panel to “Find records.”


- In the properties panel, click **Describe** and rename the tool Get Opportunity Details.
- Click **Authenticate** and enter your Salesforce credentials.
- Click **Input Data** , and from the Record type drop-down, select “Opportunity.”
- Under Fields, click **Add to Fields** and add the Name, Account ID, Stage, Amount, and Close Date fields. We’re retrieving more than just the Stage field because we plan to use the values of the other fields in our future text message.
- Now we specify that we want these field values only from the record with the Opportunity ID that triggered the workflow. Under Conditions, click **Add to Conditions** . A new condition will appear. Set Field to Opportunity ID and Operator to Equal to. For Value, click on the datatype dropdown and choose jsonpath. Then click in the field to pick the ID, which then shows up in the box as $.steps.trigger.events\[0\].Id. Alternatively, you can drag the mouse from the Value setting to the Salesforce Opp Trigger icon and select the ID field there.


Now we can check whether the Stage is Closed Won.


Click the plus sign again to add another connector at the bottom, and this time choose Boolean under Logic Tools. Click **Boolean Condition** .


- In the properties panel, click **Describe** and rename the tool Check If Closed Won.
- Click **Input data** and fill in some required values.
- For 1st Value field, click the datatype drop-down and choose jsonpath. Then click in the field and choose Stage, which will show up as $.steps.salesforce-1.records\[0\].StageName. Alternatively, you can drag the mouse from the 1st Value setting to the previous step in the workflow and select the stage name field there.
- For Comparison type, use Contains.
- For 2nd Value, enter “Closed Won” (without the quotation marks).


If the boolean test returns false, meaning it’s not a Closed Won, we don’t have to do anything. But if the test returns true, we have to retrieve the account name from the Account record so we can include it with the other information we already retrieved in our text message.


Click the empty box in the True branch and add another Salesforce service connector.


- In the properties panel, click **Describe** and rename the tool Get Account Details.
- Set the action at the top of the configuration pane to “Find records.”
- Set Record type to “Account.”
- Under Fields, click **Add to Fields** and add the Account Name field.
- Under Conditions, click **Add to Conditions** . A new condition will appear. Set Field to Account ID, Operator to Equal to, and Value to $.steps.salesforce-1.records\[0\].AccountId. Alternatively, you can drag the mouse from the Value setting to the previous step in the workflow and select the stage name field there. This gives us access to the Account Name of the Account record whose ID matches the Account ID we retrieved earlier, which we’ll use in the text message we plan to send.


Now we have all the information we need to send a text message. Click the plus sign under the previous connector and choose the Plivo service connector.


- In the properties panel, click **Describe** and rename the tool Celebrate Using Plivo.
- Set the action at the top of the properties panel to “Send an MMS.”
- Click **Authenticate** , then enter your Plivo authentication information. You can find your Plivo Auth ID and Auth Token on the overview page of the[Plivo console](https://console.plivo.com/dashboard/) .
- Click Input data. For Source, enter the number you want to be the caller ID for the text message. Set Destination to be the phone number you want to receive the text message. Set Text to: New opportunity closed won, let’s celebrate! Account Name: {$.steps.salesforce-2.records\[0\].Name} Owner Name: {$.steps.salesforce-1.records\[0\].Opportunity_Owner_Name__c} Closed Date: {$.steps.salesforce-1.records\[0\].CloseDate} To enter the variable names, begin typing {$ and you’ll see a list of values from which to select. Click Add to Media URLs. Set Media URL to “” which is the address of a happy GIF file. If you left this off, you could send an SMS message instead of[MMS](https://www.plivo.com/blog/what-is-mms-messaging/) ; the process is otherwise the same.


Your workflow is now complete. **Click Enable workflow** at the bottom of the page, then test your configuration by marking an opportunity in a Salesforce Opportunity record Closed Won and checking the device associated with the destination phone number for the message you’re expecting.


**Note:** If you’re using a Plivo Trial account, you can make calls only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
