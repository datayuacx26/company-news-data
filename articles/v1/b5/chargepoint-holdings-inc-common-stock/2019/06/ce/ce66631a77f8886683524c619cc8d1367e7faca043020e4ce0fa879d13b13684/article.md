---
schema_version: "1.0.0"
document_id: "ce66631a77f8886683524c619cc8d1367e7faca043020e4ce0fa879d13b13684"
company_key: "chargepoint-holdings-inc-common-stock"
company: "ChargePoint Holdings Inc."
source_id: "chargepoint-holdings-inc-common-stock-news-import-2405547b27fc"
canonical_url: "https://www.chargepoint.com/engineering/how-we-streamlined-global-charging-inter-cloud-communication"
published_at: "2019-06-02T17:00:39+00:00"
first_seen_at: "2026-07-21T13:13:26.870476+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:3b2f17a58da65fdb475e41a773e7354fdd8ac56a23033355d3573bee6d0a39be"
---

# How We Streamlined Global Charging with Inter-Cloud Communication

ChargePoint operates the world's largest and most open EV charging network. We're on a mission to get everyone behind the wheel of an EV and provide them a place to charge wherever they go. Currently, we've got over 65,000 charging stations worldwide, and a driver plugs into our network every 2.0 seconds. Each of these stations connect to and communicate with the ChargePoint Cloud, managed by our Cloud Software Team.


### Evolving Company, Evolving Cloud


For the first ten years of our company's history, we operated mostly within North America. A single cloud instance located in North America proved sufficient to cater to the needs of this region. When we expanded our operations into Europe and Australia, however, our architecture had to evolve to ensure compliance with more stringent data privacy laws. Because maintaining our users' privacy requirements is our highest priority, we decided to create separate cloud instances for each region. Each cloud instance would be self-operating and able to serve the specific business needs of its particular region.


Each instance's isolation from other cloud instances, however, would result in inoperability for drivers from one region trying to use stations located in a separate region, thereby compromising the experience of drivers charging up outside their home regions.


In order to provide a seamless charging experience to our roaming drivers without compromising on regional data privacy requirements, we needed to devise a way for each cloud instance to communicate with its counterparts in other regions.


This was no simple task because the business logic of each cloud instance was tightly coupled to underlying databases, making it difficult to locate information for separate cloud instances.


To solve this challenge, we needed to build out: a) object locator services, and b) a global event system.


### Object Locator Services


Object locator services are able to discover any object of a given type in any region, as long as the object belongs to ChargePoint. Enabling these services allowed us to separate the business logic from datastores and move towards a service-oriented architecture.


The object locator services utilize a generic request delegation framework, which is made up of two components: a routing service that is responsible for finding the cloud region that any given request needs to be routed to, and remote handlers, which are the destination endpoints within the remote cloud that locator requests are delegated to.


The object locator service abstracts the database layer from our application's business logic. So now, whenever our application needs an object of a certain type (e.g. "user"), it sends a request to the object locator service with an associated object ID and type. The service first attempts to return that object from its own cache. In case of a cache miss, it uses the routing service to determine where that particular object ID belongs to. Once the cloud region has been determined, the service invokes the remote handler for that object type in that specific cloud instance. The remote handler then constructs and returns that object to the object locator services, which, in turn, caches that object and returns it to the client.


Throughout this whole process, there are safety checks in place to ensure that cross-region requests are restricted to roaming drivers who are moving across regions, thereby maintaining compliance with local data privacy laws.


Factoring in these object locator services, our business logic changed as depicted below:


### Global Event System


While the object locator service solves the issue of locating objects of known types and unknown cloud instances, we still needed a mechanism for a cloud instance to send arbitrary data (of an unknown type) to other cloud instances.


To facilitate this, we built a global event publishing system, in which each cloud instance can publish messages for other cloud instances and receive messages from other cloud instances. Leveraging RabbitMQ's Federation Plugin, we devised a dedicated exchange in each cloud instance's region and followed the mesh topology in order to set up federation among these exchanges. The mesh topology ensured that any message published to that exchange in any cloud instance would automatically be published to all federated exchanges in other cloud instances.


The application ensures that messages are tagged with a destination cloud instance prior to getting published to the federated exchange. Once messages have been received, each cloud instance only processes the messages intended for itself, and discards the rest.


Through the implementation of object locator services and a global event system, we were able to make inter-cloud communication possible, and thus improve the charging experience for our roaming drivers.


Want to tackle challenges like these and help build out the fueling network of the future? Come join our team!


[See Openings](https://www.chargepoint.com/about/opportunities/)


The information contained herein is considered proprietary and for informational purposes only and is not to be used or replicated in any way without the prior written authorization of ChargePoint, Inc.
