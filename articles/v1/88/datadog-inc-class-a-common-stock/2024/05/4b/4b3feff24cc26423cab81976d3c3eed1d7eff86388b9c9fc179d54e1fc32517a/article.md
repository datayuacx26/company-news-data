---
schema_version: "1.0.0"
document_id: "4b3feff24cc26423cab81976d3c3eed1d7eff86388b9c9fc179d54e1fc32517a"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/engineering-vp-spotlight-ivo-dimitrov/"
published_at: "2024-05-21T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:3006ee0265976cf1d72f33c41b000d15b4db61c3944bd3f470b2ac615e13ff68"
---

# Engineering VP spotlight: Ivo Dimitrov

Thomas Sobolik


Senior Technical Content Writer


Ivo Dimitrov


*In this edition of the Datadog Engineering Spotlight, Tom from the Community team sat down with Ivo Dimitrov, one of our Engineering VPs. Tom and Ivo spoke about Ivo’s career as an engineering manager for several top organizations, his transition from individual contributor to manager, and what excites him today about the distributed data systems his teams are building.*


*This interview has been edited for clarity and length.*


## How did you originally become interested in software engineering?


I’ve been in the industry for more than 30 years—a long time! Growing up, I always knew that my career would have something to do with engineering. I thought it might be civil engineering, but I was also very attracted to electronics. So when I went to college, I ended up pursuing electrical engineering. While studying[digital control](https://en.wikipedia.org/wiki/Digital_control) systems, I wrote software for the first time, and eventually contributed to a real-time operating system kernel. Once I got into coding, there was no going back. I loved it. I spent a lot of time developing system software and writing in C, and it just eventually became my career.


At the beginning of my career, I did about 10 years of hands-on development of various types of software, and always gravitated towards high-performance, low-level system programming, and software development in C and C++.


## How did you transition from being an individual contributor to management?


When I started my final IC role at Microsoft in 2006, I was on a team that was working on a very early version of what is known today as Azure Blob Storage. A couple of years later, we went through a reorg. My team needed a new leader, so I decided to give it a try. I was naturally curious about what team leadership was like, and I saw it as an opportunity to pick up new skills. You can say it was a bit of an accident. At the beginning, it was super hard because I didn’t have any prior management experience and had to learn on the job.


Ivo Dimitrov is the VP of Engineering for Datadog’s Distributed Data Systems organization. Ivo Dimitrov is the VP of Engineering for Datadog’s Distributed Data Systems organization.


Luckily, though, I was surrounded by smart people and there were a lot of experienced leaders at Microsoft to mentor me. Microsoft was a mature company that offered a lot of management training, which I could do while working in the role. The training covered different aspects of management, from communication to how you treat others and how to resolve conflicts—all requirements for being a successful manager.


During that transition, I discovered that I liked the extra responsibilities that came with leading my team. I felt I could really act like an owner and proactively catalyze others—not just be responsible for my own work. And that seemed to play to my technical strengths and everything I had learned in the first 10 or so years of my career. I learned how to navigate cross-org and cross-discipline boundaries, and refine the software development craft.


I still love being an engineering manager. I don’t mean to throw cliches around, but I was born in Eastern Europe, and in a sense, this career has been my American Dream.


## You were still passionate about writing code, and as a manager, you didn’t lose that sense of ownership of the deeper work. How?


I enjoyed having a bigger scope and broader responsibilities, and I really enjoyed working more closely with other engineers. I felt that as a team lead and later as a manager of a large engineering organization, I could strike a balance between going deep into the inner workings of the distributed systems we were building while also having a deep understanding of why we were doing it. I found it very rewarding to steer my teams and be part of dreaming up the broader vision of the organization.


## What were a few storage infrastructure projects you worked on that inform the work you’re doing now at Datadog?


Back in the day at Microsoft, I worked on systems that made up the storage tier for Hotmail. Then I went to LinkedIn in 2014, when it was a much smaller company than it is now. They used a lot of open source technology, like MySQL and Java, which was a very different environment than what I had been used to. I had to immerse myself in a completely different culture.


The first team I led at LinkedIn was working on a proprietary key-value storage solution, codenamed Espresso. I was lucky to have a great team, and together we took the Espresso platform from its early days to a mature platform that’s probably still handling and powering about 95 percent of LinkedIn’s data sets today. It’s a very scalable solution and I’m proud of it.


I ended up developing and overseeing a few other projects in the storage space. One of those was codenamed Venice, which is an open source container for serving derived data. Another was Ambry, an open source blob storage solution. There was also Helix, an open source cluster manager. All of them were internet-scale storage platforms that powered the core business at LinkedIn.


## What led to your decision to join Datadog? What was it like when you first started?


I didn’t know much about Datadog at first, but the recruiting team put me in touch with the SVP of Core Engineering. That first conversation made a great impact on me and quickly sparked my interest in the company. Over the course of several conversations with senior folks at Datadog, I realized that three things about Datadog resonated very strongly with me.


First was the people. I could tell I was talking to very smart, very capable, serious people, and things really clicked. They knew their craft, the problem space, and the salient engineering topics, and I felt that we were speaking the same language.


Second, I thought Datadog’s technology was very cool. At LinkedIn, which was a much larger org than Datadog, we had built up a lot of legacy code and tooling, which eventually started slowing us down. That wasn’t the case at Datadog—there was practically no red tape. People were encouraged to take intelligent risks, deliver, experiment, fail fast, learn, retry, and innovate. I appreciate this kind of company culture.


> I don’t mean to throw cliches around, but I was born in Eastern Europe, and in a sense, this career has been my American Dream.


The Metrics platform and the work being done on the Events platform—everything that would eventually become part of my portfolio—was powered by Kubernetes. Everyone was very forward-thinking. They knew we would have to be best-in-class, and I was really interested in taking on that challenge.


So the third thing that grabbed me was that opportunity to be a part of Datadog’s growth. I quickly bought into the vision. I thought, “This is a growing company with smart leadership, smart engineers, capable people, and great technology.” I wanted to be part of it.


## Which teams do you manage at Datadog, and what are they working on?


Currently, I lead the Distributed Data Systems organization, which comprises a portfolio of storage technologies. We own the Metrics platform, which powers everything in the metrics and timeseries realm, as well as the Events platform, which is responsible for semi-structured data like logs, profiles, and traces. Our portfolio also includes Driveline, a specialized main memory database optimized for online analytics workloads.


We also have a team called Cross-Platform Queries. Because these systems have historically evolved around proprietary, domain-specific APIs, this creates a steep learning curve for not only our engineers, but also our customers. The team built a cross-product query platform to implement a common interface which abstracts access to Datadog’s data systems and unifies the API.


We also operate Datadog’s Alerts platform, because it is probably responsible for more than 80 percent of the queries handled by the Metrics and Events platforms. Bringing alerts, metrics, and event queries all under the same umbrella creates possibilities for optimizations and a more symbiotic relationship among these platforms. And we oversee the Data and Analytics platform, because analytics is really what makes data valuable—it enables you to mine data and extract nuggets of knowledge. Data analytics rounds out our portfolio of technologies for data storage, querying, and ingestion.


Finally, we have a platform automation team, which caters more to availability, resilience, and those kinds of operational challenges, simply because all of these platforms feature significant complexity. Having a team within the org that knows how to run these systems in production makes a lot of sense.


> Bringing alerts, metrics, and event queries all under the same umbrella creates possibilities for optimizations and a more symbiotic relationship among these platforms.


## What is your day-to-day like?


It varies a lot. I spend a lot of time on both attracting and retaining talent, which means interviewing, making connections, helping us hire, and also conducting numerous one-on-ones with direct and skip-level reports on a daily basis. I do probably 20 to 25 of those per week. These allow me to stay in touch with the problems people are working on and keep a finger on the pulse of the organization.


Another big chunk of my time is spent on staying current with the technology. Datadog’s engineering managers are all highly technical, which allows us to establish a close rapport and working relationship with our engineers—we all speak the same language. We understand the problem space and what’s easy and what’s hard, what are the risks, technical pros and cons and trade-offs. By sustaining an awareness of the current technical challenges and development of the distributed data systems field, I can maintain context at a high level, so that when a problem or something flares up, I can zoom in relatively quickly and navigate an otherwise ambiguous situation.


Ivo works on electronics projects in his free time. This is a disco lighting controller he’s currently building. Ivo works on electronics projects in his free time. This is a disco lighting controller he’s currently building.


And I think the final part is longer-term thinking and contributions in the design and planning of our storage, search, and query execution platforms. I work with my organization alongside other engineering leaders to ensure that our big bets in this space are well-informed. These days, data storage is largely a commodity. But at Datadog’s scale (hundreds of PBs), various optimizations yield non-trivial cost savings. Similarly, search and query execution poses many challenges—data tiering, multi-tenancy, fair resource allocation, performance, and high availability requirements, to name a few—especially when these platforms are touching massive amounts of data. Putting it all together means that we need to innovate continuously and design and run best-in-class platforms. Nothing else will meet our customers’ expectations and support internal engineering needs.


## How are you working with product teams?


The role of the Distributed Data Systems team is to support product engineering, right? They in turn build the applications we need to delight our customers. So by definition, we need to operate in lockstep. When our product teams come up with a feature, they work with designers to figure out the UI and the user experience. But then sooner or later, that feature needs to be powered by the underlying platforms. By definition, my org interacts with both the product teams at the top of the stack and the infrastructure teams at the bottom, and we require tight collaboration to support them.


## How do you help your colleagues with their work?


When I meet with my engineers, I generally refrain from suggesting direct solutions, because they are a lot closer to the problem than me, and they are the real experts in their area. My job is to connect the dots. When I see duplicated work or two teams working on things that are probably related, I try to spark a conversation between those teams. I try to make sure that we don’t deviate from our overall direction. And in some cases, I try to be a sounding board for key design and implementation decisions where I can leverage my experience if it’s something I’ve seen or built before.


In general, I try to keep things on track. When teams become blocked, I step in and look at the problem from what I hope is an unbiased perspective. I try to be impartial when it comes to teams not arriving at a consensus, and facilitate their decision-making by asking the right questions rather than trying to solve the problem myself.


> Search and query execution poses many challenges—data tiering, multi-tenancy, fair resource allocation, performance, and high availability requirements, to name a few—especially when these platforms are touching massive amounts of data.


## Overall, what are the most important skills for your success as a VP?


I’ve always picked my roles so that I can maximize the value I bring to the table, while at the same time making sure there is still more for me to learn. And my position here at Datadog fits this bill. Datadog is a fast-growing company—when I joined three years ago, we had maybe 1,100 engineers, and now we have about three times that many. I think that the more we grow, the more the experience and expertise I’ve developed in my prior roles comes in handy. We have to maintain a fine balance, because we don’t want to rush and introduce unnecessary processes and procedures. But ultimately our job is to take Datadog from the two-plus-billion-dollar-revenue company we are today to become a three-, five-, and ten-billion-dollar-company. And that means the platforms have to scale with the business. So that’s where I add value—by helping us pick up the right strategy, the right technology, bring the right people, and execute towards that goal.


I think now the challenge for me is to amalgamate what I’ve learned over my career at growing companies with what Datadog actually is today and to strike a good balance, because you can only catalyze so much change before the substrate kind of saturates. If you swing the pendulum too much, you’re going to experience resistance. But if you don’t do it at all, then you kind of end up in the slow lane. I think that my natural inclination is to try to move fast. It works well for a smaller company, but as you grow, your codebase inflates and your systems become more complex, so you need to introduce new processes. With some more structure in place, you can disseminate knowledge better, and understand whether you are building the right things, building them well, and doing it at the right time.


## How would you describe the general engineering culture at Datadog?


We have so many smart people here who could absolutely hold their own at any other top-tier companies in our industry. At the same time, they’re very humble and down to earth—even the more senior ones, like our senior staff engineers. Our senior engineers are very scrappy and hands-on, and they don’t shy away from executing: writing code, creating designs, troubleshooting systems in production, and doing whatever it takes to help us advance our business model and build our infrastructure.


Datadog has a very no-frills, simple, and humble culture, where taking intelligent risks is absolutely accepted and encouraged with very little in the way. It’s a very grassroots, bottom-up culture with no unnecessary oversight from the top. That’s very different from other big companies where things start at the top and then trickle down. A lot of the engineering work at Datadog stems from the grassroots. Our engineers experiment, and when something seems promising, then we are very quick to create a product or platform out of it. There’s a high velocity of experimentation happening here.


---


Many thanks to Ivo for sitting down to share his experience and insights with us! If you’re interested in working with people like Ivo who are passionate about solving Datadog’s technical challenges and building out our backend platforms,[check out our Careers page](https://careers.datadoghq.com/engineering/) and join the pack!


-
-
-
