---
schema_version: "1.0.0"
document_id: "cbdca14b5d71f3c85b068a3ff06b77fd2d41611f3b20a5445355272daa5c5108"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-receive-and-respond-incoming-mms-messages-in-node/"
published_at: "2022-05-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:e0a962c491c6e65a08376a23ab781a84ef4669fd5e0da39355fbb253c4d36b76"
---

# How to Receive and Respond to Incoming MMS Messages Using Node.js with Express and Plivo

## Overview


This guide shows how to receive and automatically respond to incoming MMS messages on a[Plivo number](https://www.plivo.com/phone-numbers/) , as you might want to do for someone who’s out of the office or who leaves the company.


Here’s how to use[Plivo’s SMS APIs](https://www.plivo.com/sms/) to build this use case.


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. To receive incoming messages, you must have a Plivo phone number that supports SMS; you can rent numbers from the[Numbers](https://console.plivo.com/active-phone-numbers/) page of the Plivo console or by using the[Numbers API](https://www.plivo.com/docs/numbers/) . If this is your first time using Plivo APIs, follow our instructions to[set up a Node.js development environment](https://www.plivo.com/docs/sdk/server/set-up-node-dev-environment-api-messaging/) .


## Create the autoresponder application using Express


Create a file called autoresponder.js and paste into it this code.


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
43
44
45
46
47
48
49
const    plivo    =    require  (  '  plivo  '  );
const    express    =    require  (  '  express  '  );
const    bodyParser    =    require  (  '  body-parser  '  );
const    app    =    express  ();
app  .  use  (  bodyParser  .  urlencoded  ({
extended  :    true
}));
app  .  use  (  function  (  req  ,    response  ,    next  )    {
response  .  contentType  (  ‘  application/xml  ’  );
next  ();
});
app  .  set  (  ‘  port  ’  ,    (  process  .  env  .  PORT    ||    3000  ));
app  .  all  (  ‘  /autoresponder/  ’  ,    function  (  request  ,    response  )    {
let    from_number    =    request  .  body  .  From    ||    request  .  query  .  From  ;
let    to_number    =    request  .  body  .  To    ||    request  .  query  .  To  ;
let    text    =    request  .  body  .  Text    ||    request  .  query  .  Text  ;
let    media    =    request  .  body  .  Media0    ||    request  .  query  .  Media0  ;
console  .  log  (  ‘  Message received - From:   ’    +    from_number    +    ’  , To:   ’    +    to_number    +    ’  , Text:   ’    +    text  +    ’  , Media:   ’    +    media  );
if    (  text  .  toLowerCase  ()    ==    ‘  hi  ’  )    {
text    =    “  Hello!  ”
media    =    [  “  https://media.giphy.com/media/888R35MJTmDxQfRzfS/giphy.gif   ”  ]
}
else    if    (  text  .  toLowerCase  ()    ==    ‘  bye  ’  )
{
body    =    “  Bye and have a nice day!  ”
media    =    [  “  https://media.giphy.com/media/QM5lHSyFjz1XW/giphy.gif   ”  ]
}
else
{
body    =    “  I’m glad that we connected  ”
}


var    client    =    new    plivo  .  Client  (  “  <auth_id>  ”  ,    “  <auth_token>  ”  );
client  .  messages  .  create  ({
src  :    to_number  ,
dst  :    from_number  ,
text  :    body  ,
type  :    “  mms  ”  ,
media_urls  :    media
}).  then  (  function  (  response  )    {
console  .  log  (  response  );
});
});
app  .  listen  (  app  .  get  (  ‘  port  ’  ),    function    ()    {
console  .  log  (  ‘  Node app is running on port  ’  ,    app  .  get  (  ‘  port  ’  ));
});


```


Save the file and run it.


background-color: #e3d2d2 */ } /* Error */ .highlight .k { color: #00A0DB} /* Keyword */ .highlight .cm { color: #008800; font-style: italic } /* Comment.Multiline */ .highlight .cp { color: #008080 } /* Comment.Preproc */ .highlight .c1 { color: #008800; font-style: italic } /* Comment.Single */ .highlight .cs { color: #008800; font-weight: bold } /* Comment.Special */ .highlight .gd { color: #000000; background-color: #ffdddd } /* Generic.Deleted */ .highlight .ge { font-style: italic } /* Generic.Emph */ .highlight .gr { color: #aa0000 } /* Generic.Error */ .highlight .gh { color: #999999 } /* Generic.Heading */ .highlight .gi { color: #000000; background-color: #ddffdd } /* Generic.Inserted */ .highlight .go { color: #888888 } /* Generic.Output */ .highlight .gp { color: #555555 } /* Generic.Prompt */ .highlight .gs { font-weight: bold } /* Generic.Strong */ .highlight .gu { color: #aaaaaa } /* Generic.Subheading */ .highlight .gt { color: #aa0000 } /* Generic.Traceback */ .highlight .kc { color: #00A0DB; font-weight: bold } /* Keyword.Constant */ .highlight .kd { color: #00A0DB; font-weight: bold } /* Keyword.Declaration */ .highlight .kn { color: #00A0DB} /* Keyword.Namespace */ .highlight .kp { color: #00A0DB; font-weight: bold } /* Keyword.Pseudo */ .highlight .kr { color: #00A0DB; font-weight: bold } /* Keyword.Reserved */ .highlight .kt { color: #00A0DB; font-weight: bold } /* Keyword.Type */ .highlight .m { color: #ff8045 } /* Literal.Number */ .highlight .s { color: #ff8045 } /* Literal.String */ .highlight .na { color: #FF0000 } /* Name.Attribute */ .highlight .nt { color: #00A0DB} /* Name.Tag */ .highlight .ow { font-weight: bold } /* Operator.Word */ .highlight .w { color: #bbbbbb } /* Text.Whitespace */ .highlight .mf { color: #ff8045 } /* Literal.Number.Float */ .highlight .mh { color: #ff8045 } /* Literal.Number.Hex */ .highlight .mi { color: #ff8045 } /* Literal.Number.Integer */ .highlight .mo { color: #ff8045 } /* Literal.Number.Oct */ .highlight .sb { color: #ff8045 } /* Literal.String.Backtick */ .highlight .sc { color: #800080 } /* Literal.String.Char */ .highlight .sd { color: #ff8045 } /* Literal.String.Doc */ .highlight .s2 { color: #ff8045 } /* Literal.String.Double */ .highlight .se { color: #ff8045 } /* Literal.String.Escape */ .highlight .sh { color: #ff8045 } /* Literal.String.Heredoc */ .highlight .si { color: #ff8045 } /* Literal.String.Interpol */ .highlight .sx { color: #ff8045 } /* Literal.String.Other */ .highlight .sr { color: #ff8045 } /* Literal.String.Regex */ .highlight .s1 { color: #ff8045 } /* Literal.String.Single */ .highlight .ss { color: #ff8045 } /* Literal.String.Symbol */ .highlight .il { color: #ff8045 } /* Literal.Number.Integer.Long */


pre code, pre { font-size: inherit; color: #d3d3d3; word-break: normal; font: 16px soleil; line-height: 29px; padding: 15px 18px 15px 18px; } pre{ background: rgb(33, 33, 48); min-width: 100% padding-left: 18px } .rouge-table pre{ padding: 0; }


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; } td{ vertical-align: top; text-align: left; border-bottom: hidden; padding: 5px; }


```text
$   node autoresponder.js


```


You should see your basic server application in action at http://localhost:3000/autoresponder/.


[Set up ngrok](https://www.plivo.com/docs/sdk/server/set-up-node-dev-environment-api-xml-voice/#ngrok-setup) to expose your local server to the internet.


## Create a Plivo application for the autoresponder


Associate the controller you created with Plivo by creating a Plivo application. Visiting Messaging >[Applications](https://console.plivo.com/sms/applications/) and click **Add New Application** . You can also use Plivo’s[Application API](https://www.plivo.com/docs/account/api/application/#create-an-application) .


Give your application a name - we called ours Autoresponder. Enter the server URL you want to use (for example https://<yourdomain>.com/autoresponder/) in the Message URL field and set the method to POST. Click **Create Application** to save your application.


## Assign a Plivo number to your application


Navigate to the[Numbers](https://console.plivo.com/number/) page and select the phone number you want to use for this application.
From the Application Type drop-down, select XML Application.
From the Plivo Application drop-down, select Autoresponder (the name we gave the application).


Click **Update Number** to save.


## Test


Send a text message to the Plivo number you specified using any phone. The message should reply from the destination number you specified.


**Note:** If you’re using a Plivo Trial account, you can send messages only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers >[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
