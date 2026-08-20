---
schema_version: "1.0.0"
document_id: "72ec87707ac301b751b6a31a41689df46781842c2c9978d85b52a6ee303d2013"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-send-an-eta-notification-using-plivo-python-and-flask/"
published_at: "2022-07-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:c7ce02265699549c5e31a41955cc97a2966587daa4804241b15242d936cdece4"
---

# How to send an ETA notification using Plivo Python and Flask

## Overview


Today, many people order products online and have them delivered. Many retail businesses provide consumers with updates on the estimated time of arrival (ETA) for their orders. You can use the Plivo Python SDK and the Flask framework to create an order, recognize when the order status changes, and send users an SMS notification with the latest ETA. We’ve created some demo code you can use as a template or to see how to incorporate these tasks into your own applications.


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. To send messages to the United States and Canada, you must have a Plivo phone number that supports SMS; you can rent numbers from the[Numbers](https://console.plivo.com/active-phone-numbers/) page of the Plivo console or by using the[Numbers API](https://www.plivo.com/docs/numbers/) . If this is your first time using Plivo APIs, follow our instructions to[set up a Python development environment](https://www.plivo.com/docs/sdk/server/set-up-python-dev-environment-api-messaging/) .


## How does it work?


Our code examples cover three steps of the order and delivery process.


- A user places an order from a web application on their desktop or a mobile app. The order application reports an initial ETA to the consumer.
- The retailer processes the order and updates its system when the ordered item is handed off to a delivery service.
- When the order is delivered, the service updates the delivery status again.


In the first two steps of the process, the program must calculate an updated ETA to communicate with the consumer.


For purposes of this demo we’ve put all three functions into one program, but in the real world they might be in three different applications.


## Set up the demo application locally


- Clone the demo application repository from GitHub.


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; padding: 15px 18px 15px 18px; } pre.lineno{ color: #fff; opacity: .3; }


```text
git clone https://github.com/plivo/ETA-notifications-flask.git


```


- Change your working directory to 2fa-python-demo.


```text
cd   ETA-notification-flask


```


- Install the dependencies using the requirements.txt file.


```text
pip  install    -r   requirements.txt


```


Edit .env. Replace the auth placeholders with your authentication credentials from the[Plivo console](https://console.plivo.com/dashboard/) . Replace the phone number placeholder with an actual phone number in[E.164](https://en.wikipedia.org/wiki/E.164) format (for example, +12025551234).


## A review of the code


Let’s walk through what the code does.


First, a user enters their name and phone number in an order form. (In a real application, information about the item being ordered, the delivery location, and other information would also be included.) This code adds the order to a database. This application doesn’t send any SMS message.


```text
@  app  .  route  (  '/add/orders'  ,    methods  =  [  'POST'  ])
def    add_orders  ():
customer_name    =    request  .  form  .  get  (  'name'  )
phone_number    =    request  .  form  .  get  (  'phone_number'  )
order    =    Order  (  customer_name  =  customer_name  ,
customer_phone_number  =  phone_number  )
db  .  session  .  add  (  order  )
db  .  session  .  commit  ()
return    redirect  (  url_for  (  'order_index'  ))


```


Later, someone at the company updates the order status, either by making a change through a user interface, as we show here, or via an automatic update from other software, such as a shipping application. Code on the back end calculates an updated ETA, and this code then sends an SMS message to the customer with the new time.


```text
@  app  .  route  (  '/order/<order_id>/pickup'  ,    methods  =  [  'POST'  ])
def    order_pickup  (  order_id  ):
order    =    Order  .  query  .  get  (  order_id  )
order  .  status    =    'Shipped'
order  .  notification_status    =    'queued'
db  .  session  .  commit  ()
callback_url    =    request  .  base_url  .  replace  (  ‘/pickup’  ,    ”  )    +    ‘/notification/status/update’
send_sms_notification  (  order  .  customer_phone_number  ,
‘Your package is on its way -
ETA ’    +    now  .  strftime  (  “%d/%m/%Y %H:%M:%S”  ),
callback_url  )
return    redirect  (  url_for  (  ‘order_show’  ,    order_id  =  order_id  ))


```


Similar code handles notification of package delivery.


```text
@  app  .  route  (  ‘/order/<order_id>/deliver’  ,    methods  =  [  ‘POST’  ])
def    order_deliver  (  order_id  ):
order    =    Order  .  query  .  get  (  order_id  )
order  .  status    =    ‘Delivered’
order  .  notification_status    =    ‘queued’
db  .  session  .  commit  ()
callback_url    =    request  .  base_url  .  replace  (  ‘/deliver’  ,    ”  )    +    ‘/notification/status/update’
send_sms_notification  (  order  .  customer_phone_number  ,
‘Your package was delivered ’  +    now  .  strftime  (  “%d/%m/%Y %H:%M:%S”  ),    callback_url  )
return    redirect  (  url_for  (  ‘order_index’  ))


```


Along with the order status, the[status](https://support.plivo.com/hc/en-us/articles/360041315292-What-are-the-different-SMS-statuses-in-Plivo-) of the sent message is also updated, which is referred to via the callback url in this code.


## Test


Save your code and run it.


```text
flask run


```


[Set up ngrok](https://www.plivo.com/docs/sdk/server/set-up-python-dev-environment-api-xml-voice/#ngrok-setup) to expose your local server to the internet.


You should be able to see the application in action at[https://.ngrok.io/](https://.ngrok.io/) .


**Note:** If you’re using a Plivo Trial account, you can send messages and make calls only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers >[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
