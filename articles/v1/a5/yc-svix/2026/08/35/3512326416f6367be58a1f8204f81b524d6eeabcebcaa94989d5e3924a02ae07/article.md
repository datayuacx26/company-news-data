---
schema_version: "1.0.0"
document_id: "3512326416f6367be58a1f8204f81b524d6eeabcebcaa94989d5e3924a02ae07"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/webhooks-suck/"
published_at: "2026-08-18T12:00:00+00:00"
first_seen_at: "2026-08-18T20:31:08.666551+00:00"
fetched_at: "2026-08-18T20:31:11.153377+00:00"
content_hash: "sha256:b20cc4a0d7b1bc56232c586205aaccca65e66ca8a2c56ef051b3302eead74c50"
---

# Webhooks Suck: Fixing the Problems with Consuming Webhooks

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


Webhooks are simple and ubiquitous, and that's both a weakness and a strength. It means that they are everyone's go-to, even for use-cases that they don't solve very well.


They have also traditionally been very difficult to consume due to ad-hoc and complex signature schemes, bad observability, and difficult setup.


This post is about many of the challenges with consuming webhooks, and what we are doing to solve them. I've been meaning to write this post for a while, and[a recent post on HN](https://news.ycombinator.com/item?id=49184216) was the push I needed to finally finish it.


Here are some of the common challenges. Some of them we've already solved, but some are still work in progress.


## Webhook signature verification


One of the most annoying parts of consuming webhooks has traditionally been signature verification. Webhooks are unauthenticated HTTP calls and can therefore be faked and forged by attackers. That's why webhooks are most commonly signed in order to ensure authenticity and their timestamps verified in order to protect against replay attacks.


This used to be a hot mess (still is to an extent), where every provider would reinvent their own signature scheme. Some were good, most were bad, and many were very bad. Though even for the good ones, that often meant consumers had to understand, implement, and test a new signature scheme for each provider.


To solve this, we created[Standard Webhooks](https://www.standardwebhooks.com/) , which includes a consistent signature scheme that can be used by everyone. Standard Webhooks includes a set of SDKs for signature verifications which senders and receivers can both use, as well as built-in support in Svix, ngrok, Kong, and many others making it mostly an issue of the past.


## Webhook observability and diagnosing failures


When an API call fails the caller knows it fails. Their call fails, and they get a timeout error, an HTTP 500, or some other type of error. With webhooks that's not the case, as you often don't know you were supposed to receive an event when you receive one. Even if you monitor your infrastructure well, the failure may happen in an area you don't control, e.g. a networking issue or a DNS issue.


To solve this Svix provides full visibility into every webhook attempt, including status, failure reason, request content, response content, and other metadata. This gives people the ability to fully diagnose issues when they happen.


Svix also supports automatically sending emails and Slack messages to consumers when failures happen consistently. So they get an out-of-band message in addition to the failed webhooks themselves.


## Managing webhook subscriptions without click ops


One of the things I find most annoying about webhooks is click ops. If you want to receive webhooks from a provider an admin almost always has to go to their website, login, navigate to the UI, and update the webhook subscription.


This happens completely disjointly from code and infrastructure deployment, and is especially annoying for teams that employ infrastructure-as-code.


Svix has always supported controlling webhooks subscriptions via an API. Though while the API works, it always felt a bit cumbersome, as it required writing custom code.


That's why we introduced[Webhooks AutoConfig](https://www.svix.com/blog/webhooks-autoconfig/) earlier this year. It makes updating webhooks subscriptions trivial, which makes consuming webhooks much easier.


## Local development with webhooks


Local development is a common pain with webhooks. The problem is that webhooks are often sent from 3rd party SaaS services which can't access your local development machine.


To solve it, most people use some sort of an HTTP forwarding service such as ngrok or[Svix Play](https://www.svix.com/play/) to forward requests to a local endpoint. This works, but it suffers from a variety of issues.


The main problem is that setup is a pain. You need to run this additional CLI app, point it at the local server, and update the sender with the right credentials. This works until you turn your computer off and come back the day after to many failing events that are constantly retrying or a disabled endpoint you need to turn back on.


It's also a problem when you have multiple people sharing the same development SaaS service, making changes in parallel, and due to different times of having their laptops on, have completely different states. They also need to contact that service's admin every time they need to modify each developer's webhook subscription. This should probably also be addressed by more SaaS offering separate testing accounts, but that's a problem for another day.


Our solution to this is a combination of[Webhooks AutoConfig](https://docs.svix.com/receiving/webhooks-autoconfig) and[Polling Endpoints](https://docs.svix.com/advanced-endpoints/polling-endpoints) . Webhooks AutoConfig lets people automatically update their subscriptions from their code (never having to access the UI). Polling Endpoints make it so there are never failed events, as the events are sent directly to the local server when it's running, and do nothing when not. It also lets people test in parallel, as now every integration gets every webhook.


## Testing webhooks


Webhooks are asynchronous, and asynchronous flows are generally harder to test because testing tools are not wired to react to outside events. On top of that, you would need to expose the test suite to the source of events and generate them, which is not easy.


We haven't started working on this yet, but we have some ideas. The best way is to currently mock webhooks and potentially use[the webhook simulation helper](https://www.standardwebhooks.com/simulate) we built.


## Replaying failed webhooks


Bugs happen. Therefore bugs in webhooks receivers happen. As a consumer of webhooks, it's very frustrating not being able to replay failed messages once you fix bugs in the receiver. If you can't play back failed messages you often need to write a custom script that reconciles state somehow, which is additional work and is fragile.


That's why being able to replay failed events is table stakes for any webhook system. At Svix we've also added the ability to replay "missing events" which are events that have never been tried, because for example the endpoint was created after the event was sent or was not subscribed to a specific event type when a message with that type was sent. We also support replaying successful messages, because we noticed that many integrations would return a` 200 OK` without correctly processing the message. This gives them the ability to retry events in those scenarios as well.


## Webhook retry storms and overloaded endpoints


While your service may be able to generate 10,000+ events per second and Svix will happily process these for you, many of your customers will likely fail when faced with this load. To make matters worse, this load often leads to slower processing, which leads to more load, more failures, and more retries. This causes significant sustained load on webhook consumers which often brings their services down.


This is not theoretical, we see it time and time again. That's why we introduced[Endpoint Throttling](https://docs.svix.com/throttling) a few years ago. It lets your customers control the rate at which they receive events to make sure they are never overloaded.


Consumers can also use Polling Endpoints to better control the rate at which they receive events. Though those come with their own challenges. More on that below.


## Webhook idempotency and duplicate delivery


Webhooks are generally "at least once", which means duplicate delivery can happen. Depending on the consumer this can lead to conflicts, failures, or other inconsistencies.


We follow[Standard Webhooks](https://www.svix.com/blog/standard-webhooks/) and include a unique message ID per message that is stable across retries. Webhook consumers can then use this ID to ensure they only process a message once.


## Webhook processing timeouts


It's best practice to process webhooks quickly. Though in practice it's much easier to have webhook receivers do a lot of heavy lifting, including database access, network calls, and more. Keeping many webhook connections active can cause load on the sender, so many senders configure very low connection timeouts.


The problem with this is that many consumer webhooks register as failed and are then retried, causing annoyance for the consumers. At Svix we recommend people consume webhooks quickly, but we offer a generous 15s timeout by default. We also support[Advanced Endpoint Types](https://docs.svix.com/advanced-endpoints) that let your customers consume events directly into their S3, event broker, or many other systems; and making it easier for them to process events asynchronously.


## Out-of-order webhook delivery


Webhooks are naturally unordered. We[previously wrote about why](https://www.svix.com/blog/guaranteeing-webhook-ordering/) that's the case. This means that your receivers need to be able to handle that, otherwise you may try to create a child object before the parent was created, or you may overwrite a database row with old information.


We generally recommend people design their APIs without ordering requirements, and there are ways to mitigate some of the issues. Though what we do at Svix is let your customers choose ordered delivery when absolutely required by either using[FIFO Endpoints](https://docs.svix.com/advanced-endpoints/fifo-endpoints) or Polling Endpoints.


## Detecting missing webhooks


Missing events are much harder to notice than failing ones. As mentioned Svix will notify you when events start failing or your endpoint is disabled. However, it won't notify you if the provider stopped sending webhooks for your account.


This is still an open problem we haven't addressed. We could add minimum webhooks notifications or other anomaly detection. Though this remains TBD.


## Consuming webhooks from multiple providers


Consuming from multiple providers tends to be a real pain. Each has their own signature schemes, payload conventions, retry schedule, etc. It's also N different dashboards to update when you want to change something.


Standard Webhooks has been great in that regard, making the N different services at least somewhat consistent. We've also introduced[Svix Ingest](https://www.svix.com/ingest/) to consolidate webhooks from multiple providers and make it easier to control them in one place, though we still have more planned.


## Webhooks vs. polling APIs (` /events` )


A common solution suggested online, including in the HN post I mentioned above, is to drop webhooks in favor of` /events` . An API for polling events in an ordered manner.


While it's true that ordered polling solves a lot of problem (that's why we added Polling Endpoints), it also introduces many problems of its own.


It changes the model so that you have to have a long running task that keeps on polling the sender causing load on the sender, increased costs for the receiver, and generally increased latency compared to webhooks. This tradeoff is OK when you receive webhooks almost constantly, but is much worse when webhook delivery is sparse. You can fix this by sending ping webhooks when there's data available so that you get lightweight nudges to poll when there's new data.


Additionally, polling endpoints as often suggested are a single stream, meaning that to maintain ordering you can only fetch data from one worker which limits throughput. You also need to have your own error management because if you fetch 100 messages and message #32 fails, you need to be able to save that locally for additional processing. You can solve some of these problems by replicating the` /events` data to a local event broker and handling failures, though it's still a challenge that leads to the same problems webhooks suffer from.


## Summary


These are some of the common challenges webhook consumers face when consuming webhooks, and how we approach them at Svix. This list doesn't include the challenges that Svix solves for senders, which are a whole different matter; for that side of the story, read[Webhooks Are Harder Than They Seem](https://www.svix.com/blog/webhooks-are-harder-than-they-seem/) .


Please let me know if you've experienced other challenges that I haven't addressed above, and I would be happy to add them.


---


For more content like this, make sure to follow us on[X / Twitter](https://x.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
