---
schema_version: "1.0.0"
document_id: "b6453a94de78ff46dbe7b55d17ed3326769c8a5a153b79e7cb92b8429692fb25"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/how-to-build-a-voice-survey-app/"
published_at: "2022-02-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:d861a29270e08d4a0baeccbe21bc724b7e60c57367b8e6503ebd3cd0a6a77aec"
---

# How to Build a Voice Survey Application the Low-Code Way Using PHLO

Plivo lets you automate[voice surveys](https://www.plivo.com/docs/voice/use-cases/voice-survey/node/) for use cases such as collecting feedback from customers and conducting polling on political issues. You can set up multiple levels of questions and walk users through different paths depending on the keys they press in response to your questions, and save the responses for analysis.


## How it works


## Prerequisites


To get started, you need a Plivo account -[sign up](https://console.plivo.com/accounts/register/) with your work email address if you don’t have one already. If this is your first time triggering a PHLO with Node.js, follow our instructions to[set up a Node.js development environment](https://www.plivo.com/docs/sdk/server/set-up-node-dev-environment-phlo/) .


## Create the PHLO


To create a PHLO, visit the[PHLO](https://console.plivo.com/phlo/list/) page of the Plivo console. If this is your first PHLO, the PHLO page will be empty.


- Click **Create New PHLO** .
- In the **Choose your use case** pop-up, click **Build my own** . The PHLO canvas will appear with the **Start** node.


**Note:** The Start node is the starting point of any PHLO. It lets you trigger a PHLO to start upon one of three actions: incoming SMS message, incoming call, or API request.


- Click the **Start** node to open the Configuration tab to the right of the canvas, then enter the information to retrieve from the HTTP Request payload. For this example, enter From and To phone numbers and your business name.


- Validate the configuration by clicking **Validate** . Every time you finish configuring a node, click **Validate** to check the syntax and save your changes.


- From the list of components on the left side, drag and drop the **Initiate Call** component onto the canvas. This adds an **Initiate Call** node onto the canvas. When a component is placed on the canvas it becomes a node.
- Draw a line to connect the **Start** node’s **API Request** trigger state to the **Initiate Call** node.


- In the Configuration tab of the **Initiate Call** node, rename the node to **Call_Customer** . You can rename nodes as you like to improve your PHLO’s readability. To enter values for the From and To fields, start typing two curly brackets. PHLO will display a list of all available variables; choose the appropriate ones. When you use variables in a PHLO, the values are retrieved from the HTTP Request payload you defined in the Start node.


- Next, drag and drop the **IVR Menu** component onto the canvas. Draw a line to connect the **Initiate Call** node‘s **Answered** trigger state to the IVR Menu node.


- Click the **IVR Menu** node to open its Configuration tab. Rename the **IVR Menu** node **Question_1** . For this example, select **1** and **2** as allowed choices. In the Speak Text box, enter a message to play to the user that introduces the survey and states the choices they can respond with. If you like, you can also configure the Language and Voice fields for the message.


- Repeat the process with another **IVR Menu** node. Rename it **Question_2** .


‍


- To daisy-chain to the second question after the user gives a valid response to question 1, connect the **Question_1** node‘s **1** and **2** trigger states to the **Question_2** node.


- Configure the choices for **Question_2** on its **Configuration** tab. Again, select **1** and **2** as allowed choices and enter a message to play to the user.


- Drag and drop the **Play Audio** component onto the canvas. Draw a line to connect the **Question_2** node‘s **1** and **2** trigger states to the **Play Audio** node.


- In its Configuration tab, rename the node to **Acknowledge_Participation** . Enter a message of thanks to play to the user in the node‘s Speak Text box.


- Drag and drop the **HTTP Request** component onto the canvas. Draw a line to connect the **Acknowledge_Participation** node‘s **Prompt Completed** trigger state to the HTTP Request node.


- Rename the **HTTP Request** node **Handle_Callback** . Configure the node to post the survey results to a website. On its Configuration tab, enter key names answer1 and answer2. For their values, begin typing two curly brackets to view all available variables, then select **Question_1.digits** and **Question_2.digits** .


- Give the PHLO a name by clicking in the upper left, then click **Save** .


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
import    plivo
phlo_id    =    “<phlo_id>”
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
const    authId    =    “<auth_id>”
const    authToken    =    “<auth_token>”
const    phloId    =    “<phlo_id>”
func    main  ()    {
testPhloRunWithoutParams  ()
}
func    testPhloRunWithoutParams  ()    {
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


Haven’t tried Plivo yet? Getting started is easy and only takes minutes.[Sign up](https://console.plivo.com/accounts/register/) today.
