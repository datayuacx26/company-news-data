---
schema_version: "1.0.0"
document_id: "28f1b550b7d8539f1abf384373d10132f3839cdfe5a4340ec8ec198cb0a618df"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-receive-and-respond-incoming-mms-messages-in-ruby/"
published_at: "2022-05-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:1459afb4192e1727be935fed5fee2cf56a6e1b66fd226884421d2416fc902b4a"
---

# How to Receive and Respond to Incoming MMS Messages in Ruby with Rails and Plivo

## Overview


This guide shows how to receive and automatically respond to incoming MMS messages on a[Plivo number](https://www.plivo.com/phone-numbers/) , as you might want to do for someone who’s out of the office or who leaves the company.


Here’s how to use[Plivo’s SMS APIs](https://www.plivo.com/sms/) to build this use case.


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. To receive incoming messages you must have a Plivo phone number that supports MMS; you can rent numbers from the[Numbers](https://console.plivo.com/active-phone-numbers/) page of the Plivo console or by using the[Numbers API](https://www.plivo.com/docs/numbers/) . If this is your first time using Plivo APIs, follow our instructions to[set up a Ruby development environment](https://www.plivo.com/docs/sdk/server/set-up-ruby-dev-environment-api-messaging/) and a web server and safely expose that server to the internet.


## Create a Rails server to receive MMS messages


Change to the project directory and run this command to create a Rails controller for inbound messages.


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; } td{ vertical-align: top; text-align: left; border-bottom: hidden; padding: 5px; }


```text
$   rails generate controller Plivo sms


```


This command generates a controller named plivo_controller in the app/controllers/ directory and a respective view in the app/views/plivo directory. We can delete the view as we don’t need it.


```text
$   rm   app/views/plivo/sms.html.erb


```


## Create the autoresponder application using Rails server


Edit app/controllers/plivo_controller.rb and paste into it this code.


```text
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
include    Plivo
include    Plivo  ::  Exceptions
include    Plivo  ::  XML
class    PlivoController    <    ApplicationController
skip_before_action    :verify_authenticity_token
def    receive  -  mms
from_number    =    params  [  :From  ]
to_number    =    params  [  :To  ]
text    =    params  [  :Text  ]
media_url    =    params  [  :Media0  ]
puts    “Message received - From:   #{  from_number  }  , To:   #{  to_number  }  , Text:   #{  text  }  , Media:   #{  media_url  }  ”
<span class="k">if</span> <span class="n">text</span><span class="p">.</span><span class="nf">downcase</span> <span class="o">==</span> <span class="s2">"hi"</span>
<span class="n">text</span> <span class="o">=</span> <span class="s2">"Hello!"</span>
&nbsp; 		<span class="n">media</span> <span class="o">=</span> <span class="p">[</span><span class="s2">"https://media.giphy.com/media/888R35MJTmDxQfRzfS/giphy.gif"</span><span class="p">]</span>
<span class="k">elsif</span> <span class="n">text</span><span class="p">.</span><span class="nf">downcase</span> <span class="o">==</span> <span class="s2">"bye"</span>
<span class="n">text</span> <span class="o">=</span> <span class="s2">"Bye and have a nice day!"</span>
<span class="n">media</span> <span class="o">=</span> <span class="p">[</span><span class="s2">"https://media.giphy.com/media/QM5lHSyFjz1XW/giphy.gif"</span><span class="p">]</span>
<span class="k">else</span>
&nbsp; &nbsp;<span class="n">text</span> <span class="o">=</span> <span class="s2">"I'm glad that we connected"</span>
&nbsp; &nbsp; &nbsp; &nbsp;<span class="n">media</span> <span class="o">=</span> <span class="p">[</span><span class="s2">"https://media.giphy.com/media/888R35MJTmDxQfRzfS/giphy.gif"</span><span class="p">]</span>
<span class="k">end</span>


<span class="n">api</span> <span class="o">=</span> <span class="no">RestClient</span><span class="p">.</span><span class="nf">new</span><span class="p">(</span><span class="s2">"&lt;auth_id&gt;"</span><span class="p">,</span><span class="s2">"&lt;auth_token&gt;"</span><span class="p">)</span>
<span class="n">response</span> <span class="o">=</span> <span class="n">api</span><span class="p">.</span><span class="nf">messages</span><span class="p">.</span><span class="nf">create</span><span class="p">(</span>
<span class="n">src</span><span class="ss">:to_number</span><span class="p">,</span>
<span class="n">dst</span><span class="ss">:from_number</span><span class="p">,</span>
<span class="n">text</span><span class="ss">:body</span><span class="p">,</span>
<span class="ss">media_urls: </span><span class="n">media</span><span class="p">,</span>
<span class="ss">type: </span><span class="s2">"mms"</span>
)
render    json:   response  .  to_s
end
end


```


### Add a route


Edit config/routes.rb and change the line


```text
Rails  .  application  .  routes  .  draw    do
get    ‘plivo/sms’
end


```


to


```text
Rails  .  application  .  routes  .  draw    do
post    ‘plivo/receive-mms/’    =>    ‘plivo#receivemms’
end


```


## Test


Start the Rails server


```text
$   rails server


```


You should see your basic server application in action at[http://localhost:3000/plivo/receive-mms/](http://localhost:3000/plivo/receive-mms/) .


Replace the auth placeholders with your authentication credentials from the[Plivo console](https://console.plivo.com/dashboard/)


**Note:**


- We recommend that you store your credentials in the auth_id and auth_token environment variables, to avoid the possibility of accidentally committing them to source control. If you do this, you can initialize the client with no arguments and Plivo will automatically fetch the values from the environment variables. You can use os module(os.environ) to store environment variables and fetch them when initializing the client.
- Sending and receiving MMS is only available in the United States and Canada.


[Expose your local server to the internet.](https://www.plivo.com/docs/sdk/server/set-up-ruby-dev-environment-api-messaging/#ngrok-setup)


## Create a Plivo application


Associate the Rails server you created with Plivo by creating a Plivo application. Visiting Messaging >[Applications](https://console.plivo.com/sms/applications/) and click Add New Application. You can also use Plivo’s[Application API.](https://www.plivo.com/docs/account/api/application/#create-an-application)


Give your application a name - we called our Receive-MMS. Enter the server URL you want to use (for example https://<yourdomain>.com/receive-mms/) in the Message URL field and set the method to POST. Click Create Application to save your application.


## Assign a Plivo number to your application


Navigate to the[Numbers](https://console.plivo.com/number/) page and select the phone number you want to use for this application. From the Application Type drop-down, select XML Application. From the Plivo Application drop-down, select Respond-MMS (the name we gave the application).


Click *Update Number* to save.


## Test


Send a text message to the[Plivo number](https://console.plivo.com/active-phone-numbers/) you specified using any phone. The message should be replied to the destination number you specified.


**Note:** If you’re using a Plivo Trial account, you can make calls only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
