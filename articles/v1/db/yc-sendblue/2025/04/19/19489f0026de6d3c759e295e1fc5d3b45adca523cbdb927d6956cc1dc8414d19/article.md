---
schema_version: "1.0.0"
document_id: "19489f0026de6d3c759e295e1fc5d3b45adca523cbdb927d6956cc1dc8414d19"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/sending-a-group-message-with-sendblues-group-message-api"
published_at: "2025-04-10T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:c1982b4ac46953b149fcb00b12f8d931495134482fd1916bb57c0cda3b4877e1"
---

# Sending a Group Message with Sendblue's Group Message API

We'll start by setting up a Sendblue client in our workspace. This will allow us to send arbitrary messages from anywhere in our codebase. We'll also set up a webhook to receive status updates for Stripe Subscriptions.


‍


### 1. Configuring the Sendblue Client


To send Sendblue group iMessages, make sure that you have signed up for an account ([https://sendblue.com/signup](https://sendblue.com/signup) ). Once you've signed up, you can find your Sendblue API key in your \[api settings\]([https://sendblue.com/dashboard/api](https://sendblue.com/dashboard/api) ). Then you can use the Sendblue client to send messages to groups of people:


```text
in   `sendblue_client.py`:


```python


from   sendblue   import   Sendblue


client = Sendblue(api_key=  "YOUR_API_KEY"  , api_secret=  "YOUR_API_SECRET"  )


client.send_group_message(


numbers=[  "+19998887777"  ,   "+19998887778"  ],


content=  "Testing a group message!"  ,


media_url=  "https://picsum.photos/200/300.jpg"  ,


)


```
```


Of course, you will want to replace the api key and secret with your own. You can also replace the numbers and message content.


‍


Once you are able to verify that the messages are coming through with this simple script, you can move on to the next step.


‍


### 2. Configuring the Stripe Webhook


Assuming you have followed \[Stripe's basic subscription payments tutorial\]([https://stripe.com/docs/billing/quickstart](https://stripe.com/docs/billing/quickstart) ), you should have a Stripe workflow setup. Now we just need to add a notification to the workflow to send a message to our group when a subscription is created.


‍


First, make sure that the webhook is configured to send a notification to Sendblue. You can do this by going to the \[webhook settings\]([https://dashboard.stripe.com/webhooks](https://dashboard.stripe.com/webhooks) ) and adding a new webhook endpoint. The endpoint should be the URL of your backend server. It should look something like this:


```text
![Stripe webhooks](assets/images/group-message/stripe-subscription.png){: width=  "80%"   }
```


Correctly Configured Stripe Webhook


```text
{: style=  "color:gray; font-size: 80%; text-align: center;"  }
```


‍


### 3. Inserting a Notification for Stripe Subscriptions


Once you've added the webhook, you can add the Sendblue code from before into your Stripe webhook handler. This will send a message to your webhook when a subscription is created. This is what your webhook handler should look like before adding Sendblue:


```text
```python


import   json


import   os


import   stripe


from   flask   import   Flask, jsonify, request


# This is your Stripe CLI webhook secret for testing your endpoint locally.


endpoint_secret =   'whsec_YOUR-WEBHOOK-SECRET'


app = Flask(__name__)


@app.route(  '/webhook'  , methods=[  'POST'  ]  )


def     webhook  ():


event =   None


payload = request.data


sig_header = request.headers[  'STRIPE_SIGNATURE'  ]


try  :


event = stripe.Webhook.construct_event(


payload, sig_header, endpoint_secret


)


except   ValueError   as   e:


# Invalid payload


raise   e


except   stripe.error.SignatureVerificationError   as   e:


# Invalid signature


raise   e


# Handle the event


if   event[  'type'  ] ==   'customer.subscription.created'  :


subscription = event[  'data'  ][  'object'  ]


# ... your custom handler for new subscriptions


else  :


print  (  'Unhandled event type {}'  .  format  (event[  'type'  ]))


return   jsonify(success=  True  )


```
```


Next, add an import to the Sendblue client at the top of this module like so:


```text
```python


from   sendblue_client   import   client


ADMIN_NUMBERS = [  "+19998887777"  ,   "+19998887778"  ]   # replace with your own numbers


```
```


Then, add the following code to the \`customer.subscription.created\` handler:


```text
```python


...


if   event[  'type'  ] ==   'customer.subscription.created'  :


subscription = event[  'data'  ][  'object'  ]


# ... your custom handler for new subscriptions


client.send_group_message(


numbers=ADMIN_NUMBERS,


content=  "There was a new subscription! "   + subscription[  "id"  ],


media_url=  "https://picsum.photos/200/300.jpg"  ,


)


...


```
```
