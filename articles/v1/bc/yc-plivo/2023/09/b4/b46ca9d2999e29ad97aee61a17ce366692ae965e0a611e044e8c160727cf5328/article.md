---
schema_version: "1.0.0"
document_id: "b46ca9d2999e29ad97aee61a17ce366692ae965e0a611e044e8c160727cf5328"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/maximize-ivr-menu-efficiency-with-preanswer/"
published_at: "2023-09-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:19ba1f9f8b0904140660376eea3794e9f29551b0f849894380fe889108554159"
---

# Get Extra Value from Your IVR Menu with PreAnswer

One time you can be sure to have your customers’ attention is when they call you. Many callers spend a few moments on hold on their way to getting their questions answered. Don’t waste those precious seconds — use them wisely by giving callers information they can use.


It’s a rare business nowadays that keeps a human receptionist on the payroll to answer customers’ phone calls. Instead, most companies use[interactive voice response](https://plivo-webflow.webflow.io/glossary/what-is-ivr) (IVR) — automated technology that speaks a menu of options and lets users make choices by speaking or pressing a phone keypad key.


Plivo makes it easy to create an IVR menu tree in a couple of ways. Our PHLO visual workflow design tool lets you drag components onto a canvas and use them as building blocks for your menu tree; we wrote a[blog post](http://plivo-webflow.webflow.io/blog/how-to-build-virtual-assistant-the-no-code-way-using-phlo) that walks you through the process. Or you can[write an IVR menu with your favorite SDK and Plivo XML documents](https://www.plivo.com/docs/voice/use-cases/ivr/python#xml-how-it-works) . It’s not drag-and-drop, but it’s pretty easy — and it’s what you need to do to take advantage of this tip.


## From IVR to OIC


When you forward a call to an extension, sometimes it gets queued up waiting to be answered. If you had a customer’s attention, even if just for a few seconds, what would you communicate to them?


[PreAnswer](https://www.plivo.com/docs/voice/xml/preanswer/) lets you specify what happens after a call is transferred but before it’s picked up. Some companies squander those seconds playing inoffensive music. But there are better possibilities.


For instance, suppose you’re a restaurant and you have a daily special, or maybe you’re a retailer with a one-day sale. You can put text that describes the deal into a file that your application can open and read out using text-to-speech.


Or suppose you’re transferring a call to a department that gets the same questions over and over. You could record answers to common questions and play them to callers. If you answer a customer’s question with recorded information they’ll hang up satisfied, and you’ll have freed up an employee’s time.


## Tech specs


Here’s how it works on a technical level. Plivo lets you control call flows with XML code. The PreAnswer XML element lets you embed any of three other elements:


- [Speak](https://www.plivo.com/docs/voice/xml/speak/) plays specified text using text-to-speech. The Speak XML element tells Plivo to generate spoken audio, powered by[Amazon Polly](https://aws.amazon.com/polly/) . We support 27 languages and more than 40 voices, and by using Speech Synthesis Markup Language ([SSML](https://www.plivo.com/docs/voice/concepts/ssml/) ) you can control pronunciation, pitch, and volume to make the spoken words sound more natural and less machinelike.
- [Play](https://www.plivo.com/docs/voice/xml/play/) plays audio in MP3 or WAV format.
- [Wait](https://www.plivo.com/docs/voice/xml/wait/) waits silently for a specified number of seconds.


When you forward a call, you can specify the PreAnswer element with an embedded Speak or Play element.


### Speak friend and enter


Here’s a little Python code that shows how to use the Speak element. Suppose you put the messages you want spoken in a text document called speak_input.txt:


Thanks for being patient. To compensate you for your time on hold, we’re offering a 50% discount on a yearly subscription. Use the discount code “hold50” when you sign up. Someone will be with you shortly.


This code opens that file, reads the text, and adds it to the Speak element.


```text
from plivo import plivoxml
preanswer_message_file = open(“speak_input.txt”, “r”)
preanswer_message = preanswer_message_file.read()
response = plivoxml.ResponseElement()
response.add(plivoxml.PreAnswerElement().add(plivoxml.SpeakElement(preanswer_message)))


```


### Play on words


Alternatively, you could record your message (in this example in a file called sales_discount.mp3 that lives on Amazon S3) and use the Play element.


```text
from plivo import plivoxml
preanswer_play = “ https://s3.amazonaws.com/sales_discount.mp3”
response = plivoxml.ResponseElement()
response.add(plivoxml.PreAnswerElement().add(plivoxml.PlayElement(preanswer_play)))
```


### Wait a moment


Sometimes you might want a few seconds of silence before you speak or play a message. This code uses the Wait element to pause for 10 seconds.


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; padding: 15px 18px 15px 18px; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; }


```text
from plivo import plivoxml


response = plivoxml.ResponseElement()


response = (plivoxml.PreAnswerElement().add(plivoxml.WaitElement(None).set_length(10)))
```


What should you use PreAnswer time for? That’s up to you. Here are some possibilities.


- Provide an estimate of how long people will spend on hold.
- Remind people that they can find answers in your online support pages.
- Present special offers.
- Share company news, or if you’re a financial institution maybe share stock market news.


## Get creative


Of course you can choose a safe, boring message: “Thanks for calling our support line. We appreciate your business. Calls are answered in the order received.” You could even use bland “elevator music.” But given all of the more valuable possibilities, we suggest you get creative and take advantage of those fleeting moments of your callers’ attention.
