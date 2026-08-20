---
schema_version: "1.0.0"
document_id: "8f01e17f990c30c54d641291c31bcf44e9ca523416ed2542237d6c03bf574522"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/cqrs2"
published_at: "2017-07-26T13:53:23+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:27:26.335944+00:00"
content_hash: "sha256:2000784558c36b6e4837095d747f4a96ca1fcda728cd1b393c3a8721804ff019"
---

# From CRUD to CQRS with Dropwizard (Part 2)

# **Part 2: Asynchronous Command Handling**


This is the second part of a multi-week series of posts describing various implementations of Command Query Responsibility Segregation (CQRS) and Event Sourcing using[Dropwizard](http://www.dropwizard.io/) . Week 1 can be found[here](http://dresscode.renttherunway.com/blog/cqrs1) . As I mentioned last week, CQRS is a design pattern where commands modifying data are separated from queries requesting it and the data is denormalized into structures matching the Data Transfer Objects (DTO) that these queries are requesting. If you want to learn more about CQRS, read Martin Fowler’s[blog post](https://martinfowler.com/bliki/CQRS.html) on the subject.


Picking up from last week, here’s the initial CQRS application we built:


This application was entirely synchronous. We didn’t send a response to the client until we had already denormalized and written the changes they requested to all data stores. In contrast, this week’s application looks like this:


The major difference, as you can see, is that instead of directly processing the commands created from the request, we are writing these commands to a message bus (after validating the request) and then[asynchronously](http://krondo.com/in-which-we-begin-at-the-beginning/) reading the commands out of the message bus, handling them, and writing our data. Handling the command consists of retrieving the existing state of the entity, applying the command, and generating an event if there is a delta. This event should encapsulate the change in state of the entity and also include some mechanism for ensuring[idempotency](https://en.wikipedia.org/wiki/Idempotence) (we’ll get into this more in a future post). So for example if a command tells our service to create a product, the event generated upon successful execution of the command would be a ProductCreated event which would contain the data for that new product’s current state.


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
2. Event(s) are generated


8. If the Command results in Event(s):


1. The event(s) are denormalized to relevant data sources


The steps for a data retrieval request (query) are the same as last week:


1. Http request is made to retrieve entity
2. Entity is retrieved via key lookup


There are now certainly more moving pieces and we have changed the user experience. Instead of being able to respond to the client and tell them we’ve completed their request, we can only tell them that their request has been received and looks good to the best of our current knowledge.


Comparing this approach to last weeks, there are some additional drawbacks:


- Client doesn’t know when command is handled
- Additional technologies- message bus
- Additional complexity- writing to and reading from the message bus


But there are also additional benefits:


- Write requests are validated and response sent to the client more quickly
- The handling of commands is decoupled from the handling of requests


- If the format of the request changes tomorrow then the command handling service doesn’t need to changed
- At some point in the future the client could directly write to the message bus or commands could be written from several sources and the command handler wouldn’t change
- The location of either the request handler or the command handler could change at any time with no need for service discovery or reconfiguration


- If our message bus can be partitioned (Kafka) then we can horizontally scale out our command handlers and ensure that all commands for a given partition key (ex SKU ID) are handled by the same instance of our command handler.
- If our message bus persists messages (Kafka) then we can replay requests for debugging or disaster recovery by using another consumer or moving the offset back


- Transient errors during command handling would automatically be retried with the correct Kafka settings in place.


Coming up in part 3: CQRS with eventually consistent data denormalization.
