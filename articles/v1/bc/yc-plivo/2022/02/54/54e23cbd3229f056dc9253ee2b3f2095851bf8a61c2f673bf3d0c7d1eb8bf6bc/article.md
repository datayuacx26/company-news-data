---
schema_version: "1.0.0"
document_id: "54e23cbd3229f056dc9253ee2b3f2095851bf8a61c2f673bf3d0c7d1eb8bf6bc"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-build-a-voice-alerts-app/"
published_at: "2022-02-25T00:00:00+00:00"
first_seen_at: "2026-07-21T11:30:36.100142+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:e2424233b7643c46114c6c8a2209d10750d76d82bdc0717a5f07206270244d9c"
---

# How to Build a Voice Alerts Application the Low-Code Way Using PHLO

You can[make voice API](https://www.plivo.com/docs/voice/use-cases/make-outbound-calls/node/) calls to alert customers to critical issues that require immediate attention. You can play recorded audio when the call recipient answers or use text-to-speech. You can then take action based on a dialpad key they press in response. You can set different actions if the call is not answered, if the line is busy, or if you reach[voicemail](https://www.plivo.com/docs/voice/use-cases/voicemail/node/) .


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. If this is your first time triggering a PHLO, follow our instructions to[set up a development environment](https://www.plivo.com/docs/sdk/server/) available in different languages.


## Create the PHLO


To create a PHLO, visit the[PHLO](https://console.plivo.com/phlo/list/) page of the Plivo console. If this is your first PHLO, the PHLO page will be empty.


- Click **Create New PHLO** .
- In the **Choose your use case** pop-up, click **Build my own** . The PHLO canvas will appear with the **Start** node. **Note:** The Start node is the starting point of any PHLO. It lets you trigger a PHLO to start upon one of three actions: incoming SMS message, incoming call, or API request.
- Click the **Start** node to open the Configuration tab to the right of the canvas, then enter the information to retrieve from the HTTP Request payload - in this case, from and to numbers and a database server name.
- From the list of components on the left side, drag and drop the **Initiate Call** component onto the canvas. This adds an **Initiate Call** node onto the canvas. When a component is placed on the canvas it becomes a node.
- Draw a line to connect the **Start** node’s **API Request** trigger state to the **Initiate Call** node.
- In the Configuration tab of the **Initiate Call** node, give the node a meaningful name. To enter values for the From and To fields, start typing two curly brackets. PHLO will display a list of all available variables; choose the appropriate ones. When you use variables in a PHLO, the values are retrieved from the HTTP Request payload you defined in the Start node.
- Validate the configuration by clicking **Validate** . Every time you finish configuring a node, click **Validate** to check the syntax and save your changes.


- Next, create a node from the **Play Audio** component. Connect the **Initiate Call** node to the **Play Audio** node using the **Answered** trigger state.
- Click the **IVR Menu** node to open its Configuration tab. Rename the node to **Gather_Input** . You can rename nodes as you like to improve your PHLO’s readability. For this example, select **1** and **2** as the allowed choices, and enter a message to play to the user in the Speak Text field. If you like, you can also configure the Language and Voice fields for the message.


- Because we specified two allowed choices in the IVR menu, we need to drag and drop three Play Audio nodes onto the canvas - one for each option, plus one for invalid input. Rename the nodes Invalid_Input_Prompt, Resolved_Prompt, and Escalation_Prompt in their Configuration tabs, and enter appropriate messages in their Prompt fields.
- From the Gather_Input node, connect the Wrong Input trigger state to the Invalid_Input_Prompt node.


- From the Invalid_Input_Prompt node, connect the Prompt Completed trigger state back to the Gather_Input node. This sends the user back to the IVR menu if they press an incorrect option, or if they don‘t press any key.
- From the Gather_Input node, connect the 1 and 2 trigger states to the Resolved_Prompt and Escalation_Prompt nodes.
- Configure all three **Play Audio** nodes to each play a relevant message to the user. Audio playback can either be static or dynamic or a combination of the two; for example, you could specify in the Speak Text field “Your status is,” followed by a variable to include the dynamic text. You can bring up a list of available variables by typing two curly brackets in the Speak Text field.


- Drag and drop the **Initiate Call** component onto the canvas and rename the node to **Escalation_Call** .
- Draw a line to connect the Prompt Completed trigger state of the Escalation_Prompt node to the Escalation_Call node. This triggers a call to another phone number and announces the alert. You can set up any number of escalation numbers by creating similar nodes for each phone number.
- Draw a line to connect the **Answered** trigger state of the **Escalation_Call** node back to the **Gather Input** node. This will play the same prompt to the user after the original escalation call is answered and completed and give the user another chance to either resolve the call or escalate it.


- After you complete and validate all the node configurations, give the PHLO a name by clicking in the upper left, then click **Save** .


Your completed PHLO should look like this:


Your PHLO is now ready to test.


## Trigger the PHLO


You integrate a PHLO into your application workflow by making an API request to trigger the PHLO with the required payload - the set of parameters you pass to the PHLO. You can define a static payload by specifying values when you create the PHLO, or define a dynamic payload by passing values through parameters when you trigger the PHLO from your application.


### With a static payload


When you configure values when creating the PHLO, they act as a static payload.


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
8
9
10
import    plivo
auth_id    =    ‘<auth_id>’
auth_token    =    ‘<auth_token>’
phlo_id    =    ‘<phlo_id>’
payload    =    {  “From”    :    “<caller_id>”  ,  “To”    :    “<destination_number>”  }
phlo_client    =    plivo  .  phlo  .  RestClient  (  auth_id  =  auth_id  ,    auth_token  =  auth_token  )
phlo    =    phlo_client  .  phlo  .  get  (  phlo_id  )
response    =    phlo  .  run  (  **  payload  )
print    (  response  )


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
require    'rubygems'
require    'plivo'
include    Plivo
AUTH_ID    =    ‘<auth_id>’
AUTH_TOKEN    =    ‘<auth_token>’
client    =    Phlo  .  new  (  AUTH_ID  ,    AUTH_TOKEN  )
# then initialize client as:
# client = Phlo.new
begin
phlo    =    client  .  phlo  .  get  (  ‘<phlo_id>’  )
#parameters set in PHLO - params
params    =    {
From  :    ‘<caller_id>’  ,
To  :    ‘<destination_number>’  ,
}
response    =    phlo  .  run  (  params  )
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
13
14
15
16
17
18
var    plivo    =    require  (  '  plivo  '  );
var    PhloClient    =    plivo  .  PhloClient  ;
var    authId    =    ‘  <auth_id>  ’  ;
var    authToken    =    ‘  <auth_token>  ’  ;
var    phloId    =    ‘  <phlo_id>  ’  ;
var    phloClient    =    phlo    =    null  ;
var    payload    =    {
From  :    ‘  <caller_id>  ’  ,
To  :    ‘  <destination_number>  ’  ,
}
phloClient    =    new    PhloClient  (  authId  ,    authToken  );
phloClient  .  phlo  (  phloId  ).  run  (  payload  ).  then  (  function    (  result  )    {
console  .  log  (  ‘  Phlo run result  ’  ,    result  );
}).  catch  (  function    (  err  )    {
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
require    ‘vendor/autoload.php’  ;
use    Plivo\Resources\PHLO\PhloRestClient  ;
use    Plivo\Exceptions\PlivoRestException  ;
$client    =    new    PhloRestClient  (  “<auth_id>”  ,    “<auth_token>”  );
$phlo    =    $client  ->  phlo  ->  get  (  “<phlo_id>”  );
try    {
$response    =    $phlo  ->  run  ([  “From”    =>    “<caller_id>”  ,    “To”    =>    “<destination_number>”  ]);
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
20
21
22
import    com.plivo.api.Plivo  ;
import    com.plivo.api.PlivoClient  ;
import    com.plivo.api.exceptions.PlivoRestException  ;
import    com.plivo.api.models.phlo.Phlo  ;
import    java.io.IOException  ;
public    class    Example
{
private    static    final    String    authId    =    “<auth_id>”  ;
private    static    final    String    authToken    =    “<auth_token>”  ;
{
String    phloId    =    “<phlo_id>”  ;
Plivo  .  init  (  authId  ,    authToken  );
Map  <  String  ,    Object  >    payload    =    new    HashMap  <>();
payload  .  put  (  “From”  ,    “<caller_id>”  );
payload  .  put  (  “To”  ,    “<destination_number>”  );
PhloUpdateResponse    response    =    Phlo  .  updater  (  phloId  ).  payload  (  payload  ).  run  ();
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
35
36
37
38
39
package    main
import    (
“fmt”
“plivo-go”
)
// Initialize these parameters with corresponding values to trigger resources
const    authId    =    “<auth_id>”
const    authToken    =    “<auth_token>”
const    phloId    =    “<phlo_id>”
func    main  ()    {
testPhloRunWithParams  ()
}
func    testPhloRunWithParams  ()    {
if    err    !=    nil    {
fmt  .  Print  (  “Error”  ,    err  .  Error  ())
return
}
phloGet  ,    err    :=    phloClient  .  Phlos  .  Get  (  phloId  )
if    err    !=    nil    {
fmt  .  Print  (  “Error”  ,    err  .  Error  ())
return
}
//pass corresponding from and to values
type    params    map  [  string  ]  interface  {}
response  ,    err    :=    phloGet  .  Run  (  params  {
“From”  :    “<caller_id>”  ,
“To”  :      “<destination_number>”  ,
})
<span class="k">if</span> <span class="n">err</span> <span class="o">!=</span> <span class="no">nil</span> <span class="p">{</span>
<span class="nb">println</span><span class="p">(</span><span class="n">err</span><span class="p">)</span>
<span class="p">}</span>
<span class="n">fmt</span><span class="o">.</span><span class="n">Printf</span><span class="p">(</span><span class="s">"Response: %#v</span><span class="se">\n</span><span class="s">"</span><span class="p">,</span> <span class="n">response</span><span class="p">)</span>
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
using    System  ;
using    System.Collections.Generic  ;
using    Plivo  ;
namespace    test_PHLO
{
class    Program
{
public    static    void    Main  (  string  []    args  )
{
var    phloID    =    “<phlo_id>”  ;
var    phlo    =    phloClient  .  Phlo  .  Get  (  phloID  );
var    data    =    new    Dictionary  <  string  ,    object  >
{
{    “From”  ,    “<caller_id>”    },
{    “To”  ,    “<destination_number>”    }
};
Console  .  WriteLine  (  phlo  .  Run  (  data  ));
}
}
}


```


```text
1
2
3
4
5
curl  --request   POST  \
--user   AUTH_ID:AUTH_TOKEN  \
--url    'https://phlorunner.plivo.com/v1/account/{auth_id}/phlo/{phlo_id}'    \
--header    'Content-Type: application/json'    \
--data    '{"from": "<caller_id>","to": "<destination_number>"}'


```


Replace the auth placeholders with your authentication credentials from the[Plivo console](https://console.plivo.com/dashboard/) . Replace the phlo_id placeholder with your PHLO ID from the[Plivo console](https://console.plivo.com/phlo/list/) . Replace the phone number placeholders with actual phone numbers in[E.164 format](https://en.wikipedia.org/wiki/E.164) (for example, +12025551234).


## Test


Save the file and run it.


Note: If you’re using a Plivo Trial account, you can make calls only to phone numbers that have been verified with Plivo. You can verify (sandbox) a number by going to the console’s Phone Numbers >[Sandbox Numbers](https://console.plivo.com/phone-numbers/sandbox-numbers/) page.


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
