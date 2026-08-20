---
schema_version: "1.0.0"
document_id: "223ded8bb2c1c82a23640f68c0b0806e4a6242bf0d7befd35eaed8cd41b5c02c"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/beyond-agents.md-shared-memory-for-coding-agents-across-services-and-repos"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T13:25:51.357320+00:00"
fetched_at: "2026-07-29T13:25:51.938497+00:00"
content_hash: "sha256:5483300796bb7e3fdc6e5085807ea48ef921d2a27ed732577c3ae9a2ac03c392"
---

# Beyond AGENTS․md: Shared Memory For Coding Agents Across Services And Repos

Coding agents are everywhere, and if you are a developer, then you probably have at least a few agents ready to fire up the code. But a coding agent is as smart as what it can see, or what it is allowed to see.


Say you have an Agent A who made changes to a part of the codebase that broke code in another part it was never allowed to access. Then Agent B took up the work in the blind part and, unaware of the changes made by Agent A, it made some new changes which broke the code elsewhere. Overall, from a well-working codebase, now you have a broken codebase. But who’s to blame? Because each agent tried to fix changes as per the information it could access.


Now, what if it had a shared memory in between that kept track of all changes made by each agent? So, changes made by Agent A are known already by Agent B before it makes any changes to the codebase.


To test this, I created a small repository with two services that talk to each other. Here, one agent breaks the repo with some changes and then asks a second agent to update the second service. I tested the second agent twice, once with memory (mem0) and once without.


Let’s dive deeper to see how adding shared memory to coding agents helps.


## **The setup**


The codebase includes 2 repositories:


-


` order-service` : This repo contains an event called` OrderStatusChanged` which changes the order status(as the name suggests) whenever an order moves through its lifecycle.


-


` payment-service` : This listens for the previous event and sends a receipt when an order is paid.


**Note** : The event is the only thing connecting them, and neither service imports the other, nor one reads the other's files.


Here is what a usual` payment-service/AGENTS.md` says in this scenario:


```text


```


```text


```


```text


```


Nothing in this AGENTS.md file tells the agent what` order-service` sends, what event looks like, or what changes it made recently. So, your multiple agents are using the same markdown file and end up having the same knowledge. AGENTS.md is not the wrong idea, but once written, it does not update automatically. That is fine for conventions and guardrails that rarely change, but not for facts that change in a service.


Here is a small comparison between the AGENTS.md file and a Mem0 memory:


**AGENTS.md**


**Mem0**


Written by


a person, once


the agent, as it works


Scope


the service it sits in


across services and repos


Updates


when someone remembers to edit it


the moment a change is recorded


How the agent gets it


loaded every session, all of it


searched on demand, filtered to what names this service


Best for


conventions, guardrails


facts that cross boundaries and change


An agent scoped to` payment-service` cannot learn about a change made in` order-service` by reading its own code, because the information is simply not there to read. That is where memory comes as a connector between the two agents. To make the comparison fair, this demo enforces three rules, all of them in` prompts/agentB-consumeevent.md` and` agents/runAgentB.js` :


-


**Rule 1:** The second agent(Agent B) is opened only in` payment-service` and it never sees` order-service` .


-


**Rule 2:** Agent B’s handler file is reset to a neutral version before every run, so no run can read what a previous run wrote.


-


**Rule 3:** The two runs happen in separate fresh sessions, so nothing leaks through a shared conversation.


Without these rules, the agent could stumble onto the answer from a sibling file or from earlier context, and the test would prove nothing. But now, the only path for the change to reach the second agent is via Mem0.


Now, let’s see what role each agent plays in this experiment.


***Fig: The two agents never share code and never share a session. The only thing that travels between them is the memory, tagged with who consumes the change.***


### **Agent A**


The first agent was pointed to` order-service` and given a simple task as follows:


```text
The   reporting   pipeline   needs   order   status    as   a  number   instead   of   a   free  - text   string  ,    so   replace   the   field  .  Some    other   services   consume   this    event  ,    so   record   the   change    in    Mem0   before   finishing  ,    and   do    not   try    to   open   those   other   services
```


```text
```


```text
```


The agent listed all files, read the current event, and rewrote it. The original event looked like this:
` { orderId: "o_123", status: "PAID" }`


After the change, the event looked like this:
` { orderId: "o_123", statusCode: 2 } // 2 = PAID`


So, anything that read` event.status` now reads nothing at all, because the field no longer exists. Then the agent wrote a memory to Mem0 describing what it had done:


### **Agent B (without Mem0)**


Now, the second agent was pointed to` payment-service` with no memory, so it had no idea about the changes made by Agent A. Its handler was reset to the original version, which sends a receipt when` event.status` is` "PAID"` . Its task was to make sure a receipt goes out whenever an order is paid.


It followed the instructions and wrote a slightly better version of the same thing. Its output guards against a missing or non-string status, warns if a paid order arrives with no order id, and otherwise keeps the original behavior:


```text
export    function    onOrderStatusChanged  (  event  )    {
const    status   =  event   &&  typeof    event  . status   ===  "string"
?  event  . status  . toUpperCase  (  )
:  null  ;


if    (  status   ===  "PAID"  )    {
if    (  ! event   || ! event  . orderId  )    {
console  . warn  (  "PAID status received but no orderId present"  ,    event  )  ;
return    null  ;
}
return    sendReceipt  (  event  . orderId  )  ;
}
return    null  ;
}
```


```text
export    function    onOrderStatusChanged  (  event  )    {
const    status   =  event   &&  typeof    event  . status   ===  "string"
?  event  . status  . toUpperCase  (  )
:  null  ;


if    (  status   ===  "PAID"  )    {
if    (  ! event   || ! event  . orderId  )    {
return    null  ;
}
return    sendReceipt  (  event  . orderId  )  ;
}
return    null  ;
}
```


```text
export    function    onOrderStatusChanged  (  event  )    {
const    status   =  event   &&  typeof    event  . status   ===  "string"
?  event  . status  . toUpperCase  (  )
:  null  ;


if    (  status   ===  "PAID"  )    {
if    (  ! event   || ! event  . orderId  )    {
return    null  ;
}
return    sendReceipt  (  event  . orderId  )  ;
}
return    null  ;
}
```


This is good code, but it is also completely wrong for the event that now exists. However, the agent had no way to know that. The real event carries` statusCode: 2` and no` status` field at all. So if the` status` field is null, the paid branch is never entered, and no receipt is sent.


This handler did not crash, but failed quietly. When I ran it, the only output was the line saying the event was received.


The original neutral handler reads` event.status.toUpperCase()` with no guard at all. When I ran that against the new event, it threw an error immediately, because we cannot call a method on a field that does not exist at all.


### **Agent B (with Mem0)**


The third run was the same agent, the same prompt, and the same AGENTS.md file, but with one addition. It could search Mem0, and its prompt told it to check for recorded changes to events before writing any changes to the code.


So it did.


Before touching the handler, it searched Mem0 and got back the memory Agent A had written.


So, now its handler reflects that knowledge:


```text
export    function    onOrderStatusChanged  (  event  )    {
if    (  typeof    event  . statusCode   ===  "number"  )    {
if    (  event  . statusCode   ===  2  )    {
return    sendReceipt  (  event  . orderId  )  ;
}
return    null  ;
}


if    (  event  . status   &&  typeof    event  . status   ===  "string"  )    {
if    (  event  . status  . toUpperCase  (  )   ===  "PAID"  )    {
return    sendReceipt  (  event  . orderId  )  ;
}
}
return    null  ;
}
```


```text
export    function    onOrderStatusChanged  (  event  )    {
if    (  typeof    event  . statusCode   ===  "number"  )    {
if    (  event  . statusCode   ===  2  )    {
return    sendReceipt  (  event  . orderId  )  ;
}
return    null  ;
}


if    (  event  . status   &&  typeof    event  . status   ===  "string"  )    {
if    (  event  . status  . toUpperCase  (  )   ===  "PAID"  )    {
return    sendReceipt  (  event  . orderId  )  ;
}
}
return    null  ;
}
```


```text
export    function    onOrderStatusChanged  (  event  )    {
if    (  typeof    event  . statusCode   ===  "number"  )    {
if    (  event  . statusCode   ===  2  )    {
return    sendReceipt  (  event  . orderId  )  ;
}
return    null  ;
}


if    (  event  . status   &&  typeof    event  . status   ===  "string"  )    {
if    (  event  . status  . toUpperCase  (  )   ===  "PAID"  )    {
return    sendReceipt  (  event  . orderId  )  ;
}
}
return    null  ;
}
```


It reads` statusCode` first and treats` 2` as paid, which is correct for the event that actually exists. It also keeps a fallback to the old` status` string, which is a reasonable thing to do during a migration.


## **Results**


I ran two terminals side by side to check the effect of memory on the agent’s ability to understand the changes done within the codebase.


Without memory, the handler throws a` TypeError` , because` event.status` does not exist to call` toUpperCase` . With Memory, the handler uses loaded memories from Mem0, runs the event and sends the receipt for order` o_123` .


***Fig: Without memory, the agent reads a field that no longer exists and sends nothing. With Mem0, it reads the field that does exist, and the receipt goes out.***


### **Agent B (without Mem0)**


This baseline agent runs using the following command:


```text
$   npm   run   agentB :  baseline
```


```text
$   npm   run   agentB :  baseline
```


```text
$   npm   run   agentB :  baseline
```


and returns:


```text
[turn 0]   read_file(onOrderStatusChanged.js)
[turn 1]   list_files(src)      → handlers/onOrderStatusChanged.js
[turn 2]


```


```text
[turn 0]   read_file(onOrderStatusChanged.js)
[turn 1]   list_files(src)      → handlers/onOrderStatusChanged.js
[turn 2]


```


```text
[turn 0]   read_file(onOrderStatusChanged.js)
[turn 1]   list_files(src)      → handlers/onOrderStatusChanged.js
[turn 2]


```


The agent reads its own handler, sees a handler built around` status` , and writes its version of the same thing. Its own summary says: it preserved the event shape the file already used and made the handler more defensive. It never learned that the shape had changed, because there was nothing in its scope to learn it from.


### **Agent B (with Mem0)**


This memory-backed agent runs using the following command:


```text
$   npm   run   agentB :  memory
```


```text
$   npm   run   agentB :  memory
```


```text
$   npm   run   agentB :  memory
```


and returns:


```text
[turn 0]   list_files(.)        → AGENTS.md, handlers/onOrderStatusChanged.js
[turn 1]   read_file(onOrderStatusChanged.js)
[turn 2]   mem0_search("order event shape payment-service")
→ "…removed 'status', added numeric 'statusCode', 2 = PAID.
Consumers: payment-service, email-service"
[turn 3]


```


```text
[turn 0]   list_files(.)        → AGENTS.md, handlers/onOrderStatusChanged.js
[turn 1]   read_file(onOrderStatusChanged.js)
[turn 2]   mem0_search("order event shape payment-service")
→ "…removed 'status', added numeric 'statusCode', 2 = PAID.
Consumers: payment-service, email-service"
[turn 3]


```


```text
[turn 0]   list_files(.)        → AGENTS.md, handlers/onOrderStatusChanged.js
[turn 1]   read_file(onOrderStatusChanged.js)
[turn 2]   mem0_search("order event shape payment-service")
→ "…removed 'status', added numeric 'statusCode', 2 = PAID.
Consumers: payment-service, email-service"
[turn 3]


```


The memory agent calls` mem0_search` before writing anything. The result names the change directly. The agent summarizes its reasoning by showing how` order-service` replaced the string status with a numeric` statusCode` where 2 means paid, so it wrote the handler to send a receipt on` statusCode === 2` and kept a fallback for the old shape.


## **Conclusion**


AGENTS.md still matters. Conventions, guardrails, and parts of code that rarely change; a file works fine for that. But` event.status` becoming` event.statusCode` isn't a convention. It's a fact that changed on a Tuesday, and no one edited the markdown to say so. The baseline agent didn't fail because it was careless; it failed because nothing in its scope told it that event no longer existed.


The baseline agent produced working, defensive, sensible code that silently drops paid orders. In a real system, this is the kind of bug that shows up weeks later when someone asks why a batch of customers never got receipts. However, with memory, the same agent, given the same prompt, got it right on the first try. It was just given a place to look, and what it found there had been recorded by a different agent, in a different session, working on a service it was never allowed to reach without permission. Both Agent A and Agent B were separate runs with no overlap, and the only thing that traveled from one to the other was a memory/record of what changed and who it affected.


Coding agents are already good at reasoning over what's in front of them. What they're bad at is knowing what they can't see. AGENTS.md answers "what are the rules here”, but a shared memory answers "what changed since the last time anyone looked." Multi-agent, multi-repo work needs both answers, and right now most setups only give agents the first one.


## **Frequently Asked Questions**


### Q. Is AGENTS.md outdated or unnecessary?


No. AGENTS.md is still the right place for conventions and guardrails, things like "don't add a database here." The gap is state that changes during active work: a renamed field, a changed event shape, a decision made an hour ago in a different repo. That's not something a static file updates itself to reflect.


### Q. How was Agent B stopped from just guessing correctly or peeking at Agent A's work?


The test enforced three rules: Agent B was only ever opened inside` payment-service` and never given access to` order-service` , its handler file was reset to a neutral version before every run, and each run happened in a fresh session with no shared conversation history. The only channel between the two agents was Mem0.


### Q. Why didn't the baseline agent's handler throw an error?


Because it was written defensively. It checked whether` event.status` was a string before calling` .toUpperCase()` on it, so a missing field just meant the paid branch was never entered. No crash, no error, no receipt. That's what makes this failure mode worse than a crash: it fails silently and looks like correct, careful code.


### Q. What exactly did the memory-backed agent retrieve from Mem0?


A single recorded memory from Agent A's session, describing that` order-service` had removed the string` status` field and replaced it with a numeric` statusCode` , where` 2` means paid, and naming` payment-service` and` email-service` as consumers of that event.
