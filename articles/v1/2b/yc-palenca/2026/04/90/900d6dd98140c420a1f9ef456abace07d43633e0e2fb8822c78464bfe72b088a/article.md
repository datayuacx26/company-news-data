---
schema_version: "1.0.0"
document_id: "900d6dd98140c420a1f9ef456abace07d43633e0e2fb8822c78464bfe72b088a"
company_key: "yc-palenca"
company: "Palenca"
source_id: "yc-palenca-rss-c9f8371bccc6"
canonical_url: "https://blog.palenca.com/how-to-integrate-palencas-api-in-under-10-minutes-2/"
published_at: "2026-04-14T03:12:33+00:00"
first_seen_at: "2026-07-25T18:13:53.501952+00:00"
fetched_at: "2026-07-28T20:52:42.510574+00:00"
content_hash: "sha256:3626bfb4ecf5739b72e49b6efa17000a0e38fbf8058f1d4b93bd3b58ab10c4c1"
---

# How to Integrate Palenca's API in Under 10 Minutes

---


Palenca lets you verify income and employment data for gig economy workers across platforms like Uber, Rappi, Didi, and more. Whether you're building a lending product, onboarding flow, or background check — connecting to Palenca's API is the fastest way to get real earnings data.


This guide walks you through a full integration in five steps. By the end, you'll have a working sandbox connection, a widget embedded in your app, and a webhook listener ready for production.


---


## Step 1: Create Your Sandbox Account and Get API Keys


Head to the[Palenca Console](https://www.console.palenca.com/sign/up?ref=blog.palenca.com) and sign up for a free account.


Once inside, navigate to **Developers** in the left menu. You'll find two sets of API keys:


- **Sandbox** — for testing with simulated data (` https://sandbox.palenca.com` )
- **Production** — for live connections (` https://api.palenca.com` )


Copy your **Sandbox Private API Key** — you'll need it for every request.


Next, create a **Widget** :


1. Go to the **Widgets** section in the Console
2. Click **Create Widget**
3. Give it a name, select your target countries and platforms
4. Copy the **Widget ID**


You now have everything you need: an **API key** and a **Widget ID** .


---


## Step 2: Make Your First API Call — Create a User


Every integration starts with creating a **User** . A User represents a person (e.g., a gig worker). Each User can then have multiple **Accounts** — one per platform they work on (Uber, Rappi, etc.).


### cURL


```text
curl -X POST https://sandbox.palenca.com/v1/users \
-H "x-api-key: YOUR_SANDBOX_API_KEY" \
-H "Content-Type: application/json" \
-d '{"widget_id": "YOUR_WIDGET_ID"}'


```


### Python


```text
import requests


url = "https://sandbox.palenca.com/v1/users"


headers = {
"x-api-key": "YOUR_SANDBOX_API_KEY",
"Content-Type": "application/json"
}


payload = {
"widget_id": "YOUR_WIDGET_ID"
}


response = requests.post(url, json=payload, headers=headers)
print(response.json())


```


### Response


```text
{
"success": true,
"error": null,
"data": {
"user_id": "2aa94c25-38cf-4734-b881-4f29f0bbc6bd",
"external_id": null
}
}


```


Save the` user_id` — you'll need it to track this worker's accounts and data.


> **Tip:** You can pass an` external_id` in the request body to map Palenca users to your own internal identifiers.


---


## Step 3: Embed the Palenca Widget


The Widget is a drop-in UI that handles the entire platform login flow for your users — credential input, 2FA, and error handling. You don't need to build any of that yourself.


### Option A: CDN (Vanilla JS)


Add the script tag to your page:


```text
<script src="https://link.palenca.com/v2/index.min.js"></script>


<div id="palenca-container"></div>


<script>
const publicApiKey = "YOUR_PUBLIC_API_KEY";
const widgetId = "YOUR_WIDGET_ID";


const renderConfig = {
configuration: {
hideConsent: true,
country: "mx",
},
appearance: {
primaryColor: "#4F46E5",
},
};


palenca.loadLink(publicApiKey, widgetId).then((link) => {
link.on("ready", () => {
console.log("Widget is ready");
});


link.on("user_created", (event) => {
console.log("User created:", event);
});


link.on("connection_success", (event) => {
console.log("Connection success for userId:", event.data.user_id);
});


link.on("connection_error", (event) => {
console.log("Connection error:", event.data.error.code);
});


link.render("palenca-container", renderConfig);
});
</script>


```


### Option B: ES6 Module (React)


Install the package:


```text
npm install @palenca/palencia-link


```


Then use the React component:


```text
import { loadLink, PalencaLinkReact } from "@palenca/palencia-link";
import { useCallback } from "react";


const linkPromise = loadLink("YOUR_PUBLIC_API_KEY", "YOUR_WIDGET_ID");


const options = {
configuration: {
hideConsent: true,
country: "mx",
},
appearance: {
primaryColor: "#4F46E5",
},
};


export default function PalencaConnect() {
const handleEvent = useCallback((event, data) => {
console.log("Event:", event);
console.log("Data:", data);
}, []);


return <PalencaLinkReact link={linkPromise} options={options} onEvent={handleEvent} />;
}


```


### Option C: Public Link (No Code)


If you don't need an embedded widget, share the hosted link directly:


```text
https://connect.palenca.com/?widget_id=YOUR_WIDGET_ID&external_id=YOUR_USER_ID


```


This is useful for testing or simple flows where you just need a URL.


---


## Step 4: Handle the Webhook Payload


When a worker connects their platform account through the Widget, Palenca sends webhook events to your server so you know exactly when data is ready.


### Set Up Your Webhook


1. In the Console, go to **Developers → Webhooks**
2. Click **Create Webhook**
3. Enter your endpoint URL (use[webhook.site](https://webhook.site/?ref=blog.palenca.com) for testing)
4. Associate it with your Widget
5. Select the events you want to receive


### Webhook Events


Event Type What it means


` login.created` Transitory A connection attempt has started


` login.success` Final Connection succeeded — data is ready


` login.error` Final A technical error occurred


` login.incomplete` Final User action required (e.g., missing 2FA)


### Sample` login.success` Payload


```text
{
"data": {
"webhook_id": "f393ce50-d8db-43a1-83da-a2d45b5dee50",
"account_id": "6fdf1c40-503b-11ef-88d3-6d05baf28f03",
"external_id": "1234567890",
"platform": "uber",
"user_id": "829462f0-503b-11ef-88d3-6d05baf28f03",
"status_details": null,
"webhook_action": "login.success"
},
"webhook_url": "https://your-app.com/palenca/webhook"
}


```


### Handle It in Your Backend


```text
from flask import Flask, request, jsonify
import requests


app = Flask(__name__)


PALENCA_API_KEY = "YOUR_SANDBOX_API_KEY"
BASE_URL = "https://sandbox.palenca.com"


@app.route("/palenca/webhook", methods=["POST"])
def handle_webhook():
event = request.json
action = event["data"]["webhook_action"]


if action == "login.success":
account_id = event["data"]["account_id"]


# Fetch the worker's earnings
earnings = requests.post(
f"{BASE_URL}/v1/accounts/{account_id}/earnings/search",
headers={
"x-api-key": PALENCA_API_KEY,
"Content-Type": "application/json"
},
json={
"start_date": "2025-01-01",
"end_date": "2026-04-01",
"options": {"items_per_page": 10}
}
)


# Fetch the worker's profile
profile = requests.get(
f"{BASE_URL}/v1/accounts/{account_id}/profile",
headers={"x-api-key": PALENCA_API_KEY}
)


print("Earnings:", earnings.json())
print("Profile:", profile.json())


return jsonify({"status": "ok"}), 200


```


> **Important:** Only call the Palenca API when you receive a` login.success` event. Other events indicate the connection is still in progress or failed.


### Sample Earnings Response


```text
{
"success": true,
"error": null,
"data": {
"account_id": "6fdf1c40-503b-11ef-88d3-6d05baf28f03",
"earnings": [
{
"amount": 179.0,
"currency": "mxn",
"earning_date": "2025-03-14",
"cash_amount": null,
"count_trips": 6
},
{
"amount": 245.50,
"currency": "mxn",
"earning_date": "2025-03-13",
"cash_amount": 80.0,
"count_trips": 9
}
]
},
"pagination": {
"page": 1,
"items_per_page": 10,
"total_items": 195,
"total_pages": 20
}
}


```


### Sample Profile Response


```text
{
"success": true,
"error": null,
"data": {
"account_id": "6fdf1c40-503b-11ef-88d3-6d05baf28f03",
"external_id": "1234567890",
"profile": {
"first_name": "Maria",
"last_name": "Garcia",
"email": "maria@example.com",
"phone": "+5215512345678",
"birthday": "1990-05-15",
"city_name": "Mexico City"
},
"metrics_info": {
"rating": 4.92,
"lifetime_trips": 3420,
"acceptance_rate": 0.95,
"cancellation_rate": 0.02,
"activation_status": "active"
}
}
}


```


---


## Step 5: Go Live Checklist


Ready to move to production? Run through this checklist:


- **Swap your base URL** — change` https://sandbox.palenca.com` to` https://api.palenca.com`
- **Use production API keys** — get them from the Console under **Developers** (production mode)
- **Create a production Widget** — set up your countries, platforms, and branding
- **Update your webhook URL** — point it to your production server, not webhook.site
- **Handle all webhook events** — implement logic for` login.error` and` login.incomplete` , not just` login.success`
- **Add retry logic** — Palenca retries failed webhooks up to 3 times at ~30-second intervals, but your server should return` 200` or` 201` within 30 seconds
- **Verify TLS** — Palenca requires HTTPS with TLS v1.3 or higher; certificate pinning is not supported
- **Test end to end** — run a full flow: widget → login → webhook → data fetch


---


## What's Next?


Once your integration is live, you can:


- **Query earnings data** with date ranges and pagination for detailed income verification
- **Retrieve full profiles** including identity documents, bank info, and vehicle details
- **Monitor connection health** through login states and webhook events
- **Scale across platforms** — Palenca supports Uber, Didi, Rappi, Cabify, InDrive, 99, and more


Check the[full API reference](https://developers.palenca.com/reference?ref=blog.palenca.com) for all available endpoints, or reach out tosoporte@palenca.com if you have questions.


---


*Building something cool with Palenca? We'd love to hear about it —get in touch .*
