---
schema_version: "1.0.0"
document_id: "545ed6f3df2ca582e3bfb78300858ca3db8d3408e86613fedac3189676840f6a"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/cqrs1"
published_at: "2017-07-12T15:44:21+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:551a0175e8098d1ea0f593f04553988b22ca3fd2edf031ad6cf1c589a2a86c1e"
---

# From CRUD to CQRS with Dropwizard (Part 1)

# **Part 1: Synchronicity Everywhere**


This is the first part of a multi-week series of posts describing various implementations of Command Query Responsibility Segregation (CQRS) and Event Sourcing using[Dropwizard](http://www.dropwizard.io/) . CQRS is a design pattern where commands modifying data are separated from queries requesting it and the data is denormalized into structures matching the Data Transfer Objects (DTO) that these queries are requesting. I’m not going to get deep into the details of CQRS here, if you want to learn more then I highly recommend Martin Fowler’s[blog post](https://martinfowler.com/bliki/CQRS.html) on the subject. But here is a quick comparison between CRUD (Create, Read, Update, Delete) and CQRS. A typical CRUD application looks like this:


As you can see, there’s a User Interface which writes and request data from an API which in turn persists and retrieves it from a data store.


In contrast, here’s a basic CQRS application:


The major difference, as you can see, is that we are separating the source of truth written to by the api from the projection which is read by the api. A denormalizer is used to keep the two in sync. In future weeks we’ll introduce[asynchronicity](http://krondo.com/in-which-we-begin-at-the-beginning/) , message buses like[Kafka](https://kafka.apache.org/intro) , and[eventual consistency](http://www.allthingsdistributed.com/2008/12/eventually_consistent.html) . But for our initial purposes we will assume that this[denormalization](https://firebase.googleblog.com/2013/04/denormalizing-your-data-is-normal.html) will be done synchronously with the update of the source of truth and prior to the response being sent to the user interface.


I’ve created a small Dropwizard application to demonstrate this pattern using Mongo. You can find the code and instructions on how to run it in IntelliJ and send commands to it via Postman[here](https://github.com/RentTheRunway/cqrs-demos) .


The steps for a data update request (command) are:


1. Http request is received by API to change entity
2. Request is translated to command(s)
3. Command(s) are handled
4. Existing entity is retrieved and command(s) are validated
5. Command(s) are applied to entity and delta is determined
6. If there’s a delta:


1. The entity is updated
2. Event(s) are generated


7. If the Command results in Event(s):


1. The event(s) are denormalized to relevant data sources


8. Response is sent to client


And the steps for a data retrieval request (query) are:


1. Http request is made to retrieve entity
2. Entity is retrieved via key lookup


As you can see, data changes require a few extra steps but data retrieval is extremely simple. This would still be the case regardless of how many sources of truth must be denormalized and combined to form the document we need for the query.


However there are still some drawbacks:


- Duplicate data storage
- Transactions across data stores need to be handled by application
- Denormalization needs to be carefully managed to avoid inconsistent states
- Writes are slower/more expensive since we are synchronously denormalizing
- Reads can result in large payloads depending on domain design


However, in some cases these are outweighed by the benefits:


- Reads are faster and can be optimized separately from writes
- Since data is stored as key/value, lower level/cheaper data storage can be used
- Fewer http calls on read since documents are integrated on writes
- UX doesn’t need to change because consistency model is the same
- We don’t need message buses (yet!)


Coming up in part 2: CQRS with asynchronous commands
