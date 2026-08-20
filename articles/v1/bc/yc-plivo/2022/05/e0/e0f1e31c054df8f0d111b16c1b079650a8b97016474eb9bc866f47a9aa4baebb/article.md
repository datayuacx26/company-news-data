---
schema_version: "1.0.0"
document_id: "e0f1e31c054df8f0d111b16c1b079650a8b97016474eb9bc866f47a9aa4baebb"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-receive-and-respond-incoming-mms-messages-in-php/"
published_at: "2022-05-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:29d158de97d1644680bbee6d36a63e47d734ce04b1d405eb7a21864e9caa617a"
---

# How to Receive and Respond to Incoming MMS Messages in PHP with Laravel and Plivo

## Overview


This guide shows how to receive and automatically respond to incoming MMS messages on a[Plivo number](https://www.plivo.com/phone-numbers/) , as you might want to do for someone who’s out of the office or who leaves the company.


Here’s how to use[Plivo’s SMS APIs](https://www.plivo.com/sms/) to build this use case.


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. To receive incoming messages you must have a Plivo phone number that supports MMS; you can rent numbers from the[Numbers](https://console.plivo.com/active-phone-numbers/) page of the Plivo console or by using the[Numbers API](https://www.plivo.com/docs/numbers/) . If this is your first time using Plivo APIs, follow our instructions to[set up a PHP development environment](https://www.plivo.com/docs/sdk/server/set-up-php-dev-environment-api-messaging/) and a web server and safely expose that server to the internet.


## Create a Laravel controller to receive and respond to messages


Change to the project directory and run this command.


.highlight pre{ background-color: rgb(33, 33, 48); border-radius: 0; padding: 15px 18px 15px 18px; } pre.lineno{ color: #fff; opacity: .3; } .w-richtext figure { max-width: 100%; position: relative; }


```text
$   php artisan make:controller SMSController


```


This command generates a controller named SMSController in the app/http/controllers/ directory. Edit app/http/controllers/SMSController.php and paste into it this code.


```text
<?php
namespace    App\Http\Controllers  ;
use    Illuminate\Http\Request  ;
use    Plivo\XML\Response  ;
class    SMSController    extends    Controller
{
public    function    autoresponder  ()
{
$from_number    =    $_POST  [  “From”  ];
$to_number    =    $_POST  [  “To”  ];
$text    =    $_POST  [  “Text”  ];
$media_url    =    $_REQUEST  [  “Media0”  ];
echo  (  “Message received - From   $from_number  , To:   $to_number  , Text:   $text  , Media:   $media_url  ”  );
if    (  strtolower  (  $text  )    ==    “hi”  )
$text    =    “Hello!”
$media    =    [  “ https://media.giphy.com/media/888R35MJTmDxQfRzfS/giphy.gif  ”  ]
else    if    (  strtolower  (  $text  )    ==    “bye”  )
$text    =    “Bye and have a nice day!”
$media    =    [  “ https://media.giphy.com/media/QM5lHSyFjz1XW/giphy.gif  ”  ]
else
$text    =    “I’m glad that we connected”
$client    =    new    RestClient  (  “<auth_id>”  ,  “<auth_token>”  );
$client  ->  client  ->  setTimeout  (  40  );
$response    =    $client  ->  messages  ->  create  (
[
“src”    =>    $to_number  ,
“dst”    =>    $from_number  ,
“text”     =>  $body  ,
“type”    =>    “mms”  ,
“media_urls”    =>    $media  ,
]);
echo    json_encode  (  $response  );
}
}


```


Replace the auth placeholders with your authentication credentials from the[Plivo console](https://console.plivo.com/dashboard/) . Replace the phone number placeholders with actual phone numbers in[E.164 format](https://en.wikipedia.org/wiki/E.164) (for example, +12025551234). In countries other than the US and Canada you can use a[sender ID](https://www.plivo.com/docs/sms/concepts/sender-id-usage/) for the message source. You must have a Plivo phone number to send messages to the US or Canada; you can buy a Plivo number from Phone Numbers >[Buy Numbers](https://console.plivo.com/phone-numbers/search/) on the Plivo console or via the[Numbers API](https://www.plivo.com/docs/numbers/api/phone-number/#buy-a-phone-number) .


### Add a route


Edit routes/web.php and add this line at the end of the file.


background-color: #e3d2d2 */ } /* Error */ .highlight .k { color: #00A0DB} /* Keyword */ .highlight .cm { color: #008800; font-style: italic } /* Comment.Multiline */ .highlight .cp { color: #008080 } /* Comment.Preproc */ .highlight .c1 { color: #008800; font-style: italic } /* Comment.Single */ .highlight .cs { color: #008800; font-weight: bold } /* Comment.Special */ .highlight .gd { color: #000000; background-color: #ffdddd } /* Generic.Deleted */ .highlight .ge { font-style: italic } /* Generic.Emph */ .highlight .gr { color: #aa0000 } /* Generic.Error */ .highlight .gh { color: #999999 } /* Generic.Heading */ .highlight .gi { color: #000000; background-color: #ddffdd } /* Generic.Inserted */ .highlight .go { color: #888888 } /* Generic.Output */ .highlight .gp { color: #555555 } /* Generic.Prompt */ .highlight .gs { font-weight: bold } /* Generic.Strong */ .highlight .gu { color: #aaaaaa } /* Generic.Subheading */ .highlight .gt { color: #aa0000 } /* Generic.Traceback */ .highlight .kc { color: #00A0DB; font-weight: bold } /* Keyword.Constant */ .highlight .kd { color: #00A0DB; font-weight: bold } /* Keyword.Declaration */ .highlight .kn { color: #00A0DB} /* Keyword.Namespace */ .highlight .kp { color: #00A0DB; font-weight: bold } /* Keyword.Pseudo */ .highlight .kr { color: #00A0DB; font-weight: bold } /* Keyword.Reserved */ .highlight .kt { color: #00A0DB; font-weight: bold } /* Keyword.Type */ .highlight .m { color: #ff8045 } /* Literal.Number */ .highlight .s { color: #ff8045 } /* Literal.String */ .highlight .na { color: #FF0000 } /* Name.Attribute */ .highlight .nt { color: #00A0DB} /* Name.Tag */ .highlight .ow { font-weight: bold } /* Operator.Word */ .highlight .w { color: #bbbbbb } /* Text.Whitespace */ .highlight .mf { color: #ff8045 } /* Literal.Number.Float */ .highlight .mh { color: #ff8045 } /* Literal.Number.Hex */ .highlight .mi { color: #ff8045 } /* Literal.Number.Integer */ .highlight .mo { color: #ff8045 } /* Literal.Number.Oct */ .highlight .sb { color: #ff8045 } /* Literal.String.Backtick */ .highlight .sc { color: #800080 } /* Literal.String.Char */ .highlight .sd { color: #ff8045 } /* Literal.String.Doc */ .highlight .s2 { color: #ff8045 } /* Literal.String.Double */ .highlight .se { color: #ff8045 } /* Literal.String.Escape */ .highlight .sh { color: #ff8045 } /* Literal.String.Heredoc */ .highlight .si { color: #ff8045 } /* Literal.String.Interpol */ .highlight .sx { color: #ff8045 } /* Literal.String.Other */ .highlight .sr { color: #ff8045 } /* Literal.String.Regex */ .highlight .s1 { color: #ff8045 } /* Literal.String.Single */ .highlight .ss { color: #ff8045 } /* Literal.String.Symbol */ .highlight .il { color: #ff8045 } /* Literal.Number.Integer.Long */


pre code, pre { font-size: inherit; color: #d3d3d3; word-break: normal; font: 16px Arial,soleil; line-height: 29px; } pre{ background: rgb(33, 33, 48); min-width: 100% } .rouge-table pre{ padding: 0; }


```text
Route::match ([  'get'  ,  'post'  ]  ,  '/autoresponder'  ,  'SMSController@autoresponder'  )  ;


```


**Note:** If you’re using Laravel 8, use the fully qualified class name for your controllers - for example:


```text
Route::match(['get', 'post'], '/sendSMS', 'App\Http\Controllers\SMSController@sendSMS');
```


For ngrok test, add this line to mylaravelapp/quickstart/app/Http/Middleware/VerifyCsrfToken.php.
protected $except = \['*'\];


Run your code.


```text
$   php artisan serve


```


You should see your basic server application in action at http://localhost:8000/autoresponder.


[Set up ngrok](https://www.plivo.com/docs/sdk/server/set-up-php-dev-environment-api-xml-voice/#ngrok-setup) to expose your local server to the internet.


## Create a Plivo application for the autoresponder


Associate the controller you created with Plivo by creating a Plivo application. Visiting Messaging >[Applications](https://console.plivo.com/sms/applications/) and click **Add New Application** . You can also use Plivo’s[Application API](https://www.plivo.com/docs/account/api/application/#create-an-application) .


Give your application a name - we called ours Autoresponder. Enter the server URL you want to use (for example https://<yourdomain>.com/autoresponder/) in the Message URL field and set the method to POST. Click **Create Application** to save your application.


## Assign a Plivo number to your application


Navigate to the[Numbers](https://console.plivo.com/number/) page and select the phone number you want to use for this application. From the Application Type drop-down, select XML Application. From the Plivo Application drop-down, select Autoresponder (the name we gave the application). Click **Update Number** to save.


## Test


Send a text message to the[Plivo number](https://console.plivo.com/active-phone-numbers/) you specified using any phone. The message should be replied to the destination number you specified.


**Note:** If you’re using a Plivo Trial account, you can make calls only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
