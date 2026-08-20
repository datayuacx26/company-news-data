---
schema_version: "1.0.0"
document_id: "784877060cb8684a01c7eeb32b2776d478a11bde726285803a181956e360822e"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/agents-were-built-for-the-5-vps"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:841eb1ca4b4b0fbd9baf970d8077dd3af6de9422aa22a5c57f37723e585045fb"
---

# Agents were built for the $5 VPS

Today's application and sites are being hosted on tons of serverless platforms, but it's about to become clear that this is the "MCP" of the hosting world. Just like we moved from "MCP" to file systems, the world will move from serverless platforms back to the "virtual server" as the de-facto hosting strategy in the agent era. Let's talk about why.


## How things used to be


Before all of this "javascript" happened, you used to deploy websites and software the old-fashioned way, by installing everything onto a server. LAMP was the name of the game, the best stack to use was Linux (for your OS), Apache (for your webserver), MySQL (for your database), and PHP (for your actual content).


The nifty part about these olden times (or the now times if you're[@levelsio](https://x.com/levelsio) ) is that everything was really easy. If you wanted to test your application locally, well, you had the files, you could install the same webserver, you could run MySQL, and just see the same results. If you wanted to deploy to the cloud, you would just ... copy your stuff up to the box -- files could use ftp and your database might need to be backed up and restored.


There was just one box, one place where everything in your cloud lived (okay, maybe you still had a managed database), but you could easily SSH into that box and poke around at ... the server logs, the CPU usage, the request count. There was no external observability stack, the tools that you used to debug regular Linux processes just worked in this world.


## Then comes scale


As time went on, and the great Web 2.0 applications needed to handle more and more traffic, people realized that putting everything on a single-box was a recipe for not being able to grow your site traffic.


- If your database is on one box, how do you grow it past the storage size of that box?
- If your web server is on one box, how do you allow it to serve an unbounded number of requests per second?
- If your code is on one box, how do you make it so that's low-latency for people all around the world to access?
- If you only have one box, what do you do if that box has downtime or needs to be replaced?


The answer to these problems was, of course, horizontal scalability. What if these things weren't on a single box, and they were instead fully-managed, highly-available services. They could even be geo-distributed for latency reasons! The region? Earth.


This was really, really hard to build, and the companies that desperately needed this kind of scale (Facebook, Amazon, Google) spend tremendous amount of resources making them possible. The people who worked at these companies left, realized that building for high-scale was the right thing, and slowly diffused these technologies to developers by making them easier and easier to use -- which leads us to the crop of "neo-clouds" that we have today.


There is just one problem with this story. It used to be the case that you could just SSH into your server and understand the full state of the world. Is it working? Is my database down? Am I burning too much CPU? Why did that request get a 500 error? These were easy to answer on one server, but nearly impossible to answer if your code was running across thousands of anonymous servers that you don't have access to across the globe.


And so, for the industry's second act, we needed to invent a bunch of new observability tooling which would be able to aggregate data across all of these hosts and provide value to the developers who were trying to understand their systems.


Thus, we end up in state where a website could be deployed on Vercel, using a database from Neon, and sending logs and telemetry to a third system like Datadog.


## Enter the agent


Now, in 2026, the world has changed tremendously again. We have all of this amazing tooling that lets us create and build software faster than ever before.


This means that far more people will be creating software than before, and there will be a LOT more of it. Interestingly though, the "load" that this software needs to support will not be evenly distributed. As more and more software becomes "personal", the average piece of software will actually have lower performance requirements than at any time in the past (this, obviously, excludes the core infrastructure that this vast new quantity of software runs on, which will need to support much higher scale than before).


These AI tools are really good at using Linux machines because the labs have spent a tremendous amount of effort RL'ing these models into being good at coding (which is ultimately the same skill as using a Linux machine).


If you look at an agent harness like Claude code today, how does it actually debug a problem with your website deployed on the Vercel, Neon, Datadog stack? Well, it needs to learn the context of the architecture -- figuring out what services your application is using. Then, for the problem, it might need to cross-correlate a bunch of data from these non-standard tools in order to determine whether your database is overloaded and needs more compute, whether there's a code issue, or whether there's some kind of CPU overloading happening.


This starts to look a lot like the problems we were seeing with MCP earlier in the year. The more non-standard tools that the agent needs to use in order to determine the context of the problem, the lower the accuracy and performance. This is why the industry shifted towards giving the agent a standardized set of tools, like bash and the file system, which could represent any kind of problem without the agent needing to learn how to use them.


Now consider how you would debug a problem with Claude code if you were[@levelsio](https://x.com/levelsio) and just deploying everything to a single-box VPS. You just ... SSH to the box, call Claude, and it has all of the context necessary locally (from the code \[present on the box\], to the database \[present on the box\], to the logs \[present on the box\], to the CPU usage \[present on the box) to make a determination. In fact, I think that he actually says that this is exactly how he's using AI today.


This is way simpler, uses lowest-common denominator tooling (the bitter lesson of infrastructure), and is going to lead to the agent becoming higher performance for these types of problems.


As a result, I think that, moving forward, the vast majority of applications are actually going to move to a deployment strategy in which everything appears to be running locally on the current machine, because this is what gives Claude the highest performance to operate on the computer-as-context.


The more services that your application depends on and uses, the more worried you should be about your application being AI-ready as the software lifecycle becomes a closed-circle, with AI triaging and fixing most of the issues.


## But I know this is secretly about file systems, right?


Yes, of course. I think that, had we (as an industry) invested more into building amazing storage solutions, we could have avoided some of the pitfalls of creating thousands of different point-solutions that customers need to cobble together to build a working application. If you are able to share the same context (via an infinite, high-performance file system) of your... database, logs, and code, even if the application is actually running on thousands of machines spread across the globe, then we would have had an industry better prepared for tools that work better when context is co-located on the same system.


This is where I see three companies, us at[@archildata](https://x.com/archildata) ,[@ssh_exe_dev](https://x.com/ssh_exe_dev) , and the[@flydotio](https://x.com/flydotio) Sprites building interesting solutions towards.


exe.dev and sprites are working to bring the "virtual server" into the 21st century. You can login and get a box that can host network services, like databases and websites, and treat it as your own -- it's always addressable, the disk stays around, and it otherwise just "looks" like a virtual server. You can run all of your services here, or SSH in and call Claude to debug the system.


[@archildata](https://x.com/archildata) is taking a similar, but different approach. We are envisioning a world in which users have lots of ephemeral compute (which could be, geo-distributed, replicated for redundancy, etc) that all points to the same file system. I think that while exe.dev and Sprites may get us to a place where every developer can have their own server, Archil is going to get us to a place where we can replicate the benefits of serverless (high-availability) with the best outcome for agents (context is all close together and easily accessible).


You can stuff your code, database, and logs directly into an Archil file system and have them work (even if they're on multiple hosts), or you can spin up an ephemeral session with Claude to poke around all of these services to better understand how they're working.


Regardless of what ends up being the right shape of what a virtual server becomes, it's clear that agents will perform better when everything is in one place.
