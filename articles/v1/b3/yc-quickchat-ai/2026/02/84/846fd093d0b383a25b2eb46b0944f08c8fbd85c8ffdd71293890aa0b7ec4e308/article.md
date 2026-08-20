---
schema_version: "1.0.0"
document_id: "846fd093d0b383a25b2eb46b0944f08c8fbd85c8ffdd71293890aa0b7ec4e308"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/hubspot-whatsapp-template-outreach"
published_at: "2026-02-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T22:19:38.682305+00:00"
content_hash: "sha256:311e3bd96b7330579eb9d4113a557536ad85607a8af703b259131ae2b8c4376a"
---

# Automating WhatsApp Outreach with HubSpot and WhatsApp Templates

In our[HubSpot integration guide](https://quickchat.ai/post/create-ai-chatbot-for-hubspot) , we covered connecting an AI Agent to HubSpot live chat. In the[HubSpot AI Actions post](https://quickchat.ai/post/connect-ai-agent-to-hubspot) , we showed how the AI Agent can create contacts, deals, and tickets during conversations.


This post covers a different workflow: automatically sending a WhatsApp marketing template to a HubSpot contact, either the moment they are created or when you flag an existing contact for outreach. The trigger is a HubSpot webhook, not a conversation. The message is a pre-approved WhatsApp template, not a free-form AI response.


## How It Works


The flow is:


1. A contact is created in HubSpot (manually, via form submission, via API, or through an AI Action), or the` quickchat_send_whatsapp_marketing_template` property is set to` true` on an existing contact.
2. HubSpot fires a webhook to Quickchat AI:` contact.creation` for new contacts, or` contact.propertyChange` when the property is flipped to` true` .
3. Quickchat AI checks that the contact has the custom HubSpot property` quickchat_send_whatsapp_marketing_template` set to` true` .
4. If it does, Quickchat AI fetches the contact’s phone number and name from HubSpot, resolves the configured WhatsApp template’s parameters, and sends the template message via the WhatsApp Business API.


The feature does not require the **Enable HubSpot** toggle to be on. That toggle controls the live chat bot. WhatsApp template outreach only requires that HubSpot is connected (authorized via OAuth).


## Prerequisites


Before setting up this feature, you need:


- A[Quickchat AI](https://app.quickchat.ai/) account with an AI Agent
- A HubSpot account with admin access
- Your HubSpot account connected to Quickchat AI via OAuth (see[Step 2 of the HubSpot AI Actions guide](https://quickchat.ai/post/connect-ai-agent-to-hubspot#step-2-connect-hubspot) )
- A WhatsApp Business account connected to your Quickchat AI Agent
- At least one approved WhatsApp message template in your Meta Business Manager


## Step 1: Create a Custom Property in HubSpot


The system only sends a WhatsApp template to contacts that have been explicitly flagged for it. This prevents accidental messages to every new contact.


1. In HubSpot, go to **Settings** > **Properties** .
2. Click **Create property** .
3. Set the following values:


Field Value


Object type Contact


Group Contact information (or any group you prefer)


Label Send WhatsApp Marketing Template via Quickchat AI


Internal name` quickchat_send_whatsapp_marketing_template`


Field type Single checkbox


1. Save the property.


When creating a contact (or importing contacts in bulk), set this property to` true` for contacts that should receive the WhatsApp template. You can also flip it to` true` on an existing contact to trigger the message. Contacts without this property, or with it set to` false` , are skipped.


## Step 2: Create a WhatsApp Template in Meta Business Manager


WhatsApp requires all outbound business-initiated messages to use pre-approved templates. If you already have an approved marketing template, skip to Step 3.


1. Go to[Meta Business Manager](https://business.facebook.com/) > **WhatsApp Manager** > **Message Templates** .
2. Click **Create Template** .
3. Select **Marketing** as the category.
4. Choose a language (e.g.,` en` for English).
5. Write the template body. You can use placeholders for personalization.


WhatsApp supports two placeholder formats:


Format Example Description


Numbered` Hello {{1}}, welcome aboard!` Parameters are positional


Named` Hello {{firstname}}, welcome!` Parameters are referenced by name


Quickchat AI maps these placeholders to HubSpot contact properties:


Placeholder Maps to


` {{1}}` First Name


` {{2}}` Last Name


` {{firstname}}` First Name


` {{lastname}}` Last Name


Any other name Fallback value (` -` )


If your template uses parameters beyond first and last name (for example,` {{latest_post}}` or` {{3}}` ), those will receive a fallback dash (` -` ) as a placeholder value. The Meta API rejects empty string values for template parameters, so the fallback ensures delivery even when a value is unknown.


1. Submit the template for review. Meta typically approves marketing templates within a few minutes to a few hours.


## Step 3: Select the Template in Quickchat AI


1. Log in to your[Quickchat AI dashboard](https://app.quickchat.ai/) .
2. Select your AI Agent.
3. Go to **External Apps** and open the **HubSpot** integration sheet.
4. Below the pipeline configuration, you’ll see the **WhatsApp template for outbound contacts** section. This section is visible as long as HubSpot is authorized, regardless of whether the chatbot toggle is enabled.
5. The **Template** dropdown lists all approved templates from your connected WhatsApp Business account, along with their language codes.
6. Select the template you want to send. The configuration is saved immediately on selection.


To disable the feature, select **None** from the dropdown. When no template is selected, the webhook is still received but no message is sent.


## Step 4: Test the Setup


1. In HubSpot, create a new contact with:


- A valid phone number (including country code, e.g.,` +1 555 123 4567` )
- The` quickchat_send_whatsapp_marketing_template` property set to` true`


2. Within a few seconds, the contact should receive the WhatsApp template message on the phone number you specified.


You can also test the update path: set` quickchat_send_whatsapp_marketing_template` to` true` on an existing contact that has a phone number, and the template sends within a few seconds.


If the message does not arrive, check the following:


- The phone number includes a country code. Numbers without a country code cannot be routed by WhatsApp.
- The template is in “Approved” status in Meta Business Manager.
- The contact property` quickchat_send_whatsapp_marketing_template` is exactly` true` (not` True` , not` yes` ; HubSpot checkbox properties store lowercase` true` ).
- Your WhatsApp Business account is connected in the Quickchat AI Agent settings.


## Technical Details


This section covers the internals for those who want to understand what happens behind the webhook handler.


### Webhook Processing


HubSpot sends a` contact.creation` event (or a` contact.propertyChange` event when the` quickchat_send_whatsapp_marketing_template` property is set to` true` ) as a JSON array to the webhook endpoint. Each event contains the` portalId` (HubSpot account),` objectId` (contact ID), and` eventId` (unique event identifier). The handler extracts these three fields and dispatches a background task.


Deduplication uses a cache key based on` eventId` ,` objectId` , and` portalId` with a 10-minute TTL. HubSpot may retry webhook deliveries if it does not receive a timely` 200` response, so this prevents duplicate sends.


### Phone Number Normalization


The WhatsApp Business API expects phone numbers as digits only, without a leading` +` . The normalization logic:


1. Strips all non-digit characters.
2. Removes a leading` 00` international dialing prefix (e.g.,` 0048123456789` becomes` 48123456789` ).
3. Rejects numbers shorter than 7 digits as invalid.


Some examples:


Input Normalized


` +1 (555) 123-4567`` 15551234567`


` 0048123456789`` 48123456789`


` +442071234567`` 442071234567`


` 12345` Rejected (too short)


### Template Parameter Resolution


Before sending the template, the system fetches the template’s metadata from the Meta Graph API to determine the expected parameters. This is necessary because different templates have different placeholders, and the WhatsApp API rejects messages where the number of parameters does not match the template definition.


The resolution process:


1. Call` GET /{whatsapp_business_account_id}/message_templates?name={template_name}` on the Meta Graph API.
2. Find the` BODY` component in the response.
3. Extract all` {{...}}` placeholders using a regex.
4. For each placeholder:


- If it’s a digit (` {{1}}` ,` {{2}}` ), map positionally: position 1 = first name, position 2 = last name.
- If it’s a known name (` {{firstname}}` ,` {{lastname}}` ), map by name.
- Otherwise, use the fallback value` -` .


For named parameters, the Meta API requires a` parameter_name` field in the request body. Numbered parameters omit this field. The resulting payload sent to` POST /{phone_number_id}/messages` looks like this for a template with` {{firstname}}` and` {{lastname}}` :


```text
{
"messaging_product"  :   "whatsapp"  ,
"recipient_type"  :   "individual"  ,
"to"  :   "15551234567"  ,
"type"  :   "template"  ,
"template"  : {
"name"  :   "welcome_new_contact"  ,
"language"  : {   "code"  :   "en"   },
"components"  : [
{
"type"  :   "body"  ,
"parameters"  : [
{
"type"  :   "text"  ,
"parameter_name"  :   "firstname"  ,
"text"  :   "John"
},
{
"type"  :   "text"  ,
"parameter_name"  :   "lastname"  ,
"text"  :   "Doe"
}
]
}
]
}
}
```


If the template metadata cannot be fetched (network error, token issue), the system falls back to sending first name and last name as two positional parameters. If both are empty, it sends the template with no parameters at all.


### Authorization Model


This feature reuses the existing HubSpot OAuth connection but does not require the **Enable HubSpot** toggle to be on. That toggle controls the HubSpot live chat bot. The WhatsApp template outreach only requires that HubSpot is connected (authorized via OAuth). You can use this feature independently of the live chat integration.


## Use Cases


A few patterns where this is useful:


- **Post-form follow-up** : A prospect fills out a HubSpot form. A workflow sets the` quickchat_send_whatsapp_marketing_template` property to` true` . The contact is created, and the WhatsApp template is sent within seconds.
- **Event registration confirmation** : Import event attendees into HubSpot with the flag set. Each attendee receives a WhatsApp confirmation with event details.
- **Lead nurturing** : After an AI Agent creates a contact via a[HubSpot AI Action](https://quickchat.ai/post/connect-ai-agent-to-hubspot) , a HubSpot workflow can set the flag on the newly created contact, triggering a follow-up WhatsApp message asynchronously.


## What’s Next


For more on integrating your AI Agent with HubSpot, see the[HubSpot integration guide](https://quickchat.ai/post/create-ai-chatbot-for-hubspot) and the[HubSpot AI Actions tutorial](https://quickchat.ai/post/connect-ai-agent-to-hubspot) . For general information on WhatsApp templates, see Meta’s[message template documentation](https://developers.facebook.com/docs/whatsapp/message-templates) .
