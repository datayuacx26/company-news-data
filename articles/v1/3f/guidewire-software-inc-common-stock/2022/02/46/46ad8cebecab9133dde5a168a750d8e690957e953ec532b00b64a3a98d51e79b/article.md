---
schema_version: "1.0.0"
document_id: "46ad8cebecab9133dde5a168a750d8e690957e953ec532b00b64a3a98d51e79b"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/simplifying-outbound-integrations-in-insurancesuite-cloud-part-1"
published_at: "2022-02-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:94688690607de47b338622cf6d6b198d96cdced2e056b311a1f65509ec69b499"
---

# Simplifying Outbound Integrations in InsuranceSuite Cloud - Part 1

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- Simplifying Outbound Integrations in InsuranceSuite Cloud - Part 1


This is a two-part series about how Guidewire simplifies outbound integrations with Application Events and Integration Gateway in InsuranceSuite Cloud.


In Part One, we’ll cover integrations: what makes them complex in Property and Casualty (P&C), and what we’re doing to make them simpler in Guidewire Cloud Platform (GWCP). Then in part two we’ll dive deeper into outbound integrations, share a video demo of Application Events using webhooks, and talk about using Guidewire’s Integration Gateway to deliver events to additional systems.


## Watch the Video Presentation


If you’d like to see both parts in full presented by Guidewire’s Director of Product Management, Simon Reading, and Senior Software Architect, Mark Bolger, which includes the demo of Application Events and Integration Gateway, watch the video below:


## What is Downstream Integration?


Downstream integration is a super-common integration pattern in which you have an external system that needs to be notified when something has happened in InsuranceSuite. And it needs to do this to either keep the external system up to date or to trigger automated actions from a workflow and transactions that have occurred inside InsuranceSuite. You can imagine use-cases for this with things like fraud scoring, medical bill review, estimatics, or outbound payments — all of these incur some kind of outbound integration.


In the past, this kind of event-driven integration was implemented using event messaging, which works very well. However, it does require custom[Gosu coding](https://www.guidewire.com/developers/developer-tools-and-guides/gosu-programming-language) . And today we’re going to introduce Application Events, which allows you to have a no-code and low-code approach to managing this kind of event-driven integration.


## Integration Complexity in P&C


Integration, regardless of industry, is complicated and that’s true with P&C. What drives that complexity? With an InsuranceSuite implementation, there are many integrations that are involved with the core system. As you can imagine there could easily be a hundred integrations that are involved. Each of these systems are different, have different workflows, and are not necessarily designed to work together. They might use different protocols, different file formats, and so on.


There’s a large degree of complexity and variability as well, so the question is “How do we simplify it?”


## Guidewire’s Cloud Integration Frameworks Simplify Integrations


Our approach to simplifying integrations in Guidewire Cloud involves three different frameworks:


- InsuranceSuite Cloud API
- Application Events
- Integration Gateway


The first framework,[InsuranceSuite Cloud API](https://www.guidewire.com/developers/apis/cloud-apis) , is a fine-grained, productized API that allows you to perform business actions in InsuranceSuite. This is used for inbound integration, where the flow is triggered from the external system.


Application Events (App Events) is a new feature that allows us to publish events to a downstream system in near realtime.


Finally, we’re going to talk about Integration Gateway. Integration Gateway allows you to build integration applications, which bridge between InsuranceSuite and the downstream system.


## Outbound Integrations Notify Downstream Systems When InsuranceSuite Events Happen


Imagine a scenario where you have work that is being performed in InsuranceSuite—this could be users doing work like adjusters, underwriters, or it could be triggered by an API or some kind of process—and business transactions are getting committed in InsuranceSuite.


Your downstream systems need to know what is going on, because they often need to keep their data in sync with what is happening in the system of record. Or you might want to trigger an action in the downstream system when work occurs in InsuranceSuite.


Typically, the way that this is addressed is by using an event-driven design. An event encapsulates that something occurred in the core system. An event has a name—for example, “ExposureOpened”—and this is associated with an entity in the InsuranceSuite data model.


## How Does Event Messaging Traditionally Work?


In our scenario, work is getting done and transactions are being committed. InsuranceSuite uses a mechanism called event messaging to raise data-oriented events. You could think of these as CRUD-like events (create, read, update, delete) such as adding reserves, adding documents, etc. Or, they could be more policy lifecycle events or claim lifecycle events. For example, when a policy is bound, renewed, or canceled—these are all examples of events. There is the event itself, and then there is the related data: the context for what was going on at the time of the event.


This is traditionally how event messaging has worked in InsuranceSuite:


If you read this left to right, you can see what the data flow is:


1. **Transactions happen in InsuranceSuite**
2. **Events fire**
3. **Customer-written event-fired rules run**
These rules look at the event and, with encapsulated logic, they determine “Do I need to send a message to a downstream system as a result of this event?”
4. **Event firewalls create a message for a downstream system destination**
This is a payload that has all the related data which explains what the event is and gives additional context. That could be additional information about the claim, the policy, the account, and so on.
5. **Event messages are sent through transport plugins**
These custom-written plugins pick up the event and they transport it to the downstream system. Transporting could be done over JMS, by making a SOAP call, by creating a file—there are a lot of different options.
6. **The event message is delivered to the downstream system**
It could be the medical review system, fraud scoring, vehicle estimatics, or other external system that receives this event message
7. **The downstream system responds according to the event message**
This may include updating itself, triggering an action, etc.


This traditional flow requires customers to write custom Gosu integration code in the core. What we’ve seen is that when your integration code is part of InsuranceSuite, and you want to change your integration, it can interfere and impact other things that are going on. That’s because your business logic and integration logic are mixed together in one place.


## The Benefits of Guidewire’s Application Events


With App Events, we’re moving away from something that requires custom Gosu coding and moving towards no-code, low-code integrations externalized outside of InsuranceSuite. Plus, App Events can be delivered in near realtime to your downstream systems.


By using techniques like App Events and Integration Gateway, it allows us to externalize this integration code and move it outside of InsuranceSuite so that you’re able to update it semi-independently from your business logic.


This is a really huge win because it allows your business and integration teams to work a little bit more independently. It creates greater agility when your business team can focus on building functionality independently while the integration team focuses on planning.


## Built for Increased Performance and Scalability


Another goal we had was increased performance and scalability. We built this on top of cloud-native technologies, and we’re using[Apache Camel](https://camel.apache.org/manual/faq/what-is-camel.html) because they give us a lot of options for scaling.


App Events publishes InsuranceSuite events and related claim or policy information to the downstream system in near realtime. The event could be something like “ClaimOpened,” submission related events, etc.


We have two delivery mechanisms for this for different use cases: webhooks and Integration Gateway


### 1. Delivering App Events with Webhooks (No Code)


With the webhook approach, this is something that you would typically use if you’re integrating with a flexible system that is able to expose a custom HTTP endpoint. Then, when an event is fired, it is delivered over HTTP to the downstream system. Effectively, it posts the event to the custom endpoint that you’ve exposed in that system. And it will not only have the event name and metadata about the event. It will also have the claim or policy information as of the time that it occurred. Being able to generate early bound payloads in an efficient manner is really important. This was something that we really focused on when we were building this feature.


So that was a webhook approach. But often, you may need to integrate with a system that is not as flexible. It might be that you can’t expose an HTTP endpoint. So, what do you do? This is where Integration Gateway comes in.


### 2. Delivering App Events with Integration Gateway (Low Code)


Integration Gateway’s real strength is that it’s able to mediate between a wide range of different protocols. It can handle transformations and it can handle orchestration of integration logic. This gives you a lot of flexibility in delivery, because if you’re talking to a system that is SOAP-based or file-based, you can still use Application Events to trigger the flow. You can do a lot of transformation before you perform the delivery. This means you can talk to almost any downstream system on the outbound.


For example, imagine an adjuster performs a task in ClaimCenter, like filing a claim. This will generate events: claim opened, contacts added, assignments being made, and so on. Each of those events is going to have data. The data is going to be the full claim graph. What I mean by the “claim graph” is the claim and all related information: incidents, exposures, parties involved, the policy snapshot, and so on.


This is a two-step process. These events occur and, as an intermediate step, we publish them outside of InsuranceSuite into an[Apache Kafka](https://kafka.apache.org/) topic. We maintain a snapshot store of the related claim or policy information. And then we have a set of services which will then pick up these events and deliver them to the external systems using the subscription information, as defined through a user interface (UI).


The point of a subscription is that a subscription will be defined for each of your integrations. And it says, “this integration cares about these events and it wants to send them to this destination.” It’s going to post these events to a particular web address that has been exposed by that downstream system.


## Application Events and Integration Gateway


Currently, Application Events and Integration Gateway are part of our[Early Access (EA) Program](https://www.guidewire.com/guidewire-early-access-program/?_gl=1*jjuqj3*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczNjM2ODU4Ny4xNjQuMS4xNzM2MzY5Mzk2LjYwLjAuMA..) for the Guidewire Cloud Platform.


If you’re interested in using one of the integration frameworks for one of your integrations, then you should talk to your Customer Success Manager about leveraging the frameworks and being part of our Early Access program.


If you’re one of our partners who’s interested in using one of the new frameworks, you should talk to your Alliances Manager and express interest so we can start getting you set up.


This concludes Part One of our two-part series. In Part Two, we’ll show you how Application Events works with an under-the-hood view.


*This blog post is a video recording and transcript of a session titled “Simplifying Outbound Integrations with Application Events in InsuranceSuite Cloud,” which was presented by Simon Reading and Mark Bolger at Connections 2021. The transcript has been condensed and edited for easier reading.*


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
