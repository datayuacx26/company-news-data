---
schema_version: "1.0.0"
document_id: "672eddeb59c711f9e0f9feef5ae73134ebe812122884a11ecea410151f905cf5"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-rss-bbf1d949eada"
canonical_url: "https://www.tweeks.io/blog/email-automations-from-any-website-with-tw-email"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.378405+00:00"
fetched_at: "2026-07-28T22:14:43.061733+00:00"
content_hash: "sha256:f7ca1b22658b0af2ef35fc77b14ff076fdb95c1a4eaae6c2d11d1df590369a66"
---

# Email Automations From Any Website With TW_email

A tweek can send personal email notifications from website automations with` TW_email` . Add` // @grant TW_email` , then call` await TW_email(subject, body?, delay?)` when the page has a signal worth sending.


The` TW_email` call is only the handoff. A useful automation still needs a clear trigger, dedupe state, and a plan for what happens after Tweeks accepts the request.


For delayed emails, Tweeks does not keep the browser alive or hold an HTTP request open. The server creates a durable workflow run, returns a` runId` , and lets that workflow wake up later to send the email.


## API Surface


### The TW_email contract


` TW_email` sends an email from a tweek to the signed-in user's Tweeks account email address.


Tweeks is a browser extension for creating` tweeks` . A tweek is a script that modifies a website, like[YouTube - Remove Shorts](https://www.tweeks.io/t/bcd8bc32b8034b79a78a8564) or the[Universal AI Slop Finder](https://www.tweeks.io/t/PUjj1S2F) . When a tweek needs to notify you outside the page,` TW_email` gives it a direct email path without making the tweek handle SMTP, delivery credentials, or a separate mail service.


The function signature is:


text


```text
await TW_email(subject, body?, delay?);
```


The call returns a promise that resolves after Tweeks accepts the request. Immediate sends return a message id when delivery succeeds. Delayed sends return a workflow` runId` .


The recommended metadata grant is:


js


```text
// @grant TW_email
```


Tweeks also accepts` // @grant TW.email` and` // @grant email` . At runtime, the same API can be called as` TW_email(...)` ,` TW.email(...)` , or` GM.email(...)` when the tweek has the required email grant.


### Browser-triggered, server-delivered


` TW_email` does not turn the browser into a server-side monitor. The tweek still has to run on a page. Once it does, it can inspect page state, decide whether that state matters, and ask Tweeks to send or schedule an email.


That creates two separate responsibilities:


Responsibility Owner Example


Detecting the condition The tweek running on the page A price is below your target, a job failed, or today's digest has not been sent.


Accepting the request` POST /api/tw/email` Validate auth, subject, body, delay, and account email.


Delivering the email Tweeks after the request is accepted Immediate sends call the email delivery service; delayed sends start` delayedEmailWorkflow` .


For immediate emails,` POST /api/tw/email` sends through the server-side email delivery service and returns a message id. For delayed emails, the endpoint starts` delayedEmailWorkflow` and returns a workflow` runId` . Once a delayed request is accepted, the server workflow owns delivery. The original page can close.


### Request and response constraints


` TW_email(subject, body?, delay?)` sends an email to the authenticated Tweeks account email. It does not accept a` to` field and cannot send to arbitrary recipients.


The API accepts:


Field Required Limit Notes


` subject` Yes


200 characters Must be a non-empty string. The server replaces line breaks with spaces before delivery.


` body` No


50,000 characters Plain text only. Empty body is allowed.


` delay` No


30 days Duration string such as` "10s"` ,` "5m"` ,` "1h"` ,` "24h"` , or` "7d"` .


Because` TW_email` is async, always use` await` or handle the returned promise. If the user is not signed in, Tweeks returns an auth-required response so the extension can show sign-in guidance.


## Runtime And Delivery Architecture


### Extension bridge


The call path is:


text


```text
tweek runtime
-> content bridge
-> service worker
-> ExtensionServerApi.tw.requestEmail()
-> POST /api/tw/email
```


The content runtime first checks that the tweek has permission to use` TW_email` . The service worker checks the permission again, then sends the request through the authenticated Tweeks server client.


### Server endpoint


On the server,` POST /api/tw/email` validates the subject, body, delay, and signed-in user. Immediate emails go straight through the email delivery service. Delayed emails start` delayedEmailWorkflow` , sleep for the requested duration, then use the same delivery path later.


### Delayed email as durable work


#### Why not use browser timers?


Immediate sends can finish inside the API request. Delayed sends need a different shape: Tweeks has to accept work from a page, return quickly, and still send the message after the tab or service worker is gone.


There are a few constraints:


- The tweek may run in a tab that closes seconds later.
- Extension service workers can suspend.
- API requests should finish quickly; they should not wait for a one-hour or seven-day timer.
- A normal JavaScript` setTimeout` is process memory. It disappears if the process restarts.
- The server still needs an audit trail for what was accepted.


For immediate sends, the API can validate the payload and call the delivery service in the same request. For delayed sends, Tweeks treats acceptance and delivery as separate phases. The API creates a durable workflow run with the sanitized email payload, stores the scheduled request in the email log, and returns a` runId` to the tweek.


Tweeks uses the usual durable workflow shape here: persist a run with its arguments, sleep on a durable timer, resume later, and keep side effects behind explicit steps.


The delayed email workflow is intentionally small:


ts


```text
export    async    function    delayedEmailWorkflow  ( params  :  DelayedEmailParams   ) {
"use workflow"  ;


const   { toEmail, subject, body, delayMs } = params;


await    sleep  (delayMs);


const   deliveryConfig =  getEmailDeliveryConfig  ();
if   (!deliveryConfig. available  ) {
throw    new    Error  ( "Email delivery is not configured"  );
}


const   result =  await    sendEmailStep  (toEmail, subject, body, deliveryConfig);


return   result;
}
```


The important line is` sleep(delayMs)` . That is a durable timer owned by the workflow runtime, not a browser timer and not an open request. The workflow can be resumed later with the original payload. The email delivery call is isolated in a step, which gives the system a clear boundary for execution, logging, and failure handling.


#### Queue payload


The queue payload is deliberately boring. It contains only the data needed to send the email later:


ts


```text
{
userId,
toEmail,
subject  : sanitizedSubject,
body  : sanitizedBody,
delayMs,
}
```


The server resolves the recipient from the signed-in user's Tweeks account. The tweek does not provide` toEmail` , and the API does not accept arbitrary recipients. Delayed sends follow the same rule as immediate sends.


The API response for a delayed send looks like an acceptance receipt:


json


```text
{
"success"  :    true   ,
"scheduled"  :    true   ,
"runId"  :    "..."
}
```


That` runId` is not a delivery confirmation. It means Tweeks accepted the delayed job and handed it to the workflow runtime. Delivery happens when the workflow wakes up and the send step calls the email delivery service.


#### Failure boundaries


Delayed email has different failure points than immediate email.


Before queueing, the API can reject the request because the user is not signed in, the subject is missing, the body is too large, the delay is invalid, or the email service is not configured. In those cases, the tweek gets an error immediately and should not update dedupe state.


After queueing, the browser page is no longer in the loop. Delivery can still fail later if the email service rejects the message or the workflow cannot access delivery configuration. Keeping the workflow small makes the failure point obvious: one step logs delivery failure, and the same step is where retry policy belongs if the failure mode is retryable.


## Implementation Patterns


### Trigger design


Good triggers are specific, stable, and deduplicated. Bad triggers fire on incidental page activity.


Good trigger Why it works


A price crosses below a threshold The condition is specific and easy to store.


A workflow reaches` complete` ,` failed` , or` error` The page has reached a terminal state.


A daily digest has not been sent today The tweek can store the last sent date.


A new item appears that was not seen before The tweek can store a seen item id.


Avoid sending from an unbounded` MutationObserver` , inside a loop over every row, or on every page load without stored state. If the page changes often, debounce the check and store what was already sent.


### Idempotency before sending


Most email automations need idempotency. The safest pattern is:


1. Read stored state before sending.
2. Return early if the same event was already sent.
3. Call` await TW_email(...)` .
4. Write the new stored state only after the email request succeeds.


That last step matters. If the email call fails because the user is signed out, offline, or hitting a temporary service error, the tweek should not record the event as sent.


## Complete Examples


### Minimal email from a tweek


This minimal tweek sends an email to your Tweeks account email when you open a matching page.


js


```text
// ==UserScript==
// @name         Email me when this page opens
// @namespace    https://tweeks.io/examples
// @version      1.0.0
// @description  Send a personal email from a tweek.
// @match        https://example.com/account/*
// @grant        TW_email
// ==/UserScript==


( async   () => {
try   {
const   response =  await    TW_email  (
"Account page opened"  ,
`The page was opened at  ${ new    Date  ().toISOString()}  \n\n ${location.href}  `  ,
);


console  . log  ( "Email sent"  , response. messageId  );
}  catch   (error) {
console  . warn  ( "Could not send email"  , error);
}
})();
```


Use this pattern when opening the page is the event you care about.


### Price drop alert


For price alerts, store the last alerted price and a cooldown timestamp. The price condition may be true for hours or days, so the tweek should not email you every time the product page loads.


js


```text
// ==UserScript==
// @name         Product price drop email alert
// @namespace    https://tweeks.io/examples
// @version      1.0.0
// @description  Email me once when a product price drops below my target.
// @match        https://shop.example.com/products/*
// @grant        TW_email
// @grant        GM_getValue
// @grant        GM_setValue
// ==/UserScript==


( async   () => {
const   targetPrice =  50  ;
const   cooldownMs =  6   *  60   *  60   *  1000  ;
const   stateKey =  `price-alert: ${location.pathname}  `  ;
const   priceNode =  document  . querySelector  ( "[data-price]"  );


if   (!priceNode) {
return  ;
}


const   priceText = priceNode. textContent   ||  ""  ;
const   price =  Number  (priceText. replace  ( /[^0-9.]/g  ,  ""  ));


if   (! Number  . isFinite  (price)) {
return  ;
}


const   state =  await    GM  . getValue  (stateKey, {
lastAlertedPrice  :  null  ,
lastSentAt  :  0  ,
});


if   (price >= targetPrice) {
return  ;
}


if   (state. lastAlertedPrice   === price) {
return  ;
}


if   ( Date  . now  () - state. lastSentAt   < cooldownMs) {
return  ;
}


try   {
await    TW_email  (
`Price drop: $ ${price.toFixed( 2  )}  `  ,
[
`The product is now $ ${price.toFixed( 2  )}  .`  ,
`Target price: $ ${targetPrice.toFixed( 2  )}  `  ,
`Page:  ${location.href}  `  ,
`Checked at:  ${ new    Date  ().toISOString()}  `  ,
]. join  ( "\n"  ),
);


await    GM  . setValue  (stateKey, {
lastAlertedPrice  : price,
lastSentAt  :  Date  . now  (),
});
}  catch   (error) {
if   (error. authRequired  ) {
console  . warn  ( "Sign in to Tweeks before sending price alerts."  );
return  ;
}


console  . warn  ( "Price alert email failed"  , error);
}
})();
```


This is browser email automation, not crawling. It checks the product when the product page runs.


For a shopping tweek that already reads price data from product pages, see[Amazon Show Price History with CamelCamelCamel](https://www.tweeks.io/t/4f8052ffbec6428eb0b28be6) . Add a` TW_email` grant to the same page-reading approach and you have a price alert.


### Scheduled reminder


Pass a delay string as the third argument. Tweeks accepts durations like` "10s"` ,` "5m"` ,` "1h"` ,` "24h"` , and` "7d"` , up to 30 days.


js


```text
// ==UserScript==
// @name         Follow-up reminder email
// @namespace    https://tweeks.io/examples
// @version      1.0.0
// @description  Schedule a reminder after visiting a review page.
// @match        https://app.example.com/reviews/*
// @grant        TW_email
// ==/UserScript==


( async   () => {
const   needsFollowUp =  document  . body  . textContent  ?. includes  ( "Needs follow-up"  );


if   (!needsFollowUp) {
return  ;
}


try   {
const   result =  await    TW_email  (
"Follow up on review"  ,
`Re-check this review page:  ${location.href}  `  ,
"1h"  ,
);


console  . log  ( "Reminder scheduled"  , result. runId  );
}  catch   (error) {
console  . warn  ( "Could not schedule reminder"  , error);
}
})();
```


Delayed delivery returns` scheduled: true` and a workflow` runId` once Tweeks accepts the request.


### Daily page digest


Use` TW_email` when a page has enough information to summarize into a short report. This example sends at most one digest per local day, and only when the tweek runs on the page. It writes` last-dashboard-digest-date` only after the email request succeeds.


js


```text
// ==UserScript==
// @name         Daily dashboard digest email
// @namespace    https://tweeks.io/examples
// @version      1.0.0
// @description  Email a daily digest from a dashboard page when the page runs.
// @match        https://dashboard.example.com/*
// @grant        TW_email
// @grant        GM_getValue
// @grant        GM_setValue
// ==/UserScript==


( async   () => {
const   today =  new    Date  (). toISOString  (). slice  ( 0  ,  10  );
const   lastDigestDate =  await    GM  . getValue  ( "last-dashboard-digest-date"  ,  ""  );


if   (lastDigestDate === today) {
return  ;
}


const   rows = [... document  . querySelectorAll  ( "[data-digest-row]"  )];
const   lines = rows. slice  ( 0  ,  20  ). map  ( ( row, index  ) =>   {
const   label = row. querySelector  ( "[data-label]"  )?. textContent  ?. trim  () ||  `Item  ${index +  1  }  `  ;
const   value = row. querySelector  ( "[data-value]"  )?. textContent  ?. trim  () ||  "No value"  ;
return    `-  ${label}  :  ${value}  `  ;
});


if   (lines. length   ===  0  ) {
return  ;
}


try   {
await    TW_email  (
`Daily dashboard digest for  ${today}  `  ,
[ `Digest from  ${location.href}  `  ,  ""  , ...lines]. join  ( "\n"  ),
);


await    GM  . setValue  ( "last-dashboard-digest-date"  , today);
}  catch   (error) {
console  . warn  ( "Digest email failed"  , error);
}
})();
```


This pattern fits pages you already visit, internal dashboards, admin queues, and reports that only need one personal email per day.


### Workflow completion or failure


Use` TW_email` after the page has enough state to know the workflow is finished. The example below stores the terminal status it reported, so reloading the job page does not send another email for the same result.


js


```text
// ==UserScript==
// @name         Workflow completion email
// @namespace    https://tweeks.io/examples
// @version      1.0.0
// @description  Email me when a workflow completes or fails.
// @match        https://app.example.com/jobs/*
// @grant        TW_email
// @grant        GM_getValue
// @grant        GM_setValue
// ==/UserScript==


( async   () => {
const   jobId = location. pathname  . split  ( "/"  ). filter  ( Boolean  ). pop  () || location. href  ;
const   sentKey =  `workflow-email-sent: ${jobId}  `  ;


const   statusNode =  document  . querySelector  ( "[data-job-status]"  );
if   (!statusNode) {
return  ;
}


const   status = statusNode. textContent  ?. trim  (). toLowerCase  ();
const   terminalStatuses =  new    Set  ([ "complete"  ,  "completed"  ,  "failed"  ,  "error"  ]);


if   (!terminalStatuses. has  (status ||  ""  )) {
return  ;
}


const   alreadySentStatus =  await    GM  . getValue  (sentKey,  ""  );


if   (alreadySentStatus === status) {
return  ;
}


const   subject = status ===  "failed"   || status ===  "error"
?  "Workflow failed"
:  "Workflow complete"  ;


try   {
await    TW_email  (
subject,
[
`Status:  ${status}  `  ,
`Job:  ${jobId}  `  ,
`Page:  ${location.href}  `  ,
`Checked at:  ${ new    Date  ().toISOString()}  `  ,
]. join  ( "\n"  ),
);


await    GM  . setValue  (sentKey, status);
}  catch   (error) {
if   (error. authRequired  ) {
console  . warn  ( "Sign in to Tweeks before sending workflow emails."  );
return  ;
}


console  . warn  ( "Workflow notification email failed"  , error);
}
})();
```


The call sits between two checks: after the page reaches a terminal state, before the tweek records that the notification was sent.


## Operational Notes


### Failure handling in tweeks


Handle` TW_email` as a networked async call. The user may be signed out, the request may time out, or the email service may reject the request.


This pattern is enough for most tweeks:


js


```text
async    function    notifyFailure  (  ) {
try   {
await    TW_email  ( "Workflow failed"  ,  "The workflow entered failed status."  );
}  catch   (error) {
if   (error. authRequired  ) {
console  . warn  ( "Sign in to Tweeks before sending email."  );
return  ;
}


console  . warn  ( "Email notification failed"  , error);
}
}
```


Do not update dedupe state inside the` catch` branch. If the request was not accepted, the next page run should still have a chance to send.


### Subject and body design


The subject should identify the event in under 200 characters. Keep it short enough that it still works after the server strips line breaks.


The body is plain text, so it can carry the details you will need later. For debugging your own automations, include:


- the URL that triggered the email
- the observed value or status
- the threshold or expected value
- an ISO timestamp
- the next action you expect yourself to take


The body can be empty, but an empty body rarely helps. The practical limit is 50,000 characters, so trim scraped page text before sending it.


## Limits And FAQ


### Safety model


` TW_email` is scoped to personal notifications.


The API requires Tweeks authentication and sends only to the signed-in user's account email. A tweek cannot pass a recipient list to` TW_email` , so the API stays focused on self-notifications instead of general email delivery.


A tweek must explicitly request access with` // @grant TW_email` . If a tweek has` // @grant none` , remove it before adding the email grant because` none` disables granted APIs.


The server validates inputs again after the extension runtime accepts the call:


- ` subject` must be a non-empty string with at most 200 characters.
- ` body` must be a string when provided, with at most 50,000 characters.
- ` delay` must be a positive duration string no longer than 30 days.
- calls require a signed-in user with a Tweeks account email.


Use` TW_email` for personal alerts, status reports, reminders, page digests, and lightweight workflow automation. Do not use it for bulk sending, marketing email, newsletters, cold outreach, or any automation that needs arbitrary recipients.


### Can TW_email send to any address?


No.` TW_email` sends only to the signed-in user's Tweeks account email. It does not accept arbitrary recipients.


### Does TW_email work without signing in?


No.` TW_email` requires Tweeks authentication. If the user is not signed in, Tweeks returns an auth-required response and the extension can show sign-in guidance.


### Can I schedule emails?


Yes. Pass a delay string as the third argument, for example` await TW_email("Reminder", "Check this page", "1h")` .


### What is the maximum delay?


The maximum delay is 30 days.


### What happens if my browser is closed?


The tweek must run first to send or schedule the email. If a delayed email was already accepted by Tweeks, delivery is handled by the server workflow even if the browser is closed later.


### Is this a replacement for a marketing email platform?


No.` TW_email` is for personal email alerts and reminders from tweeks. It is not a bulk email, newsletter, cold outreach, or marketing automation system.


## Starting Points


Browse[universal tweeks](https://www.tweeks.io/sites/all-sites) for scripts that run on every website, or the per-site pages like[Gmail](https://www.tweeks.io/sites/gmail.com) and[LinkedIn](https://www.tweeks.io/sites/linkedin.com) . Any tweek you install can be edited to add a` TW_email` grant and an alert.
