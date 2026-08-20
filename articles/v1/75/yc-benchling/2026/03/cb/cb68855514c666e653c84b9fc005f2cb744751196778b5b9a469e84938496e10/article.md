---
schema_version: "1.0.0"
document_id: "cb68855514c666e653c84b9fc005f2cb744751196778b5b9a469e84938496e10"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/moderna-and-iff-on-the-reality-of-ai-readiness"
published_at: "2026-03-04T08:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:e1446842df6f20582f39146ba1a49efab46598e08bcd7ac48d75617e0932e6cb"
---

# Are we missing the boat? Moderna and IFF on the reality of AI readiness

Four months. That's how long it took IFF to redesign a protein, scale manufacturing, and deliver commercial quantities to one of their biggest customers.


Two decades ago? That same work would have taken two years.


Benchling's Robyn Langevin sat down with Al Park, Senior Director of Digital R&D at Moderna, and Casper Vroemen, Chief R&D Officer at International Flavors & Fragrances (IFF), to discuss what it really takes to prepare scientific data for AI. The conversation cut to the actual work of standardizing workflows, unifying systems, and convincing scientists to stop walking around with USB sticks.


Between them, they oversee R&D operations spanning over a billion COVID vaccines to the enzymes in your laundry detergent. Their perspective — and one echoed across the industry in our[2026 State of Biotech AI Report](https://www.benchling.com/biotech-ai-report-2026) — AI tools are powerful, but they're only as good as the data beneath them. And most organizations aren't as ready as they think.


** Editor's note: The conversation has been edited for length and clarity.*


## The paths that led them here


**Robyn Langevin: Before we dive into the technical aspects of being AI-ready, I'd love for people to get to know you both. Vroemen, can you walk us through your journey into the role you have today?**


**Vroemen** : I started as a graduate student at Wageningen University in the Netherlands — a biotech food science university — but I wanted to do something else. I saw a role at a smaller biotech firm called Genencor in Leiden. They were a spin-out of Genentech applying protein engineering to non-pharmaceutical applications, such as laundry enzymes. I ended up joining as a postdoc on a one-year contract and got interested, not only in the science, but also in the fact that we were creating real-world proteins and products that people use in their daily lives.


Soon enough, I was leading the Leiden R&D team, which was a small team of thirty people more or less. When we were acquired by IFF out of Dupont, I led the integration between IFF and DuPont Nutrition and Biosciences to form a global R&D organization. Then I was asked to lead all of R&D for IFF, which I currently do, focusing primarily on biotechnology research.


**Park** : I've worked in the biotech industry for over two and a half decades now. I started my career at the Boston University Graduate School for Bioinformatics — that's where I cut my teeth.


If you're as old as me or older, you've heard of Millennium Pharmaceuticals. I worked there a few years then moved to a phage display antibody company called Dyax Corp in Cambridge.


After essentially automating myself out of a job there, I went out to the West Coast, got recruited to work for a small startup biotech focused on oncology and cancer stem cells called Oncomed Pharmaceuticals. I worked there for several years before I left with two of my colleagues to start a biotech company called Stemcentrx in mid-2008.


We grew that company from the three of us to about 450 employees, at which time we got acquired by AbbVie. So I lived in the Bay Area for about 10 years before moving back to the East Coast in mid-2016. I continued to work for AbbVie for another three years as Chief Operating Officer for Bioinformatics in their Genomics Research Center.


I left at the end of 2019, ended up doing some consulting work, tried to start my own software company, COVID hit, and then my wife decided I should find a job on the East Coast. So that’s how I ended up at Moderna in early 2022 and have been there since.


## Exciting AI breakthroughs to watch


**Langevin: Getting into the realm of AI, what is one breakthrough that you’ve seen recently that you’re excited about?**


**Park:** What's interesting right now is the ability to build AI agentic workflows by daisy-chaining concepts together and seeing whether you can get something useful out of that. But all of that starts with — do you have data in a clean and structured way?


**Vroemen:** We apply AI in various disciplines; one exciting area is protein engineering. 80% to 90% of the proteins we sell are protein-engineered. For a long time, we had scientists saying, "I'm better than AI, my mutations are better." But the last product we developed for a large detergent company — half the mutations were primarily developed by AI.


It's still only half, so there's a long way to go but I'm with Albert here. The availability of highly structured, high-quality data is holding us back from massively introducing AI. While half of our mutations are now developed by AI, soon we'll be higher.


## No more USB sticks — building data infrastructure from the ground up


**Robyn: When you're thinking about data infrastructure needed for AI at scale, what was already there and what did you realize you'd have to build or invest in to achieve your AI objectives?**


**Vroemen** : We have a facility in Leiden that is purely focused on driving automation and high throughput technology.


We started our Benchling journey quite early as purely a workflow management tool. For example, when we needed to do primer design, order primers, get them into 96-well plates, and do micro fermentation. We didn't even have barcodes in the beginning.


So we really looked at Benchling at that time as a workflow management tool that helped us automate all of those workflows from molecular biology to protein engineering and data generation.


Only more recently have we further integrated Benchling in our operations as an electronic lab notebook (ELN). We had a different electronic lab notebook before but we now said, hey, let's bring everything under one umbrella. It was then that a lot of the workflow automation tools we already had started benefiting from the superimposition of AI.


##### "I think data quality, standardization and formatting across all of my twelve R&D facilities around the world is probably still the biggest challenge for AI to be really productive."


**Langevin: And Casper, if I could ask a follow-up, what influenced your decision to unify and bring all of these labs onto Benchling?**


**Vroemen:** The primary observation was that the ELN system we were using wasn't really developing very well. We became less satisfied with it because it wasn't very flexible.


People were literally walking with USB sticks from a computer that was linked to a robot or to a spectrophotometer into the lab, because we couldn't unify our lab IT environment with our office IT environment due to firewalls and machine incompatibility.


That led us to say, if we want to win in this space and do the 10x speeds that we're seeing, we need to optimize. Let's now unify everything, remove the barriers, and really get to 10x speed.


**Langevin: Al, can you speak to some of the work you're doing with Benchling today to help ensure that you have data in a structured and trusted way at Moderna?**


**Park** : Moderna started using the Benchling ecosystem in 2022, just prior to my arrival. Due to the speed and pace at which Moderna was growing at the time, there was a lot of structured data but it was a bit disjointed in terms of strategic implementation.


So a reassessment was done to identify whether there were products on the market that could unify the information being generated by the research and technical development organizations to leverage the data to gain insights.


Currently, there’s a pretty massive effort to migrate data from internal applications, where structured data exists, into Benchling as we're configuring Benchling for our use cases today.


**Langevin: There are really strong scientific use cases like the ability to streamline processes within technical development, and the productivity tooling that's super helpful for scientists. Al, can you talk through one of the use cases that you're interested in exploring today?**


**Park** : Our CEO, Stéphane Bancel, has talked in the public domain about Moderna trying to leverage AI wherever possible. I would say that Moderna is one of the leaders out there in this space in terms of leveraging AI adoption, just purely from a personal productivity perspective. We're leaning hard into leveraging internal deployments with our collaboration with OpenAI and customized Chat GPTs. But that's only addressing one aspect of where AI could be useful.


I think Casper also touched on similar efforts happening within IFF such as research-specific, ML-based approaches for AI. **


##### "In the context of Benchling, it’s really an effort to get unified, structured data on a platform that allows us to be AI-ready and layer AI enablement on top of that structured data. "


## Compressing timelines from two years to four months


**Langevin: With AI being so new, how are we assessing that we are making the right choices and tracking toward our objectives? Casper, when you're looking across your organization, what are you looking at as a metric to say you are on the right track?**


**Vroemen** : There's many metrics, of course, that we measure ourselves against and that you could apply to see what the impact of AI and Benchling is.


First of all, the speed to market. We need to get faster and faster in our design-build-test-learn (DBTL) cycles. We had a recent example where one of our biggest customers decided to change the format of their product that is to be launched in early 2026, making our enzyme incompatible with that new format.


So we had four months in total to redesign the protein, put it into a production strain, and scale up the production process to make sure we could deliver them commercial quantities of product.


And we did that.


##### "This is four months from almost scratch to scale up in manufacturing plants that in the even two decades ago would have taken us at least two years."


So that cycle time is highly enabled by new machine learning that optimizes their workflows.


**Langevin: Where in that cycle time do you feel like is the easiest to go in and add that automated layer?**


**Vroemen:** The biggest impact is in the protein design phase. We don't transfer any manufacturing process from my pilot plant in California to a full-scale plant without first modeling it with AI. But I want to say that in the process development phase, I don't really see the acceleration yet to the same extent as the research side. That's an opportunity for us to become even faster by automating that more as well.


**Langevin: Anything else to add when you're thinking about how to measure the ROI for the AI initiatives you're leading?**


**Park** : For Moderna, it's all three in order: quality first — quality of data, approach, and tool. Then speed — biotech is competitive, so speed to market is critical. Moderna, as a platform company, is leaning into that technology to produce therapeutics at scale quickly.


But then ROI — does it make sense for us to invest internally?


##### "Sometimes we're building our own internal AI products, versus evaluating products from the commercial software industry, sometimes in direct competition."


We weigh whether it makes sense to build something internally versus adopting something available commercially.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Understanding the human element in adoption


**Langevin: One big component of being AI-ready is not just the foundation of the data, but it's also making sure you have the change management and the processes in place to make sure that folks use what you roll out.**


**Thinking of that human element, how has AI changed the projects that you've led or how you've structured your teams?**


**Park:** At Moderna, product managers are very important for making sure that we can have good onboarding and train users into the configured systems that we deploy.


That change management is always difficult regardless of what company you're a part of because people have their preferences and lean into customization over generalized solutions.


But that's where Benchling shines with its flexibility. Many software systems don't have that, and it helps with training and messaging for change management.


**Vroemen** : If I go back to the first time we introduced an ELN in our company, we literally had to police our workforce to use it. I got a list every month — you had one entry, you had six entries — and we'd ask why. It sounds silly, but some change you have to drive from the top.


##### "With AI and Benchling, it's different now. There's a lot of excitement because we've shown internally what these tools can do to impact people's day-to-day lives, and people are excited about that progress."


In that sense, I foresee a lot less challenges for a broad rollout of Benchling because people already see that it improves their day to day.


**Langevin: When you're thinking about helping to support that excitement being generated by your scientists, how do you continue to fuel that momentum and ensure that they feel empowered to continue to learn on their own?**


**Vroemen** : AI is still new for many of us. We're implementing it at every level, from the CEO down.


I still believe you need to drive that from the top and encourage people to take the time to learn and start applying it in their day to day job.


##### "Without CEO buy-in, you don't drive transformation broad enough to have impact quickly."


On the other hand, people in R&D are naturally curious to learn about new fancy stuff. We create sandboxes where people can start experimenting with new tools on their own without immediately affecting the mainframe.


Of course, the good stuff that comes out of the sandboxes, we will then later integrate into the mainframe.


## Empowering scientists to experiment with AI


**Langevin: Say I'm a scientist that’s come up with a great AI idea. What would you be looking for as part of that conversation to say yes?**


**Vroemen** : We've appointed Benchling and AI champions in the organization. When someone comes up with something, the first conduit they go to is the network of AI champions. When the champions are convinced it's worth prime time, they convince us. Who am I to say no?


**Park:** That's interesting. It’s very parallel in concept to what happens at Moderna. A lot of scientists have been empowered to try and leverage as many of the available AI tools as possible.


One area where we've enabled governance is cybersecurity and permission-based access. And so, as Casper mentioned, having a sandbox for scientists to leverage and generate something that amounts to a prototype.


And then I'm going to flip on my engineering hat.


##### "A scientist might say, this works for my use case, it could be useful for others — should we productionize it? But now it becomes a roll up into the digital organization to evaluate the intended use case."


Do we broaden its scope? And in broadening its scope, does it hit the realm of privacy, permission based access, cybersecurity related concerns? How do we take that and engineer a more production ready system that can be deployed for the whole organization? So there's a governance process to something like that.


**Langevin: When you're thinking about how you’re defining your global R&D strategy and layering on your AI initiatives, how are you prioritizing what components to invest in as part of the AI journey?**


**Vroemen:** We always try to estimate what the short term and long term impact will be of a certain use case of AI.


Let me mention one example: When we mass produce proteins, every run makes 20,000 kilos of proteins that need to be top notch in low cost protein production. What goes into these fermenters is highly engineered strains that we've been working on for forty years that are unmatched by anyone else.


If we work with a partner that claims that by studying our systems they could optimize them using AI, then the math easily works out. If you can cut even 10% of 20% production cost at that scale — that is massive.


##### "I worked in that area as a scientist and thought, we've been at this for forty years, AI can't move the needle. I was wrong."


When we use AI, we still see very significant increases that we haven’t seen as scientists yet. It’s almost an easy business case to justify. lf I can win 10% here, then I'm going to have millions of dollars of impact. So the impact factor and the likelihood of success are the two criteria that I basically use to prioritize one project over the other.


**Langevin** : **I like the way that you put that. With AI and deployment of AI, we are testing hypotheses. Sometimes we assume something is going to work and it doesn't or we think that there's not an opportunity for AI to add impact and it will. We're on a big learning curve together, and there will be surprising learnings along the way.**


**To go off that, Al, has there been anything in your journey leading some of the AI initiatives at Moderna — areas that you've seen success that you might not have expected?**


**Park:** We’ve seen obvious ones like operational and productivity gains at both the individual scientist level and certain processes.


However, for as much as Moderna is a therapeutics and drug producing company, it’s also focused heavily on research. One cutting-edge area of research is applying known algorithms into the quantum computing space, which is what Moderna has made an investment on. The quantum space and hardware is still early on, but what's interesting is that existing algorithms can be reimplemented on quantum computers — generating similar results much faster.


Is it ready for prime time? Probably not yet, but investments are being made for the future to make sure that Moderna is ready.


## Building for the future and staying honest about it


**Langevin: When you're thinking about what's next with AI, how are you ensuring you're staying up to date on all of the enhancements coming out of the field?**


**Vroemen:** I would love to give a different answer, but I don't think we're that far ahead. Things are moving so fast. I personally have a healthy level of anxiety. We're well positioned. We have applied it with some early good success but we're trying to stay up to speed with all what's happening in the AI world and picking out where we want to play. **


##### "On a perfectly honest note, I don't think we're thinking ten years out. We are just trying to do the right thing now to not miss the boat."


**Park:** It's hugely challenging. Everyone knows how fast AI technology is moving. All the commercial software vendors are trying to incorporate AI workflows or AI-enabled LLMs into their products. It's hard to stay on top of everything, to be honest.


##### "Moderna tries to leverage its whole employee base to understand from their perspective what is happening out there. Then aggregating that information to see whether there's a clear-cut leader or if we need to address it as an enterprise-based solution."


**Langevin: When you are thinking about whether your team is prepared and ready to roll out and deploy the AI changes, how are you looking to ensure that your team is thinking about it in the right way?**


**Vroemen:** We've educated our employee base at three different levels in a sort of AI Academy. For example, at the leadership level, you need familiarity with different AI concepts but if you're like in the lab and a coder, you get the highest level. That way everyone is educated.


At the IFF executive level, we spent two full days with an AI guru to educate and shift our mindset. We learned the CRIT algorithm — context, role, interview, task — to work with agentic agents to make our lives easier. We've started educational programs to prime employees for AI.


We've embedded a digital biology leader in my leadership team and an overall digital leader in the business leadership team. So we've embedded leaders at multiple levels to make sure we drive this from the top.


**Langevin: With that fluency, you're going to be able to make better and faster decisions of what you're choosing to invest in and where.**


**Thank you, Al. Thank you, Casper. I know on the Benchling side, we're excited to partner with you on your AI journeys.**


## Ready to unify your R&D data?


[Download our free AI guide](https://www.benchling.com/resources/getting-started-biotech-ai-guide) with 6 use cases, example prompts, and expert tips to jumpstart your AI journey in biotech R&D


Want more insights on how hundreds of biotech leaders are using AI?[Read the 2026 State of AI Report](https://www.benchling.com/biotech-ai-report-2026)
