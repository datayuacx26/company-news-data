---
schema_version: "1.0.0"
document_id: "5bb7c2d1f711babac83bfec2ebaf867ec54958c773571fc14dac8673e72c307b"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/sms-api-with-chatgpt/"
published_at: "2023-08-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:2cb740e61397f249830e6d7a989951b50f395937fdc29fff4cc92d9623f67efc"
---

# How to Use Plivo’s SMS API with ChatGPT

Have you jumped on the ChatGPT bandwagon yet? We’ve been brainstorming how OpenAI’s AI-adjacent natural language processing tool might help Plivo customers. Ideas have ranged from applications as simple as a natural language autoresponder to an application that runs a script based on voice or SMS input to a more complex use case such as an SMS chatbot emulation. (We’d be interested in hearing about anything you’ve already built, or that you’re thinking about.)


[Plivo](https://www.plivo.com/) and ChatGPT can interact in two ways - think of them as “to” and “from.” A Plivo action (such as an incoming call or text message) could be a trigger *to* invoke ChatGPT. For instance, someone might text a question with an #askChatGPT hashtag. Plivo could use ChatGPT APIs to pass that query to ChatGPT and return the answer to the person who initiated the query.


Another use case, for data going in the other direction, would have someone invoke something *from* ChatGPT that triggers a Plivo action. For instance, you could go to the OpenAI’s[Playground](https://platform.openai.com/playground) page and ask ChatGPT to text everyone in a list a weather report for their area.


ChatGPT can’t use Plivo APIs to call Plivo directly. Any application that wants to allow ChatGPT to use its APIs has to write an[OpenAI plugin](https://platform.openai.com/docs/plugins/introduction) - and plugins are just leaking out in beta form, so a Plivo plugin won’t be available by the time you read this.


However, Plivo partners with[Zapier](https://zapier.com/) , an application integration tool, and Zapier is developing a plugin for ChatGPT that lets Playground users call zaps - and it’s easy to write a[zap that triggers a Plivo action](https://www.plivo.com/docs/integrations/zapier/) . How well can three very different platforms work together?


We thought a post that walks you through how to send messages through Plivo to ChatGPT, and from ChatGPT to recipients via Plivo, would be valuable. We start with having a Plivo action invoke ChatGPT.


## → From Plivo to ChatGPT in code →


To see how a Plivo action could trigger ChatGPT, we wanted to write a little proof-of-concept code that takes the text of any SMS message that comes in on a specific number, calls OpenAI’s API with that text, and replies to the[SMS](https://www.plivo.com/sms/) with ChatGPT’s answer.


And in the spirit of keeping things simple, we thought we’d give ChatGPT a chance to write the code for us. We gave it a prompt:


**Using the Plivo Python SDK, write Python code to send the text of an incoming SMS message to ChatGPT and return the reply from ChatGPT to the number that sent the first text.**


And it returned this code.


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; padding: 15px 18px 15px 18px; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; }


```text
# Import Plivo and ChatGPT


import plivo
from chatgpt import ChatGPT


# Create an instance of ChatGPT
chatgpt = ChatGPT()


# Define function to process incoming message, send to ChatGPT, and return response
def process_incoming_message(incoming_message, incoming_number):
# Send incoming message to ChatGPT
chatgpt_response = chatgpt.send_text(incoming_message)


# Send response from ChatGPT back to sender
client = plivo.RestClient()
client.messages.create(
src='',
dst=incoming_number,
text=chatgpt_response
)


```


‍


That’s a decent starting point, but ChatGPT isn’t going to take any developers’ jobs just yet. It’s a very simple use case - but sometimes that’s all you need. However, in this case, one fatal flaw is the lack of a ChatGPT module with APIs that let you make queries.


Plivo CTO Mike Ricordeau took the concept and made it scalable. His sms_bot.py code, using Python and Flask, is available online, so you can clone it and play with it. Follow the instructions in the README file.


Before you deploy the application, you must expose it at a public URL. To do that, we recommend using ngrok. Make sure to specify to ngrok the same port on which the sms_bot.py application is running. Ngrok will return a forwarding IP address you can use.


Once you’ve obtained a forwarding address from ngrok, visit the Plivo console and add a Plivo application. Specify the address, postpended with “/sms,” for the Message URL.


Next,[pick one of your active phone numbers](https://console.plivo.com/active-phone-numbers/) and associate it with the application you just created. In the Application Type drop-down, choose XML Application, and under Plivo Application choose the name of the Plivo application you added.


Now the application should be live and available. You can try it by texting a message to the phone number you used. Plivo should receive the question and POST the request to the Message URL, and ngrok forwards it to the Python application, which connects to OpenAI and asks the question. OpenAI gives the answer. The Python application then returns a Message XML element with the response, which is sent to the user’s phone number. The entire “transaction” took about four seconds from when I tapped Send to when I received a response.


## ← To Plivo from ChatGPT via Zapier ←


Suppose, instead, you want to have ChatGPT call Plivo to send a text message.


As we said, ChatGPT can’t use Plivo APIs directly. Fortunately, Plivo partner Zapier is a member of the OpenAI ChatGPT plugin program. As we mentioned, Zapier is an application integration tool that lets anyone connect disparate web applications together in a workflow. Anyone with a ChatGPT Plus account can use Zapier to perform an action from ChatGPT that calls Plivo.


To try it out for SMS API integration you first have to set things up.[Sign up for a ChatGPT Plus subscription](https://openai.com/blog/chatgpt-plus) - it costs $20 a month - and you’ll need a[Zapier account](https://zapier.com/sign-up) too. From the[ChatGPT home screen](https://chat.openai.com/?model=gpt-4) click on your account name in the bottom left corner, then Settings > Data Controls, and enable 2FA. The page displays a QR code that you have to scan with an authenticator application running on a smartphone. The app should display a six-digit number for you to copy into the authentication dialog.


Once you’ve enabled 2FA, click on GPT-4 at the top of the ChatGPT home screen and choose Plugins from the drop-down menu. From the Plug-in store, find Zapier and click **Install** . Your browser will open a new window and Zapier will prompt you to connect; click **Allow** .


After you’ve enabled the Zapier plugin you can go back to the main ChatGPT window and ask a question. When you have an answer, you can say something like “Send that via Zapier and Plivo.” The first time you do that, ChatGPT will give you a link to click on that brings up a page where to enable an action.


We’ve written[documentation on how to use Zapier with Plivo](https://www.plivo.com/docs/integrations/zapier/) , sans ChatGPT; it talks about the available options. If you’re one of those free spirits who prefers trying things to reading doc pages, you can type “Plivo” into the Action field to see what actions are available. (Or you can type any string and use any platform that Zapier integrates with.) Complete the form, then click **Enable action** . At some point you’ll have to specify your Plivo Auth ID and Auth Token to authenticate your account within Zapier.


Those are all the steps - sounds easy, no? But some of this software is new and not completely debugged. We had problems connecting our Plivo phone numbers to Zapier through the plugin screen. Also, see the “Have AI guess a value” prompt in the screenshot above? We tried doing that for our source number, since we were using a test account that had only a single active number. AI failed to guess the value.


### Prompt engineering


At this point you’ve created an action that ChatGPT can use by calling the Zapier plugin. Now you have to write a good prompt that not only retrieves the information you want but also lets ChatGPT know what to do with it. We found things worked best when we used a phrase like “send a text using Plivo.” ChatGPT “knew” that it could access Plivo via Zapier, as in this example.


When we clicked the link, we found that ChatGPT had filled in the source and destination numbers properly. However, the text we asked it to generate wasn’t what we were looking for; instead of a polite reminder that payment was due, the body of the text was “Recipient’s phone number.” AI works in mysterious ways.


There’s clearly a mismatch somewhere in the communication between ChatGPT and Zapier. We took a second try, telling ChatGPT to send the text to a specific destination phone number, but that didn’t help.


When we clicked **Run** , ChatGPT/Zapier/Plivo sent the text perfectly, and it arrived almost instantly, so the good news is that all of the pieces were able to communicate.


As if it weren’t enough to automate all it did so far, ChatGPT wasn’t finished. It continued …


Sadly, when we clicked on the link, there wasn’t much there but the title of the new Zap. If we had wanted to use it we would have had to fill in all the fields, as if we were starting from scratch.


## A work in progress


From this experience, we judged ChatGPT’s integration with Zapier to be a work in progress - as one might expect for such new software. We wouldn’t rely on it for SMS automation. Still, it’s clear we have a base on which companies can build. Ask us what we think again in six months.


The takeaway for now is that you can incorporate ChatGPT into workflows with Plivo - but don’t expect perfection.
