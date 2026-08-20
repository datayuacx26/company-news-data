---
schema_version: "1.0.0"
document_id: "9e577c43d0b11a4bc37ed333353a2bd478c5ed84bc07a592fa478fb4e89e4971"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/edge-computing"
published_at: "2023-05-18T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:7faee7dd5539276d878225907a7306202abf81867ccf76de30e8d247952087c7"
---

# What is Edge Computing and How Can it Improve Your Website’s Performance?

Although the concept of edge computing is not new, an increasing number of websites are taking advantage of “edge functions,” and now some of the widely used JavaScript frameworks are promoting the use of “the edge” when building websites. A lot of the hype around “the edge” involves tackling performance bottlenecks and high latencies.


In this post, I will explain what edge computing and edge functions are, what problems they solve, and explore potential scenarios where the edge could be utilized.


What is “The Edge”?


To understand what the edge is, it is necessary to first have a fundamental knowledge of how data centers are organized within regions and zones. Most cloud providers have their servers broken up into different regions or separate geographic areas, for example US-East-1 (Northern Virginia) or US-West-2 (Oregon). Each region is designed to be geographically isolated from the other. Now, within each region there are multiple, isolated availability zones, which is where origin servers are hosted. “The edge” are those data centers that live outside one of the availability zones, and “edge functions” are lightweight processes run on servers hosted in one of these data centers.


1 These may also be called


edge locations


or


points of presence


.


Availability Zones vs. Edge Locations


To give some context on how many availability zones there are compared to edge locations, here is a map from Google Cloud Platform (GCP) that illustrates all the availability zones from around the world.


Image sourced (with permission) from


[Google Cloud Platform](https://cloud.google.com/about/locations#network.)


2


Now let's contrast that with a map showing all the edge locations.


Image sourced (with permission) from


[Google Cloud Platform](https://cloud.google.com/about/locations#network)


2


As you can see, there are several more edge locations than availability zones across the globe. Depending on the location from which users are accessing your site, you can greatly increase site performance if you can use edge locations to handle some of your busiest pages or requests.


1 Target’s consumer base is mostly in the United States, but having an edge location to process these requests could make a significant impact on latencies for guests in a region like the central United States where there isn't a close availability zone.


How Are Requests Handled at the Edge?


Image created by author using


[Mermaid](https://mermaid.js.org/)


The picture above is a simplified version of how requests are handled at the edge. When a user requests content from the content delivery network, or CDN, the request is automatically routed to the closest edge location to the user. The CDN then checks to see if it has a cached version of the requested content. If the requested content is found, then the CDN serves the content to the user immediately. However, if the content is not found in the cache or the edge location cannot handle the request (perhaps it needs more data), then the edge location forwards the request to the origin server where it will handle the request and send it back to the edge location. After the content is delivered, the edge location may then cache the requested content.


1


Edge Functions vs. Serverless Functions


Serverless functions are event-driven, quickly-loading, and quickly-executing programs that run in the cloud and allow for scaling and infrastructure management. In contrast to edge functions, serverless functions are not tied to the CDN and are typically used for processing important transactional events such as an HTTP request or database update.


3


Here are some quality attributes to consider when considering edge versus serverless:


- Responsiveness – Edge functions run closer to the end user, which can significantly reduce latency and improve performance.


- Usability – By reducing latency, edge functions can improve the user experience by enabling faster load times and smoother application interactions.


- Affordability – By processing requests closer to the end user, edge functions can offload some of the workload from the origin server, which can reduce the need for costly server resources.


- Manageability - Edge functions are run on edge locations that are typically owned and managed by the CDN provider, while serverless functions are run on origin servers that are owned and managed by cloud providers.


- Efficiency - Less CPU time is available to edge functions. Cloud providers restrict CPU time on edge functions to guarantee that they are performant considering how often they are used. This essentially means that you can perform fewer operations than you can within a serverless function.


- Debuggability – Edge functions can be more challenging to debug than serverless functions. There might be fewer logs and tools available to the developer that help diagnose issues depending on the CDN provider.


- Securability – Traditionally, serverless functions offer more cyber security layers. Edge functions offer a more decentralized structure with more entry points, so more additional security considerations are needed to ensure the functions do not expose vulnerabilities. However, as edge functions are becoming more popular, cloud providers are adding additional access controls, encryption, threat detection and mitigation that will help address these security concerns.


Use Cases for Edge Functions


In the beginning of December, Target had its annual innovation week and one of the projects was an edge computing experiment that explored how we could take advantage of this technology. Here are some of the possibilities we explored:


- User Authentication


- To make a purchase at Target on the web, you need to be signed in. Because of this, putting user authentication at the edge can offer some very significant advantages to both our developers and guests. This includes better security, maintainability, increased privacy awareness, and faster performance.


4 At the edge we can cache content that requires authentication to access, and requests relating to the authentication process are answered at the edge directly.


- Determining Guest Location


- One performance bottleneck that Target faces is determining a guest’s preferred or nearby store. We must first determine a guest's location to show search results, fulfillment options, and even add an item to cart. Today, this is done in the browser and can be quite delayed. A potential solution is to move this logic to the edge and use our CDN’s geolocation feature to look up all the stores we need and send them as cookies to the browser for immediate use.


Looking Ahead


By examining a few straightforward use cases of edge functions that moved from being delivered by the origin server to the CDN and at the edge, we have just scratched the surface of edge capabilities. The popularity of edge-first frameworks is growing and requests to the edge are gradually replacing handling them at a far-off origin server as the desired default.


1 It is exciting! The user experience will be enhanced if you can serve more requests and data close to the user. The edge is the future, and I hope that this post has inspired you to learn more about it and even try it on your site.


Sources & Attribution


1[Jamstack Conf 2022](https://jamstack.org/conf/) (presentation by Erica Pisani \[Senior Engineer, Netlify\] regarding edge computing)


2 Data Center Organization Image Source:


[https://cloud.google.com/about/locations#regions](https://cloud.google.com/about/locations#regions)


3[What is Edge Computing](https://www.cloudflare.com/learning/serverless/glossary/what-is-edge-computing/#:~:text=Edge%20computing%20optimizes%20Internet%20devices,reduces%20latency%20and%20bandwidth%20usage)


4[Simplifying Authentication with Oauth at the Edge](https://www.fastly.com/blog/simplifying-authentication-with-oauth-at-the-edge)


## RELATED POSTS


### Product Engineering @ Target: Engaging With Our ‘Why'


By Nancy King, May 2, 2022


Our product engineering organization builds a large proportion of the applications that our millions of guests and 350,000+ team members use, from the mobile app and website to our point-of-sale software, to the HQ systems supporting our operations. These are the technologies that help bring Target’s purpose to life and grow our business online and across nearly 2,000 stores nationwide. The key to success is really knowing and supporting our users across the country.
