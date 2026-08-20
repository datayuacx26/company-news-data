---
schema_version: "1.0.0"
document_id: "a6e00cdf09ecc014cc237dd423e985ff7510f139874ae4b5086aa02f19179a41"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/cqrs3"
published_at: "2017-10-02T15:14:42+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:ff48df9e151c822208800bd6cf9c732026c319c19b1040c37d34c55935a0788a"
---

# From CRUD to CQRS with Dropwizard (Part 3)

# **Part 3: Eventually Consistent Denormalization**


This is the third part of a multi-week series of posts describing various implementations of Command Query Responsibility Segregation (CQRS) and Event Sourcing using[Dropwizard](http://www.dropwizard.io/) . Week 1 and 2 can be found[here](http://dresscode.renttherunway.com/blog/cqrs1) and[here](http://dresscode.renttherunway.com/blog/cqrs2) . As a quick refresher, CQRS is a design pattern where commands modifying data are separated from queries requesting it and the data is denormalized into structures matching the Data Transfer Objects (DTO) that these queries are requesting. If you want to learn more about CQRS, read Martin Fowler’s[blog post](https://martinfowler.com/bliki/CQRS.html) on the subject.


Picking up from last week, here’s the CQRS application we built:


And here is this week’s application:


The major difference, as you can see, is that now we handle events in a similar manner to the way we were handling commands last week. Instead of directly processing the events created from the command, we are listening to the change log of our source of truth, writing events to a message bus, and then[asynchronously](http://krondo.com/in-which-we-begin-at-the-beginning/) reading the events out of the message bus, denormalizing the data contained in them, and writing it to the data store backing our DTO.


I’ve created another small Dropwizard application to demonstrate this pattern using Mongo and[Kafka](https://kafka.apache.org/intro) . You can find the code and instructions on how to run it in IntelliJ and send commands to it via Postman[here](https://github.com/RentTheRunway/cqrs-demos) .


The steps for a data update request (command) are:


1. Http request received to change entity
2. Request is validated
3. Request is translated to command(s)
4. Command(s) are written to message bus
5. Response is sent to client
6. Command(s) are pulled off of message bus
7. Command(s) are handled


1. Existing entity is retrieved and command(s) are validated
2. Command(s) are applied to entity and delta is determined
3. If there’s a delta:


1. The entity is updated
2. A service listening to the the change log of datastore registers the entity update and generates event(s)
3. The event is written to a message bus


8. If the Command results in Event(s):


1. Events are handled off of message bus
2. The event(s) are denormalized to relevant data sources


The steps for a data retrieval request (query) are the same as last week:


1. Http request is made to retrieve entity
2. Entity is retrieved via key lookup


There are now even more moving pieces and we may have changed the user experience. Now we can no longer assume that the data in our data stores is strongly consistent and we need to make sure to take this into account when displaying data to our users.


Comparing this approach to last week’s, this is the main additional drawbacks:


- Denormalized data is only eventually consistent


But there are also additional benefits:


- Commands are handled more quickly and written to the source of truth without having to wait for synchronous denormalization
- Decoupling between the service handling commands for the source of truth and the services reacting to events that are emitted from the source of truth
- Resiliency via the message bus to ensure each message from the denormalizer(s) is handled at least once.


Coming up in Part 4: CQRS we take a step back and start to discuss some of the topics and design patterns that are required to understand and implement systems like this.
