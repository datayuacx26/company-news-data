---
schema_version: "1.0.0"
document_id: "ce1e8cf1596c4f69b4c0c9b74d72ddab97e184249d531c766c2a1eb42946f696"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-build-a-voice-notification-app/"
published_at: "2022-02-22T00:00:00+00:00"
first_seen_at: "2026-07-21T11:30:36.100142+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:53dae34732da4c4b7ee967cccf5a5b4ead2f8997dfa183d98e83b66a316d9bcf"
---

# How to Build a Voice Notifications Application the Low-Code Way Using PHLO

You can play recorded audio when a call recipient answers or use text-to-speech, as we show here, combining static text with dynamic information that Plivo gets from a variable. You can create and deploy a PHLO to send voice notifications with a few clicks on the PHLO canvas.


## How it works


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. If this is your first time triggering a PHLO, follow our instructions to[set up a development environment](https://www.plivo.com/docs/sdk/server/) available in different languages.


## Create the PHLO


To create a PHLO, visit the[PHLO](https://console.plivo.com/phlo/list/) page of the Plivo console. If this is your first PHLO, the PHLO page will be empty.


- Click **Create New PHLO** .
- In the **Choose your use case** pop-up, click **Build my own** . The PHLO canvas will appear with the **Start** node.
- **Note:** The Start node is the starting point of any PHLO. It lets you trigger a PHLO to start upon one of three actions: incoming SMS message, incoming call, or API request.
- Click the **Start** node to open the Configurations tab, then enter the information to retrieve from the HTTP Request payload - in this case, the From and To numbers for the call, and an item number.


- From the list of components on the left side, drag and drop the **Initiate Call** component onto the canvas. This adds an Initiate Call node onto the canvas. When a component is placed on the canvas it becomes a node.
- Draw a line to connect the **Start** node’s **API Request** trigger state to the **Initiate Call** node.


- In the Configuration tab of the Initiate Call node, give the node a name. To enter values for the **From** and **To** fields, enter two curly brackets to view all available variables, and choose the appropriate ones. The values for the numbers will be retrieved from the HTTP Request payload you defined in the Start node.


- Validate the configuration by clicking **Validate** . Do the same for each node as you go along.
- Next, create a node from the **Play Audio** component. Connect the **Initiate Call** node to the **Play Audio** node using the **Answered** trigger state.


- Configure the **Play Audio** node to play a message to the user by entering text in the Speak Text box in the Prompt section of the Configuration pane.
- Audio playback can either be static or dynamic. You define a static payload by specifying values when you create the PHLO, and a dynamic payload by passing values through[Liquid](https://shopify.github.io/liquid/) templating parameters when you trigger the PHLO from your application.
- On the **Play Audio** Configuration tab, enter a static message (for example, “Your order has been successfully placed”) in the **Speak Text** field, with a variable to include the dynamic text. Enter two curly brackets to view all available variables. Choose the item number you defined in the Start node configuration tab.


- After you complete and validate the node configurations, give the PHLO a name by clicking in the upper left, then click **Save** .


Your completed PHLO should look like this:


Your PHLO is now ready to test.


## Trigger the PHLO


You integrate a PHLO into your application workflow by making an API request to trigger the PHLO with the required payload - the set of parameters you pass to the PHLO. You can define a static payload by specifying values when you create the PHLO, or define a dynamic payload by passing values through parameters when you trigger the PHLO from your application.


### With a static payload


When you configure values when creating the PHLO, they act as a static payload.


#### Code


Create a file and paste the below code.


Replace the auth placeholders with your authentication credentials from the[Plivo console](https://console.plivo.com/dashboard/) . Replace the phlo_id placeholder with your PHLO ID from the[Plivo console](https://console.plivo.com/phlo/list/) .


### With a dynamic payload


To use dynamic values for the parameters, use Liquid templating parameters when you create the PHLO and pass the values from your code to the PHLO when you trigger it.


#### Code


Create a file and paste the below code.


- Python
- Ruby
- Node.js
- PHP
- Java
- Go
- .NET
- Curl


```text
1
2
3
4
5
6
7
import    plivo
phlo_id    =    “<phlo_id>”
phlo_client    =    plivo  .  phlo  .  RestClient  (  “<auth_id>”  ,    “<auth_token>”  )
phlo    =    phlo_client  .  phlo  .  get  (  phlo_id  )
response    =    phlo  .  run  ()
print  (  response  )


```


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
require    'rubygems'
require    'plivo'
include    Plivo
AUTH_ID    =    ‘<auth_id>’
AUTH_TOKEN    =    ‘<auth_token>’
client    =    Phlo  .  new  (  AUTH_ID  ,    AUTH_TOKEN  )
# if credentials are stored in the PLIVO_AUTH_ID and the PLIVO_AUTH_TOKEN environment variables
# then initialize client as:
# client = Phlo.new
begin
phlo    =    client  .  phlo  .  get  (  ‘<phlo_id>’  )
response    =    phlo  .  run  ()
puts    response
rescue    PlivoRESTError    =>    e
puts    ‘Exception: ’    +    e  .  message
end


```


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
var    plivo    =    require  (  '  plivo  '  );
var    PhloClient    =    plivo  .  PhloClient  ;
var    phloId    =    ‘  <phlo_id>  ’  ;
var    phloClient    =    phlo    =    null  ;
phloClient    =    new    PhloClient  (  ‘  <auth_id>  ’  ,    ‘  <auth_token>  ’  );
phloClient  .  phlo  (  phloId  ).  run  ().  then  (  function  (  result  )    {
console  .  log  (  ‘  Phlo run result  ’  ,    result  );
}).  catch  (  function  (  err  )    {
console  .  error  (  ‘  Phlo run failed  ’  ,    err  );
});


```


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
<?php
require    'vendor/autoload.php'  ;
use    Plivo\Resources\PHLO\PhloRestClient  ;
use    Plivo\Exceptions\PlivoRestException  ;
$client    =    new    PhloRestClient  (  "<auth_id>"  ,    "<auth_token>"  );
$phlo    =    $client  ->  phlo  ->  get  (  “<phlo_id>”  );
try    {
$response    =    $phlo  ->  run  ();
print_r  (  $response  );
}    catch    (  PlivoRestException    $ex  )    {
print_r  (  $ex  );
}


```


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
import    com.plivo.api.Plivo  ;
import    com.plivo.api.PlivoClient  ;
import    com.plivo.api.exceptions.PlivoRestException  ;
import    com.plivo.api.models.phlo.Phlo  ;
import    java.io.IOException  ;
public    class    Example
{
private    static    final    String    authId    =    “<auth_id>”  ;
private    static    final    String    authToken    =    “<auth_token>”  ;
private    static    PlivoClient    client    =    new    PlivoClient  (  authId  ,    authToken  );
public    static    void    main  (  String  []    args  )    throws    IOException  ,    PlivoRestException
{
String    phloId    =    “<phlo_id>”  ;
Plivo  .  init  (  authId  ,    authToken  );
Phlo    phlo    =    Phlo  .  getter  (  phloId  ).  client  (  client  ).  get  ();
PhloUpdateResponse    response    =    Phlo  .  updater  (  phloId  ).  payload  ().  run  ();
}
}


```


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
package    main
import    (
“fmt”
“plivo-go”
)
// Initialize the following params with corresponding values to trigger resources
const    authId    =    “<auth_id>”
const    authToken    =    “<auth_token>”
const    phloId    =    “<phlo_id>”
func    main  ()    {
testPhloRunWithoutParams  ()
}
func    testPhloRunWithoutParams  ()    {
phloClient  ,    err    :=    plivo  .  NewPhloClient  (  authId  ,    authToken  ,    &  plivo  .  ClientOptions  {})
if    err    !=    nil    {
fmt  .  Print  (  “Error”  ,    err  .  Error  ())
return
}
phloGet  ,    err    :=    phloClient  .  Phlos  .  Get  (  phloId  )
if    err    !=    nil    {
fmt  .  Print  (  “Error”  ,    err  .  Error  ())
return
}
response  ,    err    :=    phloGet  .  Run  (  nil  )
if    err    !=    nil    {
fmt  .  Print  (  “Error”  ,    err  .  Error  ())
return
}
fmt  .  Printf  (  “Response: %#v  \n  ”  ,    response  )
}


```


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
using    System  ;
using    Plivo  ;
namespace    test_PHLO
{
class    Program
{
public    static    void    Main  (  string  []    args  )
{
var    phloClient    =    new    PhloApi  (  “<auth_id>”  ,    “<auth_token>”  );
var    phloID    =    “<phlo_id>”  ;
var    phlo    =    phloClient  .  Phlo  .  Get  (  phloID  );
Console  .  WriteLine  (  phlo  .  Run  ());
}
}
}


```


```text
1
2
3
4
curl  --request   POST  \
--user   AUTH_ID:AUTH_TOKEN  \
--url    'https://phlorunner.plivo.com/v1/account/{auth_id}/phlo/{phlo_id}'    \
--header    'Content-Type: application/json'


```


Replace the auth placeholders with your authentication credentials from the[Plivo console](https://console.plivo.com/dashboard/) . Replace the phlo_id placeholder with your PHLO ID from the[Plivo console](https://console.plivo.com/phlo/list/) . Replace the phone number placeholders with actual phone numbers in[E.164 format](https://en.wikipedia.org/wiki/E.164) (for example, +12025551234).


## Test


Save the file and run it.


**Note:** If you’re using a Plivo Trial account, you can make calls only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers >[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried[Plivo](https://www.plivo.com/) yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
