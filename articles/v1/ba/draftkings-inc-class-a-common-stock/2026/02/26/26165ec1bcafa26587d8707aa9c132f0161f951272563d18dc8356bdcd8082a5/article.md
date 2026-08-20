---
schema_version: "1.0.0"
document_id: "26165ec1bcafa26587d8707aa9c132f0161f951272563d18dc8356bdcd8082a5"
company_key: "draftkings-inc-class-a-common-stock"
company: "DraftKings Inc."
source_id: "draftkings-inc-class-a-common-stock-rss-016c40719db2"
canonical_url: "https://medium.com/draftkings-engineering/a-secret-that-keeps-the-website-running-strong-turning-an-great-idea-into-a-million-users-756aec800a83"
published_at: "2026-02-26T14:45:39+00:00"
first_seen_at: "2026-07-20T04:35:13.112015+00:00"
fetched_at: "2026-08-20T00:36:59.605120+00:00"
content_hash: "sha256:abc5224b4782474738c279fc3463791352ece51f1fdb43aa45c24c7b90ca5f66"
---

# A secret that keeps the website running strong: Turning an great idea into a million users

### **A secret that keeps the website running strong. How load balancers turn a great idea into a million users**


### Overview


One day, you get the great idea of creating a website that allows users to bet on a sporting event. Yep, that sounds interesting… Let’s build a website and have our applications run there. That’s pretty much how DraftKings story began.


Hosting an application on a website may look simple on occasion. Still, it always comes with complexity, especially when you want your dream to become true and have more users start using the application you built.


As more users start using the website, the server and applications need to be able to handle the increased user requests. You can always configure another web server and application to provide the necessary resources. Still, if you have a million users, as on the DraftKings website, this could cause a shortage of server and application resources, potentially leading to the website being offline.


What could be one option to overcome such an issue? That’s where a device known as a **load balancer** comes into play. It could be the key to your application’s long-term success.


### **The Need for Load Balancers**


*“As the users grow and the servers strain, load balancers become the heroes of the story.”*


In a basic web server configuration, administrators create a URL that is associated with the server’s IP address. When users enter the URL in their browser, they connect to the web server. While this setup works well initially, as the website grows, the demand for content can exceed the capacity of a single server.


Traditionally, administrators would respond to increased demand by upgrading the server, but this approach has limitations as the demand continues to grow. A more scalable solution is to employ load balancing, a technique that allows multiple servers to share the load.


Load balancers play a crucial role in web server scalability and efficiency by distributing the workload among multiple servers. The main goal is to improve overall system performance, maintain high availability, and reliability by avoiding a single point of failure and better resource utilization.


**How Load Balancers Work** Instead of a single server handling each request, all the servers are placed behind the load balancer, which determines which server will handle each request. The website’s URL points to the load balancer’s **IP address** instead of directly to a web server.
This way, when traffic increases, administrators can add more servers to handle the increased requests and load on them. Sometimes, **autoscaling** is used to automatically expand and contract servers.


Load balancers can also serve **security and application functions** , such as:


- Certificate management,
- URL filtering
- DDoS protection and rate limiting (dropping abusive or high-volume traffic).
- Session persistence (sticky sessions) to maintain user experience.


### Types


There are three main types of load balancer: ***hardware*** , ***software*** , and ***cloud-based*** .


- **Hardware** load balancers are primarily used in large enterprise data centers where ultra-high performance, security, and reliability are required.
- **Software** load balancers are preferred in flexible, customizable setups such as virtualized/containerized environments, as they do not require hardware investment.
- **Cloud/Managed** load balancers are best for organizations using public cloud platforms that desire automatic scaling with minimal infrastructure management.


**Hardware Load Balancers** Those are dedicated physical devices built to handle very high throughput. They use specialized chips (ASICs or network processors) for packet processing, packet inspection, SSL offloading, traffic shaping, caching, and compression, as well as special built-in security modules (WAF, DDoS protection).


**Software Load Balancers** Software load balancers are applications that run on hardware, virtual machines, or containers. They are often open-source and highly customizable to meet specific requirements. Software load balancers can also provide application-aware routing based on factors such as headers, cookies, URLs, SSL/TLS termination, and caching, to improve performance and reduce backend server load.


Software load balancers integrate seamlessly with automation and orchestration platforms such as Kubernetes and Terraform, making them useful for modern cloud-native environments.


**Cloud / Managed Load Balancers** Cloud /managed load balancers represent fully managed load-balancing services that are offered by major cloud providers such as AWS, Google Cloud, and Microsoft Azure. These services are designed to reduce the underlying complexity of provisioning, scaling, and maintaining traditional load-balancing infrastructure.


One of the primary advantages of cloud/managed load balancers is their elastic scaling, which automatically handles increased traffic without requiring manual intervention or configuration changes.


Depending on the organization’s infrastructure strategy, each has distinct advantages, limitations, and use cases:


***Hardware***


- *Advantages* Extremely reliable, high throughput with low latency.
All-in-one solutions (LB + WAF + SSL + monitoring).
Strong vendor support and SLAs.
- *Limitations* Very expensive
Harder to scale — requires buying and racking new appliances.
- *Use cases*
Large enterprises and financial institutions with strict SLAs.
Data centers need ultra-low latency and advanced security.


***Software***


- *Advantages* ****** Very flexible and configurable for almost any use case.
Cost-effective.
Active open-source communities and frequent updates.
Easier to integrate into CI/CD pipelines.
- *Limitations*
Performance depends on the underlying hardware/VM size.
Requires more operational expertise to configure and maintain.
- *Use cases*
Web-scale companies need flexibility and rapid changes.
Container-native environments (Kubernetes Ingress controllers).
Environments with tight budgets.


***Cloud/Managed***


- *Advantages*
No hardware or software to manage.
Highly scalable and resilient by default.
Seamless integration with cloud services (CDN, WAF, firewalls).
- *Limitations*
Limited to features exposed by the provider.
Less fine-grained tuning compared to self-managed software.
- *Use cases*
Cloud-native applications.
SaaS platforms need global traffic management.


### Algorithms


To ensure efficient resource utilization, load balancers use algorithms that fall into two main categories: ***static*** and ***dynamic*** . These algorithms can optimize response times, maximize throughput, and enhance the user experience.


The most famous static algorithms are **round-robin** , **weighted round-robin** , and **IP hash** , which follow fixed rules and are independent of the server’s current state. Dynamic ones, which examine the server’s current state before distributing the traffic, include **least connections** , **weighted least connections** , **least response time** , and **resource-based.**


- *Round-Robin* — requests are distributed to the servers in a cyclic order.
- *Weighted Round-Robin* — Each server is assigned a weight based on its capacity (e.g., CPU, memory), and requests are distributed to the more powerful servers that can handle more requests.
- *IP Hash* — The load balancer performs a hash on the client’s IP address, which is then used to determine which server should handle the request. This method ensures that requests from the same client IP address are consistently routed to the same server.
- *Least Connections* — The requests are forwarded to the server with the fewest active connections.
- *Weighted Least Connections* — Considers both the number of active connections and the server’s weight based on its capacity.
- *Least Response Time* — This method distributes the requests to the server that responds the fastest, measured by response latency.
- *Resource-Based* — This algorithm distributes requests based on each server’s current resource availability, such as CPU usage, memory, or network bandwidth, by using an agent that is explicitly configured and running on the servers.


### Wrapping up


Before making a decision on the most appropriate load balancer, it’s essential to evaluate the specific application's unique needs and traffic flow.


In DraftKings, our preferred type of load balancers is Cloud/Managed, which provides both application-aware (Application Load Balancers) and high-throughput passthrough (Network Load Balancers). Choosing the correct one always comes with a challenge, especially when you have a variety of traffic types — short-lived, long-lived sessions, bidirectional flow, and more!


Want to learn more about DraftKings’ global Engineering team and culture? Check out our[Engineer Spotlights](https://careers.draftkings.com/life-at-draftkings/engineering/?utm_source=medium&utm_medium=medium&utm_campaign=medium_blog) and[current openings](https://careers.draftkings.com/jobs/?utm_source=medium&utm_medium=medium&utm_campaign=medium_blog) !


---


[A secret that keeps the website running strong: Turning an great idea into a million users](https://medium.com/draftkings-engineering/a-secret-that-keeps-the-website-running-strong-turning-an-great-idea-into-a-million-users-756aec800a83) was originally published in[DraftKings Engineering](https://medium.com/draftkings-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
