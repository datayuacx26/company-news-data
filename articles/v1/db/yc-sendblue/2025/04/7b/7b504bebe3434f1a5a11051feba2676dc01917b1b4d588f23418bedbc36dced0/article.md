---
schema_version: "1.0.0"
document_id: "7b504bebe3434f1a5a11051feba2676dc01917b1b4d588f23418bedbc36dced0"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/twilio-imessage-integration"
published_at: "2025-04-07T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:4746eb487fe100508d43dcae8cf27910fe30a85e5d188c892b94c4a3c5b169e3"
---

# Twilio iMessage Integration

If you already have a Twilio-based messaging workflow, adding iMessage through Sendblue starts by moving the number's messaging route to Sendblue. The walkthrough below shows how to send messages through Sendblue while keeping Twilio available for calling/voice if you need it.


### Step 1: Get Your Sendblue API Credentials


Sign up at[sendblue.com/signup](https://sendblue.com/signup) and grab your API key and secret from the[API dashboard](https://sendblue.com/dashboard/api) . You will have sandbox access immediately — no approval process required.


### Step 2: Install the Sendblue SDK


For Node.js projects, install via npm:


```text
npm install sendblue
```


For Python projects:


```text
pip install sendblue
```


If you prefer raw HTTP requests, the REST API works with any language that can make POST requests.


### Step 3: Initialize the Sendblue Client


Here is how to set up the client in Node.js (TypeScript):


```text
import Sendblue from 'sendblue';


const sendblue = new Sendblue(
process.env.SENDBLUE_API_KEY,
process.env.SENDBLUE_API_SECRET
);
```


And in Python:


```text
from sendblue import Sendblue


sendblue = Sendblue(
api_key=os.environ['SENDBLUE_API_KEY'],
api_secret=os.environ['SENDBLUE_API_SECRET']
)
```


### Step 4: Send Your First iMessage


Sending an iMessage is a single API call. Node.js example:


```text
const response = await sendblue.sendMessage({
number: '+19998887777',
content: 'Hey! Just following up on your inquiry.',
sendStyle: 'celebration', // optional iMessage effect
mediaUrl: 'https://example.com/brochure.pdf' // optional attachment
});


console.log(response.status); // 'QUEUED'
```


Python equivalent:


```text
response = sendblue.send_message(
number='+19998887777',
content='Hey! Just following up on your inquiry.',
send_style='celebration',
media_url='https://example.com/brochure.pdf'
)


print(response.status)  # 'QUEUED'
```


Sendblue automatically detects whether the recipient has iMessage enabled. If they do, the message is delivered as a blue bubble iMessage. If not, it falls back to RCS first, then SMS from the ported messaging line — no extra routing logic needed on your end.


### Step 5: Keep Twilio for Calls, Use Sendblue for Messaging


The supported pattern is to move the number's messaging route to Sendblue, then continue using Twilio for voice/calling if you need it. Do not send SMS from Twilio and iMessage from Sendblue on the same number; all messaging for that line should flow through Sendblue.


```text
// Use Sendblue for outbound messaging from the ported line
const response = await sendblue.sendMessage({
number: recipientPhone,
content: messageBody
});


// Sendblue chooses iMessage, RCS, SMS, or MMS as needed.
console.log(response.status);
```


When you send through Sendblue, it automatically detects the recipient's device and chooses iMessage, RCS, SMS, or MMS accordingly. Keep your Twilio call handling separate on the voice side of the number.


### Step 6: Set Up Webhooks for Inbound Messages


Configure your webhook URL in the[Sendblue dashboard](https://sendblue.com/dashboard) under Settings. When recipients reply, Sendblue sends a POST request to your endpoint with the following payload structure:


```text
{
"accountEmail": "you@yourcompany.com",
"content": "Yes, I'd love to schedule a demo!",
"is_outbound": false,
"number": "+19998887777",
"send_style": "invisible",
"date_sent": "2026-02-11T10:30:00.000Z",
"date_updated": "2026-02-11T10:30:00.000Z",
"was_downgraded": false,
"media_url": null,
"message_type": "message",
"group_id": null
}
```


Your webhook handler can then route this into your CRM, trigger automations, or feed it into an AI agent for intelligent auto-replies. Full webhook documentation is available at[docs.sendblue.com](https://docs.sendblue.com/) .


### Step 7: Send Contact Cards for Higher Trust


One of the most impactful things you can do after connecting is send a vCard (contact card) via the API. This is unique to Sendblue — no other iMessage API provider supports this. When the recipient saves the contact, your future messages show your business name instead of an unknown number:


```text
await sendblue.sendMessage({
number: '+19998887777',
content: '',
mediaUrl: 'https://yourdomain.com/contact.vcf'
});
```


This single step dramatically increases trust and long-term engagement with your contacts.
