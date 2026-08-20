---
schema_version: "1.0.0"
document_id: "f534040f8b951334e9dea40bf284686c8bb336e2541146dfb9907143e32189e1"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/pinnacle-simulate-endpoints"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T09:08:45.431702+00:00"
fetched_at: "2026-07-28T22:15:36.111958+00:00"
content_hash: "sha256:c98aa04d390151a38ef179e07bc0a931545494df2ad5434f51e9c7120c0de0bf"
---

# Test Your RCS Flows with Pinnacle's Simulate Endpoints

> **Beta feature** — The simulate endpoint is available to teams on request.[Book a call](https://cal.com/rcs/30min?notes=Interested+in+simulate+endpoint+beta+access) to get access.


Testing a conversational messaging flow is hard. The user taps a button. Your webhook fires. Your backend processes it and sends the next message. You tap another button. You need a real phone to run this loop.


Or you used to.


Pinnacle's simulate endpoint lets you trigger any user-side event — text message, button tap, location share, call action — programmatically, against your local dev environment or CI pipeline. No device. No carrier. Just a POST request that looks exactly like real inbound traffic.


## The Simulate Endpoint


```text
POST /messages/simulate/user


```


The request body defines what type of event to simulate and who it's "from":


TypeScript


```text
interface   SimulateUserRequest   {
from  :   string  ;   // E.164 phone number of the simulated user
to  :   string  ;   // Your RCS agent ID or phone number
message  ?:   {
text  ?:   string  ;
mediaUrls  ?:   string  [];
};
button  ?:   SimulatedButton  ;
options  ?:   {
physical  ?:   boolean  ;   // Route through real carrier infrastructure (see below)
};
}
```


Send a` message` to simulate an inbound text or media message. Send a` button` to simulate a user tapping a quick reply or card button.


## Simulate an Inbound Text Message


Test your inbound message handler without touching a phone:


TypeScript


```text
await   fetch  (  "  https://api.pinnacle.sh/messages/simulate/user  "  ,   {
method  :   "  POST  "  ,
headers  :   {
Authorization  :   `  Bearer   ${  process  .  env  .  PINNACLE_API_KEY  }  `  ,
"  Content-Type  "  :   "  application/json  "  ,
},
body  :   JSON  .  stringify  ({
from  :   "  +14155551234  "  ,
to  :   "  your_agent_id  "  ,
message  :   {
text  :   "  What are your store hours?  "  ,
},
}),
});
```


Your webhook receives this exactly as it would receive a real inbound message — same payload shape, same headers, same signature. Your handler code runs without modification.


You can also simulate an inbound image or media message by passing` mediaUrls` :


TypeScript


```text
body  :   JSON  .  stringify  ({
from  :   "  +14155551234  "  ,
to  :   "  your_agent_id  "  ,
message  :   {
text  :   "  Here's a photo of the issue.  "  ,
mediaUrls  :   [  "  https://cdn.example.com/uploads/photo.jpg  "  ],
},
});
```


## Simulate Button Taps (RCS Only)


This is the killer feature. RCS agents support rich action buttons — confirm an appointment, approve a transaction, select a product. Testing each button manually requires a real device, a registered test number, and someone to actually tap.


The simulate endpoint handles all of it:


Simulates a user tapping a quick reply or postback button. Your webhook receives the` payload` you set on the button — your handler processes it and sends the next message. The full round trip, no phone involved.


TypeScript


```text
body  :   JSON  .  stringify  ({
from  :   "  +14155551234  "  ,
to  :   "  your_agent_id  "  ,
button  :   {
type  :   "  trigger  "  ,
title  :   "  Confirm  "  ,
payload  :   "  confirm_appt_123  "  ,
},
});
```


## Physical Device Simulation


By default, simulate calls are synthetic — Pinnacle constructs the inbound event and delivers it directly to your webhook without touching carrier infrastructure. This is fast, fully offline, has no negative impact on messaging compliance, and ideal for CI.


Set` options.physical: true` to route the simulation through real carrier infrastructure instead:


TypeScript


```text
body  :   JSON  .  stringify  ({
from  :   "  +14155551234  "  ,
to  :   "  your_agent_id  "  ,
button  :   {
type  :   "  trigger  "  ,
title  :   "  Confirm  "  ,
payload  :   "  confirm_appt_123  "  ,
},
options  :   {
physical  :   true  ,
},
});
```


With` physical: true` , the event travels through the same carrier pipeline as a real user interaction — including real network latency, carrier acknowledgment, and delivery receipts. Use this mode when you need to validate end-to-end timing, test your retry logic under real delivery delays, or confirm that carrier-specific event shapes match what your handler expects.


Physical simulation is slower and consumes carrier capacity. Use synthetic simulation (the default) for unit and integration tests, and reserve physical mode for pre-production validation against real infrastructure.


## A Full Simulated Conversation


Here's a complete test sequence for a restaurant reservation flow, runnable in Jest or any test runner:


TypeScript


```text
// Step 1: User sends an inbound message
await   simulateUser  ({
from  :   "  +14155551234  "  ,
to  :   "  your_agent_id  "  ,
message  :   {   text  :   "  I'd like to make a reservation  "   },
});


// Your webhook fires, sends back a date selection card
await   waitForWebhook  (  "  message.sent  "  );


// Step 2: User taps "Tonight at 7pm"
await   simulateUser  ({
from  :   "  +14155551234  "  ,
to  :   "  your_agent_id  "  ,
button  :   {
type  :   "  trigger  "  ,
title  :   "  Tonight at 7pm  "  ,
payload  :   "  reserve_tonight_7pm  "  ,
},
});


// Step 3: User shares their location for nearest location lookup
await   simulateUser  ({
from  :   "  +14155551234  "  ,
to  :   "  your_agent_id  "  ,
button  :   {
type  :   "  sendLocation  "  ,
title  :   "  Share Location  "  ,
latLong  :   {   lat  :   37.7749  ,   lng  :   -  122.4194   },
},
});
```


Each simulate call triggers your real webhook handler with a real-looking event. You test the entire conversation flow in isolation, with reproducible inputs.


## Using Simulate in CI


Add simulate calls to your integration test suite. Before every deploy, your CI pipeline can:


1. Send an inbound message, verify the outbound reply
2. Tap a confirmation button, verify the downstream record update
3. Simulate a location share, verify the lookup response


No real device. No carrier dependency. No flaky test environment.


## Key Takeaways


- Simulate is in beta —[book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+simulate+endpoint+beta+access) with the Pinnacle team to get access and walk through your test setup.
- ` POST /messages/simulate/user` triggers any user-side event — text, media, or button tap — against your dev or staging environment.
- Supported button types:` trigger` ,` openUrl` ,` call` ,` scheduleEvent` ,` sendLocation` ,` requestUserLocation` .
- Your webhook receives the simulated event with the same payload shape as real inbound traffic.
- Set` options.physical: true` to route through real carrier infrastructure for timing and delivery validation.


## FAQ


**1. Do I need a whitelisted test number to use simulate?** No. The` from` number in a simulate call can be any E.164 number — it's a synthetic event, not a real message from a carrier.


**2. How do I verify my webhook received the simulated event?** Pinnacle logs all webhook deliveries in the[dashboard](https://app.pinnacle.sh/dashboard/development/webhooks) under **Webhooks → Logs** , including payload and response status. You can also inspect the event directly in your webhook handler.


**3. What's the difference between synthetic and physical simulation?** Synthetic (default) is instant and offline — Pinnacle constructs the event and posts it to your webhook directly. Physical (` options.physical: true` ) routes through real carrier infrastructure, adding real network latency and carrier-level delivery receipts. Use synthetic for most testing; use physical when you need to validate against real carrier behavior.


**4. Can I simulate both a message and a button in the same call?** No. Each simulate call takes either a` message` or a` button` , not both. Run them as separate calls to step through a multi-turn conversation.


**5. Is there an SDK wrapper for the simulate endpoint?** Not yet as a dedicated method, but you can call it directly via` fetch` or your HTTP client of choice using your Pinnacle API key. SDK support is coming soon.


[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+simulate+endpoint+beta+access) with the Pinnacle team — we'll get you set up with simulate access and walk through your test strategy.
