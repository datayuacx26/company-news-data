---
schema_version: "1.0.0"
document_id: "cec33b134027deb84136ba4f766a773ebcdfa7dce333dacbc80f0e5703f51b16"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-brex-transformed-developer-experience-and-slashed-infrastructure-costs-with-signadot/"
published_at: "2024-12-19T20:10:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:e02aa0cbff9495de2f5bd2dfd2055223283211b213226b8e53190bab104fcce7"
---

# How Brex Transformed Developer Experience and Slashed Infrastructure Costs with Signadot

[Brex](https://www.brex.com/) , a fintech company building integrated financial solutions for businesses, grew rapidly by empowering companies to manage their finances in innovative ways. To support their 1000+ microservices architecture and prioritize developer productivity, Brex built a homegrown “Preview Environments” system that would spin up complete replicas of their application stack for developers to test changes.


While initially effective for a smaller scale, this solution began to present significant challenges as the company grew - consuming substantial infrastructure costs and creating frustrating developer experiences that pushed teams toward problematic workarounds. This case study explores how Brex transformed its infrastructure approach to dramatically reduce costs while simultaneously improving developer productivity and satisfaction.


*Watch Phil Burrows (Head of Platform Engineering at Brex) talk about Signadot’s impact at Brex!*


## ‍ **The Challenge of Scale: When Microservices Meet Developer Experience**


With over 1,000 microservices in production, Brex’s engineering team faced a common challenge that comes with distributed architectures: how to enable developers to effectively test and validate changes across interconnected services.


“We had kind of taken microservices to their extreme,” explains Phil Burrows, VP of Engineering at Brex. “To make a change on any one service, you have dependencies on all these other services. Nobody ever builds microservices in a way that is fully decoupled - they build them in a way that it’s just like a distributed coupling, distributed monolith.”


*A new approach to Preview Environments*


Initially, Brex tackled this challenge by building a homegrown solution called Preview Environments. This platform would spin up a complete replica of the entire application stack - all services, databases, and dependencies—in an isolated Kubernetes namespace for each developer.


“It was a fairly clever solution,” reflects Burrows. “It solved the problem at the time it was built. But as Brex as an organization scaled, and Brex as an application and architecture continued to scale, it became less and less tenable and more and more expensive.”


## **The Hidden Costs of Preview Environment Sprawl**


The impact of Brex’s homegrown solution went far beyond just infrastructure costs and wait times - it was fundamentally changing developer behavior and team culture in concerning ways.


“There are all sorts of bad habits that you get into as an organization when it is difficult to iterate quickly,” Burrows explains. “You test things in production. You do other things to artificially shorten or shrink that inner loop that are not necessarily sustainable or beneficial long term.”


The immediate operational challenges were clear: environments took an hour to spin up initially, and even after significant engineering investment in pre-warming, developers still faced 20-30 minute wait times for deployments. The infrastructure costs had ballooned to the point where preview environments were nearly as expensive as production. Multiple engineers were dedicated full-time to maintaining this complex platform, yet system-wide failures affecting all preview environment clusters were a regular occurrence.


But the deeper impact on engineering culture was even more concerning. “It’s fairly demoralizing and just annoying and frustrating as a developer,” notes Burrows. “There are cultural implications, long-term cultural implications, to not addressing those types of things.” Developers, faced with lengthy feedback cycles, started finding workarounds. Some resorted to testing directly in production, while others began bundling multiple changes together to avoid the overhead of spinning up new environments. These practices led to higher change failure rates and riskier deployments.


“The more we got to think about it, the less it felt like that was the right way to do things,” reflects Burrows. “It just felt very wasteful. It felt very bloated, it felt over-engineered, and we kind of felt like there had to be a better way.”


## **Why Brex Chose Signadot**


After evaluating several vendors, Brex found Signadot’s approach to Preview Environments aligned perfectly with their needs. Instead of creating complete replicas of the environment, Signadot’s intelligent resource sharing and isolation capabilities offered a more efficient solution.


“We looked at a couple other vendors,” notes Burrows, “and honestly, it came down to Signadot for this specific use case. It fit what we were trying to do better than anything else, and it was more mature than the other stuff we were looking at. The return on the investment was obvious.”


## **Transformative Impact**


The implementation of Signadot has delivered significant measurable benefits:


1. **Massive Cost Savings** : “Just in infrastructure costs, it saves us a significant amount annually,” reports Burrows. “Our preview environments were nearly as expensive as our production environment in some regards.”
2. **Improved Developer Experience** : Developer sentiment surveys showed double-digit improvements in satisfaction scores after implementing Signadot.
3. **Faster Development Cycles** : The team moved from hour-long environment creation times to instant environment availability.
4. **Better Development Practices** : “It has allowed us to move to smaller shipping changes, building things more incrementally,” says Burrows. “The number of lines of code per PR has gone down over time.”
5. **Enhanced Quality** : With faster feedback loops, the team has seen improvements in their change failure rate and developer lead time metrics.


## **Looking Ahead**


While Brex has already realized significant value from Signadot for Preview Environments, they’re exploring additional use cases. The platform’s capabilities for creating ephemeral test environments could be leveraged for automated testing in their CI/CD pipeline, further improving their release velocity and quality. “We’ve been really happy with Signadot,” concludes Burrows. “I think it has exceeded our expectations, at least in terms of what we have been able to accomplish by leveraging it.” For engineering leaders and platform teams looking to improve developer experience in their microservices architecture, learn more about how Signadot can transform your development workflow at[https://www.signadot.com/](https://www.signadot.com/) .
