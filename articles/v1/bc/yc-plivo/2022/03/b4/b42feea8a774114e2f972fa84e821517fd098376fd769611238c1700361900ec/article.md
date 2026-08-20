---
schema_version: "1.0.0"
document_id: "b42feea8a774114e2f972fa84e821517fd098376fd769611238c1700361900ec"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-connect-second-person/"
published_at: "2022-03-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:551ed1083bf9124d483d33960bccb70d8975956478cbb1e5476fe8788062a67c"
---

# How to Connect a Call to a Second Person the Low-Code Way Using PHLO

## Overview


[Click-to-call](https://www.plivo.com/docs/voice/use-cases/click-to-call/) enables your website users to engage with your support and sales teams on the website itself. Sometimes they want to speak to someone via their handset but initiate the call online or talk to someone directly from the website. You can implement this click-to-call use case using[Plivo’s Browser SDK](https://www.plivo.com/docs/sdk/client/browser/overview/) .


## How it works


- Browser call
- Click to call


The[Plivo Browser SDK](https://www.plivo.com/docs/sdk/client/browser/reference/) lets you make and receive calls using Plivo applications directly from any web browser.


User enters their phone number in the settings. When a call is placed, the user's handset is called first, then the call is connected to the destination number.


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. You must have a voice-enabled Plivo phone number to receive incoming calls; you can rent numbers from the[Numbers](https://console.plivo.com/active-phone-numbers/) page of the Plivo console, or by using the[Numbers API](https://www.plivo.com/docs/numbers/) . Click-to-call requires JavaScript; we recommend using Node.js. If this is your first time triggering a PHLO with Node.js, follow our instructions to[set up a Node.js development environment](https://www.plivo.com/docs/sdk/server/set-up-node-dev-environment-api-xml-voice/) and a web server and safely expose that server to the internet.


### Create a PHLO to handle call logic


‍


To create a PHLO, visit the[PHLO](https://console.plivo.com/phlo/list/) page of the Plivo console. If this is your first PHLO, the PHLO page will be empty.


- Click **Create New PHLO** .
- In the **Choose your use case** pop-up, click **Build my own** . The PHLO canvas will appear with the **Start** node.


**Note:** The Start node is the starting point of any PHLO. It lets you trigger a PHLO to start upon one of three actions: incoming SMS message, incoming call, or API request.


- Click the **Start** node to open the Configuration tab, and then enter the information to retrieve from the HTTP Request payload - in this case key names are destinationNumber and phoneMeNumber. The values will remain blank as we will receive them when the request is made by the browser.
- Validate the configuration by clicking **Validate** . Do the same for each node as you go along.
- From the list of components on the left side, drag and drop the **Initiate Call** component onto the canvas. This adds an Initiate Call node onto the canvas. When a component is placed on the canvas it becomes a node.
- Draw a line to connect the **Start** node’s **API Request** trigger state to the **Initiate Call** node.
- In the Configuration tab of the **Initiate Call** node, give the node a name. To enter values for the **From** and **To** fields, enter two curly brackets to view all available variables, and choose the appropriate ones. The values for the numbers will be retrieved from the HTTP Request payload you defined in the Start node. Here **From** is 14159142884 and **To** is {{Start.http.params.phoneMeNumber}}.
- From the list of components on the left side, drag and drop the **Call Forward** component onto the canvas. Draw a line to connect the **Answered** trigger state of the **Initiate Call** node with the **Call Forward** node.
- Configure the **Call Forward** node to initiate call forward to another user. To enter values for the **From** and **To** fields, enter two curly brackets to view all available variables, and choose the appropriate ones. The values for the numbers will be retrieved from the HTTP Request payload you defined in the Start node. Here **From** is {{Start.http.params.phoneMeNumber}} and **To** is {{Start.http.params.destinationNumber}}.
- After you complete and validate the node configurations, give the PHLO a name by clicking in the upper left, then click **Save** .
- From the list of components on the left side, drag and drop the **Call Forward** component onto the canvas.
- Draw a line to connect the **Start** node’s **Incoming call** trigger state to the **Call Forward** node.
- In the Configuration tab of the **Call Forward** node, give the node a name. To enter values for the **From** and **To** fields, enter two curly brackets to view all available variables, and choose the appropriate ones. The values for the numbers will be retrieved from the HTTP Request payload you defined in the Start node. Here **From** is {{Start.http.params.header1}}. and **To** is {{Start.http.params.to}}.


Your complete PHLO should look like this:


## Set up the demo application locally


Download and modify the code to trigger the PHLO.


- Clone the repository from[GitHub](https://github.com/plivo/click2call-webRTC.git) .


Comment */ .highlight .err { color: #a61717; /* background-color: #e3d2d2 */ } /* Error */ .highlight .k { color: #00A0DB} /* Keyword */ .highlight .cm { color: #008800; font-style: italic } /* Comment.Multiline */ .highlight .cp { color: #008080 } /* Comment.Preproc */ .highlight .c1 { color: #008800; font-style: italic } /* Comment.Single */ .highlight .cs { color: #008800; font-weight: bold } /* Comment.Special */ .highlight .gd { color: #000000; background-color: #ffdddd } /* Generic.Deleted */ .highlight .ge { font-style: italic } /* Generic.Emph */ .highlight .gr { color: #aa0000 } /* Generic.Error */ .highlight .gh { color: #999999 } /* Generic.Heading */ .highlight .gi { color: #000000; background-color: #ddffdd } /* Generic.Inserted */ .highlight .go { color: #888888 } /* Generic.Output */ .highlight .gp { color: #555555 } /* Generic.Prompt */ .highlight .gs { font-weight: bold } /* Generic.Strong */ .highlight .gu { color: #aaaaaa } /* Generic.Subheading */ .highlight .gt { color: #aa0000 } /* Generic.Traceback */ .highlight .kc { color: #00A0DB; font-weight: bold } /* Keyword.Constant */ .highlight .kd { color: #00A0DB; font-weight: bold } /* Keyword.Declaration */ .highlight .kn { color: #00A0DB} /* Keyword.Namespace */ .highlight .kp { color: #00A0DB; font-weight: bold } /* Keyword.Pseudo */ .highlight .kr { color: #00A0DB; font-weight: bold } /* Keyword.Reserved */ .highlight .kt { color: #00A0DB; font-weight: bold } /* Keyword.Type */ .highlight .m { color: #ff8045 } /* Literal.Number */ .highlight .s { color: #ff8045 } /* Literal.String */ .highlight .na { color: #FF0000 } /* Name.Attribute */ .highlight .nt { color: #00A0DB} /* Name.Tag */ .highlight .ow { font-weight: bold } /* Operator.Word */ .highlight .w { color: #bbbbbb } /* Text.Whitespace */ .highlight .mf { color: #ff8045 } /* Literal.Number.Float */ .highlight .mh { color: #ff8045 } /* Literal.Number.Hex */ .highlight .mi { color: #ff8045 } /* Literal.Number.Integer */ .highlight .mo { color: #ff8045 } /* Literal.Number.Oct */ .highlight .sb { color: #ff8045 } /* Literal.String.Backtick */ .highlight .sc { color: #800080 } /* Literal.String.Char */ .highlight .sd { color: #ff8045 } /* Literal.String.Doc */ .highlight .s2 { color: #ff8045 } /* Literal.String.Double */ .highlight .se { color: #ff8045 } /* Literal.String.Escape */ .highlight .sh { color: #ff8045 } /* Literal.String.Heredoc */ .highlight .si { color: #ff8045 } /* Literal.String.Interpol */ .highlight .sx { color: #ff8045 } /* Literal.String.Other */ .highlight .sr { color: #ff8045 } /* Literal.String.Regex */ .highlight .s1 { color: #ff8045 } /* Literal.String.Single */ .highlight .ss { color: #ff8045 } /* Literal.String.Symbol */ .highlight .il { color: #ff8045 } /* Literal.Number.Integer.Long */


pre code, pre { font-size: inherit; color: #d3d3d3; word-break: normal; font: 16px soleil; line-height: 29px; padding: 15px 18px 15px 18px; } pre{ background: rgb(33, 33, 48); min-width: 100% padding-left: 18px } .rouge-table pre{ padding: 0; }


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; } td{ vertical-align: top; text-align: left; border-bottom: hidden; padding: 5px; }


```text
$   git clone https://github.com/plivo/click2call-webRTC.git


```


- Change your working directory to click2call-webRTC.


```text
$   cd   click2call-webRTC


```


- Install the necessary dependencies using the package.json file.


```text
$   npm  install


```


- Edit the .env file. Replace the auth placeholders with your authentication credentials from the[Plivo console](https://console.plivo.com/dashboard/) . Enter your PHLO ID, which you can find on the[Plivo console](https://console.plivo.com/phlo/list/) .


```text
PORT  =  "8080"
PLIVO_AUTH_ID  =  "<auth_id>"
PLIVO_AUTH_TOKEN  =  "<auth_token>"
PHLO_ID  =  "<phlo_url>"


```


- Edit /client/src/index.jsx and replace the caller_id placeholder with a Plivo number.


```text
1
2
3
4
5
6
const    customCallerId    =    <  caller_id  >  ;
const    extraHeaders    =    {
'  X-PH-Test1  '  :    '  test1  '  ,
'  X-PH-callerId  '  :    customCallerId
};
this  .  plivoBrowserSdk  .  client  .  call  (  dest  ,    extraHeaders  );


```


## A review of the code


Let‘s walk through what the code does. The PHLO can be triggered either by an incoming call or an HTTP request.


### Broswer SDK call


When someone clicks on an application button to initiate a call, we can use the Browser SDK‘s call() method to initiate a call from the application endpoint to the destination phone number. In this case our PHLO is the endpoint, so our outbound call is treated as an *incoming* call to our PHLO. When the request we make from the browser reaches the endpoint, the browser is connected to Plivo via the endpoint and the endpoint is attached to the PHLO, so when the browser makes a request to Plivo as an incoming call, Plivo connects to the endpoint, which in turn triggers the PHLO to forward the call to the destination number.


The code looks like this.


```text
1
2
3
4
5
6
const    customCallerId    =    <  caller_id  >  ;
const    extraHeaders    =    {
'  X-PH-Test1  '  :    '  test1  '  ,
'  X-PH-callerId  '  :    customCallerId
};
this  .  plivoBrowserSdk  .  client  .  call  (  dest  ,    extraHeaders  );


```


Here the extraHeaders is used to pass the caller_id for a call initiated by the broswer.


### Click-to-call


Click-to-call is a more complicated use case because it requires us to actually send an HTTP request with a payload to the PHLO endpoint. Remember that we‘re making a call to our user‘s handset first, then connecting to the destination once the first call is answered. We need to get both phone numbers from the application and send them to the server. The code looks like this.


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
let    XMLReq    =    new    XMLHttpRequest  ();
XMLReq  .  open  (  "  POST  "  ,    "  /makeCall  "  );
XMLReq  .  setRequestHeader  (  "  Content-Type  "  ,    "  application/json  "  );
XMLReq  .  onreadystatechange    =    function  ()    {
console  .  log  (  '  response text  '  ,    XMLReq  .  responseText  );
}
XMLReq  .  send  (  JSON  .  stringify  ({
"  src  "  :    this  .  state  .  phoneMeNumber  ,
"  dst  "  :    dest
}));


```


We need to listen for this request on the server. Once we receive the request and get the numbers from the payload, we set up another HTTP request that sends this data to the PHLO.


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
// when we receive an http post request
app  .  post  (  '  /makeCall/  '  ,    function  (  req  ,    res  )    {
console  .  log  (  req  .  fields  );
jsonObject    =    JSON  .  stringify  ({
“  phoneMeNumber  ”  :    req  .  fields  .  src  ,
“  destinationNumber  ”  :    req  .  fields  .  dst  ,
});


// prepare the header
let    postHeaders    =    {
‘  Content-Type  ’  :    ‘  application/json  ’  ,
‘  Authorization  ’  :    ‘  Basic   ’    +    new    Buffer  .  from  (  process  .  env  .  <  auth_id  >    +  ’  :  ’    +    process  .  env  .  <  auth_token  >  ).  toString  (  ‘  base64  ’  )
};


// set the post options
let    postOptions    =    {
port  :    443  ,
host  :    ‘  phlo-runner-service.plivo.com  ’  ,
path  :    process  .  env  .  <  phlo_id  >  ,
method  :    ‘  POST  ’  ,
headers  :    postHeaders  ,
};


// do the POST request
let    reqPost    =    https  .  request  (  postOptions  ,    function  (  response  )    {
console  .  log  (  “  statusCode:   ”  ,    response  .  statusCode  );
response  .  on  (  ‘  data  ’  ,    function  (  d  )    {
console  .  info  (  ‘  POST result:  \n  ’  );
process  .  stdout  .  write  (  d  );
console  .  info  (  ‘  \n\n  POST completed  ’  );
res  .  send  (  d  );
});
});


// write the json data
console  .  log  (  jsonObject  );
reqPost  .  write  (  jsonObject  );
reqPost  .  end  ();
reqPost  .  on  (  ‘  error  ’  ,    function  (  e  )    {    // log any errors
console  .  error  (  e  );
});
})


```


## Assign the PHLO to a Plivo number


Once you’ve created and configured your PHLO, assign it to a Plivo number.


- On the[Numbers](https://console.plivo.com/active-phone-numbers/) page of the console, under **Your Numbers** , click the phone number you want to use for the PHLO.
- In the **Number Configuration** box, select **PHLO** from the **Application Type** drop-down.
- From the **PHLO Name** drop-down, select the PHLO you want to use with the phone number, then click **Update Number** .


## Test


Run these commands.


```text
npm run watch
npm run start


```


You should see your basic server application running at[http://localhost:8080/](http://localhost:8080/) .[Set up ngrok](https://www.plivo.com/docs/sdk/server/set-up-node-dev-environment-api-xml-voice/#ngrok-setup) to expose your local server to the internet. Now make a call from your browser-based application to test it.


Note: If you’re using a Plivo Trial account, you can make calls only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried[Plivo](https://www.plivo.com/) yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
