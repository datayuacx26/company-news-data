---
schema_version: "1.0.0"
document_id: "9bf96ee1fd6885e9bb2ebc02cf1d7064629b284daf29b7819c7d541ff7529211"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/automated-invariant-monitoring"
published_at: "2024-09-09T00:00:00+00:00"
first_seen_at: "2026-07-24T11:51:21.619731+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:43802c267dc25255fe448b0ef42ebd7caa599ffb62ca4b1e58c766d465594058"
---

# Automated invariant monitoring — Increase

In 2012, Stripe had an incident where a file sent to a financial partner wasn’t acknowledged. This went unnoticed, leading to a delay in transferring what seemed like a significant amount of money at the time. During the debrief, it became clear that we needed more robust automated monitoring and alerting for our financial partner integrations. In the next day or so,[Patrick](https://twitter.com/patrickc) and[Siddarth](http://siddarthc.com/) built an invariant monitor.


The invariant monitor was a lightweight framework that enabled engineers to write code that continuously tracked certain database conditions to ensure they always held true. If something went wrong, it would raise an alert. It was loosely inspired by[Nagios](https://www.nagios.org/) but written as application code that runs in the context of the rest of the codebase. Examples of the conditions that were monitored were:


- Has a money movement file been generated every day?
- Has every generated file been submitted to a partner?
- Has every submitted file been acknowledged by that partner within the expected timeframe?


The invariant monitor became even more useful when[Jeremy](https://twitter.com/jeremydhoon) built automated monitoring into the frameworks for file generation, submission, retrieval, and storage.


This tool massively improved operations. It let us codify and monitor our assumptions. It was like seeing automated testing for the first time—it felt so clearly the right approach and so necessary that it was surprising every company didn’t have a similar tool.


At Increase, this kind of monitoring was one of the first things we built. We call it Checker, with the individual monitoring rules known as Checks. ([We’re not particularly creative](https://increase.com/articles/no-abstractions) with names.)


**Here’s what a Check looks like:**


```text
check   '  Database backup test restorations are running  '  ,
frequency  :   Frequency  ::  EVERY_SIX_HOURS   do
DatabaseBackupRestoration  .  exists?  (
created_at  :   (  Time  .  now   -   2  .  days  )..,
status  :   DatabaseBackupRestoration  ::  Status  ::  DONE  ,
)
end
```


This alerts if we’ve failed to restore our database backups within the last two days. It’s reassuring to know that we’d know if there’s something wrong with our database backups before we need them.


Like Stripe, we’ve built automated Checks into our frameworks. One particularly useful pattern has been implementing automated Checks for our state machines. These Checks monitor that transitory states don’t last too long. For example, any mail item received for[our new lockbox feature](https://increase.com/articles/announcing-check-lockboxes) shouldn’t have a` status` of` PENDING_PROCESSING` for more than a day. If it does, we’re alerted.


**Here’s the code:**


```text
class   InboundMailItem   <   AbstractModel
attribute   :  status  ,
default  :   Status  ::  PENDING_PROCESSING  ,
transient_values  :   [  Status  ::  PENDING_PROCESSING  ],
terminal_values  :   [  Status  ::  PROCESSED  ,   Status  ::  REJECTED  ]
end
```


Engineers mostly don’t have to think about Checks when building features.` attribute` is the default way to declare a model property in our Object-Relational Mapper so engineers typically get this monitoring for free.


We use Checks broadly. Since Checks are code, they can make API calls and monitor external services too. Security, for example, has a Check to ensure no Dropbox files are accidentally shared. And Compliance has one to track sanction screening lists. Whenever a person manually monitors something, we try to turn it into a Check. (That way they can go on vacation.)


We’ve also built tooling and processes around our Checks. If you’re considering building your own, here are a few things we’ve learned:


- As with any monitoring, be careful with your signal. A flappy Check is worse than no Check.
- Checks can have different priorities and alerting. A Check for an unexpectedly missing ACH file might page an on-call engineer while a Check that our physical card inventory is getting low might just create a ticket.
- Each Check should have a responsible team or person.
- Have a suppression mechanism to help with long-failing Checks.
- Similar to tests, ensure Checks are easy to write. Also similar to tests, periodic pruning helps.
- Have tests for your Checks. These need to include both the success and failure cases.


If ensuring people can sleep well at night is the kind of engineering you enjoy, you might also enjoyjobs@increase.com .
