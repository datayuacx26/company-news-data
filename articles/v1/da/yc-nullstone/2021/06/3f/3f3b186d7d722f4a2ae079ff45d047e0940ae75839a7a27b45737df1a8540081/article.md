---
schema_version: "1.0.0"
document_id: "3f3b186d7d722f4a2ae079ff45d047e0940ae75839a7a27b45737df1a8540081"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/phase2-growth"
published_at: "2021-06-04T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:06718ece67a2c630d852705b9d1ed08eeb0d856a4613970d24ecb6853e47502e"
---

# Application Infrastructure: Navigating the Growth Phase

## What does the Growth Phase Look Like?


Entering the Growth Phase for a software application company is an exhilarating time. You have proven traction in the market and you have lots of customers signing up. This is great for the business but as the head of your technology department, this translates into a whole new set of challenges. At this point, the application that you whipped together during the \[Startup Phase\](phase1-startup) needs to mature into a reliable, secure, and performant system.


In the Growth Phase of your company, you probably have 10+ developers on your team and you can't afford a dedicated team taking care of your infrastructure needs. So what do you do? Do you train your team on all the latest technologies such as Terraform, Kubernetes, Envoy, etc and try to piece together a new system architecture with the team that you have? Can you pull this off at the same time that you are working on splitting your monolith into a distributed system and adding new product features? As CTO of a Growth Phase company with a team of 14 engineers, I found myself in this scenario.


### Situation and Constraints


In the early stages our company, the development team wore many hats. In assessing the problem of scaling and enhancing our infrastructure, it was very tempting (given the budget constraints) to continue to operate this way and just have the developers learn new tricks. However, I knew the depth of knowledge and intricacy that was lying beneath the surface with regards to hosting and scaling software infrastructure. I knew that I needed to have my teams stay more focused on building software and become more specialized, not spread more thin.


At the time, we were hosting our application on virtual machines so moving to the cloud was very attractive. Unfortunately, moving to the cloud isn't as easy as it should be. Cloud hosting is a fantastic service but you still have to understand and be able to piece together the intricate details of networks, load balancers, DNS, security, firewalls, etc. If you don't get this right, you can easily set yourself up for failure and be in the news for all the wrong reasons.


### Evaluating Options


As I evaluated options, I thought about supplementing our team with outside consultants in order to establish a quality baseline and jump-start the process of moving to the cloud. While an outside consulting group could certainly help us re-architect and move into the cloud, we would still need to support and extend the solution after the contract was completed. In order to maintain our infrastructure moving forward, we would need to start hiring SREs and Platform engineers. One additional challenge to this approach is the difficulty of finding a quality and trustworthy consulting group. It takes a lot of time, energy, and skill to find, interview, and schedule these consultants.


I also looked at various tools and technologies that might help streamline this effort. Is it possible that someone has figured out how to leverage software to make this much simpler and automated? As I looked at various PaaS (Platform as a Service) tools, it became evident very quickly that these tools haven't evolved enough to solve our problems. PaaS systems are a great tool for the early stage startup but they just weren't going to be able to scale with us. What would we do if the vendor didn't support a language/framework that we needed to use, or a database with particular configuration, or perhaps even a newer infrastructure technology? Our company used Elasticsearch and \[Citus\](https://www.citusdata.com/) (a distributed form of Postgres) to handle large volumes of data for analytics and the hierarchical nature of our system. At the time, PaaS vendors didn't offer Elasticsearch or Citus so this was very quickly eliminated as an option.


### Our Solution


The solution that we ended up putting into action was two-fold. First, it was clear that we were no longer able to get by without dedicated resources for infrastructure and DevOps. We created a new dedicated role for an SRE (Site Reliability Engineer) and within a year this was a two person team. Second, we hired a consulting group to help us migrate to AWS and accelerate the knowledge and abilities of our new team.


While we certainly solved our problems of moving to AWS and creating a more robust infrastructure for our software application, I never got the feeling that our path was very efficient or cost effective. The entire process took us a year to complete and we spent tens of thousands in consulting costs. AWS did help cover the bulk of the consulting costs (to incentivize us to choose them) but I always thought there had to be a better way. Most of the work the consulting group delivered at this stage of our infrastructure should have been cookie cutter with a few unique differences from one customer to the next. As I have continued to talk and work with various Growth Stage companies over the years, I have learned that this problem is widespread and consistent.


### How Nullstone Can Help


Helping businesses with problems like the ones above are what Nullstone was created to solve. Nullstone is a platform used to launch applications quickly and built to be extended. We want to help businesses address their infrastructure needs in an efficient and reliable way.


To get started with Nullstone, you can launch applications just like you would any other PaaS (Platform as a Service) such as Heroku or Firebase. The out-of-the-box application patterns delivered by Nullstone set you up with a secure, industry best practice infrastructure on your own cloud provider such as AWS, Google, or Azure.


### Extending and Customizing Nullstone


As your needs begin to change and become more sophisticated, simply customize or extend Nullstone to fit your needs. Customizing and extending is done using the tools that your SREs and Platform Engineers already know; Terraform. Perhaps you would like to move between cloud providers? Perhaps you have the need for a multi-cloud solution? Or perhaps you need to integrate a brand-new or in-house technology into your infrastructure? Many companies at this stage start having needs for serverless infrastructure (AWS Lambda or Azure Functions), specialized databases, Google BigQuery or DataBricks, and the list goes on. These are all scenarios that Nullstone is built to handle.


### Solving with Nullstone


In a situation like mine described above, Nullstone would have saved months of effort and would have given us the flexibility we needed for the future. We would have launched 90% of our infrastructure using the out-of-the-box modules and patterns provided by Nullstone. For the edge cases like Elasticsearch and Citus, we would have built a few Terraform modules to satisfy that need. The Contract Pattern utilized by Nullstone would have allowed us to easily plug these new modules in and connect them to the rest of the infrastructure.


### Build for the Future


As new technologies emerge, Nullstone will continue to provide out-of-the-box solutions so that you can continue to focus on building your applications. However, there will always be cutting edge or in-house technology that you need to incorporate into your application infrastructure. When those scenarios come along, make sure you have chosen a system that will extend and grow with your business.
