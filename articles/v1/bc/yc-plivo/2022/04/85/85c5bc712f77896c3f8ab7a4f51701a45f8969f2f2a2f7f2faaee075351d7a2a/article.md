---
schema_version: "1.0.0"
document_id: "85c5bc712f77896c3f8ab7a4f51701a45f8969f2f2a2f7f2faaee075351d7a2a"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/using-phlo-with-liquid-templating/"
published_at: "2022-04-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:b0c6a471d95e96c6e8ea242aa9119b3add5eae7711b0d5060184a82d99bff387"
---

# How to Turn Liquid into Communications Gold

A cloud communications platform can help businesses automate communication with their customers and prospects. Businesses can programmatically send and receive calls and text messages using Plivo’s[voice API](https://www.plivo.com/voice/) or[SMS API](https://www.plivo.com/sms/) for a variety of use cases.


Plivo’s APIs make coding communication applications simple - and we make developers even more efficient by providing a tool for building applications through a graphical user interface.[PHLO](https://www.plivo.com/phlo/) (Plivo High-Level Objects), our visual workflow design studio, is a no-code/low-code environment that lets you construct applications using visual building blocks and invoke them either from a program, using just a few lines of code, or directly from a web page. We’ve written a slew of use case guides that show you how to build feature-rich[voice](https://www.plivo.com/docs/voice/use-cases/make-outbound-calls/node/) and[messaging](https://www.plivo.com/docs/sms/use-cases/send-an-sms/node/) applications.


In PHLO you can use parameters such as phone numbers in your applications in two ways: either by specifying the exact values you want to pass or by using template elements.


[Liquid](https://github.com/Shopify/liquid) is an open source template language originally developed by Shopify. Web developers use template languages to build web pages that combine static content (text and graphics that are the same on multiple pages) and dynamic content, which changes from one page to the next. Liquid elements act as placeholders; at run time, they’re replaced by values you pass through them.


## Static and dynamic payloads


How does this work in PHLO? Consider the simple case of[sending an SMS message](https://www.plivo.com/docs/sms/use-cases/send-an-sms/node/) . Suppose you work for a professional practice that has multiple locations, and you want to send your clients an appointment reminder on the day before they’re scheduled to meet with you. Your application needs to be aware of three fields: the number you’re sending the message from, the number you’re sending the message to, and the text of the message.


In this case you would probably use a static field for the message sender - your business’s long code phone number. You would pass the destination number as part of your API payload using a Liquid element. And the text message would be partly static and partly dynamic - something like “Don’t forget your meeting with us tomorrow at <time> in our <town> office, <address>.”


## Map out the workflow in PHLO


Here’s what the PHLO logic looks like for this use case.


This PHLO starts with a Start node, as every PHLO does. When the PHLO is called by a program making an API request, it triggers a Send Message node. Conceptually, it’s pretty simple. You could make it more elegant by adding logic to be triggered when the message is sent or if it fails, but we don’t need to do that right now.


## Configure the Start node


Once we understand the workflow, we have to configure each node to send the message properly. Clicking on a node brings up a configuration pane at the right of the PHLO canvas. Here’s what the Start node’s looks like.


Notice the payload key-value pairs under API Request. The key names match the dynamic fields we plan to pass to the PHLO. Plivo prepends the string “Start.http.params.” before each key name to create a Liquid element - for example, .


## Configure the Send Message node


We then reference those keys in the configuration pane for the Send Message node.


As you can see, the From field is a static value - it’ll be the same every time you invoke the PHLO. The To field is dynamic - it’ll hold whatever value is passed from the code that invokes it. And the Message field is a combination of the two.


To specify a Liquid element when you’re configuring a field, start typing two braces (“{{“). PHLO will instantly display a list of defined elements, and you can arrow down to the one you want to insert.


## Calling the PHLO


Now we need to run a little code to call the PHLO. In Python, the code might look like this. Strings in <angle brackets> are placeholders for actual values that the program would take from a database or a cloud application that holds client appointment information.


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; padding: 15px 18px 15px 18px; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; }


```text
import plivo
auth_id = '<auth_id>'
auth_token = '<auth_token>'
phlo_id = '<phlo_id>'
payload = {"To" : "<destination_number>",
"time" : "<appt_time>",
"town" : "<location>",
"address" : "<address>"}
phlo_client = plivo.phlo.RestClient(auth_id=auth_id, auth_token=auth_token)
phlo = phlo_client.phlo.get(phlo_id)
response = phlo.run(**payload)
print str(response)


```


You’ll notice we didn’t pass the From field. That’s because it’s static, so we don’t have to pass it to the PHLO. All of the other fields get passed as part of the API payload when we call the PHLO.


## Final thoughts


Whether you use static or dynamic payloads, whether you use PHLO or write your entire use case in your favorite programming language, pay attention to[best practices for sending text messages](https://www.plivo.com/blog/sms-api-best-practices/) . And if you get hung up when writing PHLOs or calling them, look for answers in our[PHLO documentation](https://www.plivo.com/docs/phlo/) .
