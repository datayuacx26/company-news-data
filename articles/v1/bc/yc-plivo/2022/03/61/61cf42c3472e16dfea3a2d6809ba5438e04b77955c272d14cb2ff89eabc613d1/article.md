---
schema_version: "1.0.0"
document_id: "61cf42c3472e16dfea3a2d6809ba5438e04b77955c272d14cb2ff89eabc613d1"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-receive-and-respond-incoming-mms-messages-in-python/"
published_at: "2022-03-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:c3dac62993fc301c1e4c905f3cc35924cac8d98e8b460e7ed44eed20fb8b3581"
---

# How to Receive and Respond to Incoming MMS Messages in Python with Flask and Plivo

You can receive and automatically respond to incoming MMS messages on a[Plivo number](https://www.plivo.com/phone-numbers/) , as you might want to do for someone who’s out of the office or who leaves the company. Here’s how to use[Plivo’s SMS API](https://www.plivo.com/sms/) to build this use case.


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. To receive incoming messages you must have a Plivo phone number that supports MMS; you can rent numbers from the[Numbers](https://console.plivo.com/active-phone-numbers/) page of the Plivo console or by using the[Numbers API](https://www.plivo.com/docs/numbers/) . If this is your first time using Plivo APIs, follow our instructions to[set up a Python development environment](https://www.plivo.com/docs/sdk/server/set-up-python-dev-environment-api-messaging/) and a web server and safely expose that server to the internet.


## Create a Flask server to receive MMS messages


Create a file called receive_mms.py and paste into it this code.


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
36
37
38
39
40
41
42
from    flask    import    Flask  ,    request
import    plivo
app    =    Flask  (  name   )
@  app  .  route  (  “/receive-mms/”  ,    methods  =  [  “GET”  ,    “POST”  ])
def    receive_mms  ():
# Receive MMS
from_number    =    request  .  values  .  get  (  “From”  )
to_number    =    request  .  values  .  get  (  “To”  )
text    =    request  .  values  .  get  (  “Text”  )
media_url    =    request  .  values  .  get  (  “Media0”  )
print  (  “MMS message received - From: %s, To: %s, Text: %s, Media: %s”  %    (  from_number  ,    to_number  ,    text  ,    media_url  ))
if    text  .  lower  ()    ==    “hi”  :
text    =    “Hello!!”
media    =    [  “ https://media.giphy.com/media/888R35MJTmDxQfRzfS/giphy.gif  ”  ]
elif    text  .  lower  ()    ==    “bye”  :
text    =    “Bye and have a nice day!”
media    =    [  “ https://media.giphy.com/media/QM5lHSyFjz1XW/giphy.gif  ”  ]
else  :
text    =    “I am glad that we connected”
# Reply recipient using Send MMS API
client    =    plivo  .  RestClient  (  “<auth_id>”  ,    “<auth_token>”  )
response    =    client  .  messages  .  create  (
src  =  to_number  ,
dst  =  from_number  ,
text  =  text  ,
media_urls  =  media  ,
type_  =  “mms”
)
print  (  response  )
return    “MMS responded”  ,    200
if    name     ==    “ main  ”  :
app  .  run  (  host  =  “0.0.0.0”  )


```


Replace the auth placeholders with your authentication credentials from the Plivo console


Note: We recommend that you store your credentials in the auth_id and auth_token environment variables, to avoid the possibility of accidentally committing them to source control. If you do this, you can initialize the client with no arguments and Plivo will automatically fetch the values from the environment variables. You can use os module(os.environ) to store environment variables and fetch them when initializing the client.


## Test


Save the file and run it.


background-color: #e3d2d2 */ } /* Error */ .highlight .k { color: #00A0DB} /* Keyword */ .highlight .cm { color: #008800; font-style: italic } /* Comment.Multiline */ .highlight .cp { color: #008080 } /* Comment.Preproc */ .highlight .c1 { color: #008800; font-style: italic } /* Comment.Single */ .highlight .cs { color: #008800; font-weight: bold } /* Comment.Special */ .highlight .gd { color: #000000; background-color: #ffdddd } /* Generic.Deleted */ .highlight .ge { font-style: italic } /* Generic.Emph */ .highlight .gr { color: #aa0000 } /* Generic.Error */ .highlight .gh { color: #999999 } /* Generic.Heading */ .highlight .gi { color: #000000; background-color: #ddffdd } /* Generic.Inserted */ .highlight .go { color: #888888 } /* Generic.Output */ .highlight .gp { color: #555555 } /* Generic.Prompt */ .highlight .gs { font-weight: bold } /* Generic.Strong */ .highlight .gu { color: #aaaaaa } /* Generic.Subheading */ .highlight .gt { color: #aa0000 } /* Generic.Traceback */ .highlight .kc { color: #00A0DB; font-weight: bold } /* Keyword.Constant */ .highlight .kd { color: #00A0DB; font-weight: bold } /* Keyword.Declaration */ .highlight .kn { color: #00A0DB} /* Keyword.Namespace */ .highlight .kp { color: #00A0DB; font-weight: bold } /* Keyword.Pseudo */ .highlight .kr { color: #00A0DB; font-weight: bold } /* Keyword.Reserved */ .highlight .kt { color: #00A0DB; font-weight: bold } /* Keyword.Type */ .highlight .m { color: #ff8045 } /* Literal.Number */ .highlight .s { color: #ff8045 } /* Literal.String */ .highlight .na { color: #FF0000 } /* Name.Attribute */ .highlight .nt { color: #00A0DB} /* Name.Tag */ .highlight .ow { font-weight: bold } /* Operator.Word */ .highlight .w { color: #bbbbbb } /* Text.Whitespace */ .highlight .mf { color: #ff8045 } /* Literal.Number.Float */ .highlight .mh { color: #ff8045 } /* Literal.Number.Hex */ .highlight .mi { color: #ff8045 } /* Literal.Number.Integer */ .highlight .mo { color: #ff8045 } /* Literal.Number.Oct */ .highlight .sb { color: #ff8045 } /* Literal.String.Backtick */ .highlight .sc { color: #800080 } /* Literal.String.Char */ .highlight .sd { color: #ff8045 } /* Literal.String.Doc */ .highlight .s2 { color: #ff8045 } /* Literal.String.Double */ .highlight .se { color: #ff8045 } /* Literal.String.Escape */ .highlight .sh { color: #ff8045 } /* Literal.String.Heredoc */ .highlight .si { color: #ff8045 } /* Literal.String.Interpol */ .highlight .sx { color: #ff8045 } /* Literal.String.Other */ .highlight .sr { color: #ff8045 } /* Literal.String.Regex */ .highlight .s1 { color: #ff8045 } /* Literal.String.Single */ .highlight .ss { color: #ff8045 } /* Literal.String.Symbol */ .highlight .il { color: #ff8045 } /* Literal.Number.Integer.Long */


pre code, pre { font-size: inherit; color: #d3d3d3; word-break: normal; font: 16px soleil; line-height: 29px; padding: 15px 18px 15px 18px; } pre{ background: rgb(33, 33, 48); min-width: 100% padding-left: 18px } .rouge-table pre{ padding: 0; }


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; } td{ vertical-align: top; text-align: left; border-bottom: hidden; padding: 5px; }


```text
$   python receive_mms.py


```


You should see your basic server application in action at[http://localhost:5000/receive-mms/](http://localhost:5000/receive-mms/) .


**Note:** Sending and receiving MMS is only available in the United States and Canada.


[Expose your local server to the internet](https://www.plivo.com/docs/sdk/server/set-up-python-dev-environment-api-messaging/#ngrok-setup) .


## Create a Plivo application


Associate the Flask server you created with Plivo by creating a Plivo application. Visiting Messaging >[Applications](https://console.plivo.com/sms/applications/) and click Add New Application. You can also use Plivo’s[Application API.](https://www.plivo.com/docs/account/api/application/#create-an-application)


Give your application a name - we called our Receive-MMS. Enter the server URL you want to use (for example https://<yourdomain>.com/receive-mms/) in the Message URL field and set the method to POST. Click **Create Application** to save your application.


## Assign a Plivo number to your application


Navigate to the[Numbers](https://console.plivo.com/number/) page and select the phone number you want to use for this application. From the Application Type drop-down, select XML Application. From the Plivo Application drop-down, select Respond-MMS (the name we gave the application).


Click **Update Number** to save.


## Test


Send a text message to the[Plivo number](https://console.plivo.com/active-phone-numbers/) you specified using any phone. The message should be replied to the destination number you specified.


Note: If you’re using a Plivo Trial account, you can make calls only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
