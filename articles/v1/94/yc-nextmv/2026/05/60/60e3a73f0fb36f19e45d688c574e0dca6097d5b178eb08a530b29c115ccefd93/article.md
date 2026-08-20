---
schema_version: "1.0.0"
document_id: "60e3a73f0fb36f19e45d688c574e0dca6097d5b178eb08a530b29c115ccefd93"
company_key: "yc-nextmv"
company: "Nextmv"
source_id: "yc-nextmv-news-import-d2226782a65f"
canonical_url: "https://www.nextmv.io/blog/in-conversation-the-software-or-interface"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-22T06:03:40.021254+00:00"
fetched_at: "2026-07-28T22:12:48.521638+00:00"
content_hash: "sha256:e51524a65fe3bdfed07c51374348abe82ca7877474a34361ad1cbcae3d245056"
---

# In conversation: The software-OR interface

What does it take to successfully bridge the gap between “I have built a model” and “I have deployed a model into a live business process” while working on decision science projects? Nextmv CTO and co-founder, Ryan O’Neil, hosted a conversation with Ryan Marcus, Lead Data Scientist at Carvana and Chris Pryer, Principal Engineer at NFI, that explores the software-OR interface through industry experience and observations.


*The following is a companion interview to*[a longer, related conversation](https://www.nextmv.io/videos/the-path-to-production-exploring-the-software-interface-with-or) *that has been edited for length and clarity.* **‍**


‍


‍ **Ryan O'Neil:** Ryan Marcus, you are an operations researcher/decision scientist by training. Chris, you're a software engineer by training. We're talking today about the melding of these things into decision support that's integrated into software stacks. Can you talk about your original training and early experiences with the “other side”, so to speak?


**Ryan Marcus:** I studied industrial engineering, so I definitely had some classes in operations research at school. I took one computer programming class in school. It was a C++ class, and I did not like it. I then came to J.B. Hunt and I asked about their vehicle routing algorithms. That, combined with technical interviews, got me placed on more of the coding optimization team. I learned the software side on the job.


I was put on a project where we were doing real-time assignments of resources to tasks, and it was a lot of debugging. We'd get a lot of questions on, “Why is it assigning this? Or, why is it not assigning this?” I did a lot of that debugging, figuring out if there are parameters that need to be changed, where the data came from, and what other systems is this optimization interacting with. I learned a lot on the job from that. ****


**Ryan O:** Chris, how did you gain all your digital twin optimization experience? Was that mostly learned on the job?


**Chris Pryer:** I started as an intern actually, and the team that I was joining was this brand new supply chain design team. We were immediately thrown into the thick of it, just right out of the gate studying our customers supply chains and trying to find optimizations and improvements we can make to their supply chains.


In college, I was deep into the software world, going from hackathon to hackathon and project to project. I was studying with a bunch of my fellow students in college, we were a big part of the ACM organization, and I remember going into this supply chain role in this decision science world and seeing some of the network studies that we would do. It stood out to me that these are systems in and of themselves. And from there, I was pretty much attached. I went from one problem to another and fell in love with the domain.


**Ryan O:** Falling in love is a good term for it. That definitely happened to me. I remember during my first class on deterministic models, I walked out thinking this is what I'm gonna do. This is it.


**Ryan M:** I had that same exact experience because I did civil engineering for two years. Then I switched to industrial and when I had that deterministic optimization class I knew right away. This is where I should be.


**Ryan O:** It's probably good it wasn't stochastic because I would have been a little terrified and confused. Ryan M, one of the things that you mentioned that really grabbed me was debugging assignments and trying to explain counterfactuals to people. “Why did it do this? Could you dive a little bit more into that?” We did a lot of that at Zoomer and GrubHub, and we'd work with the ops people a lot. It was very challenging. I'm curious what your experiences were with that.


**Ryan M:** That optimization program I mentioned, I would classify it as an opt-in system where we were providing recommendations, but the planners had to go in, go to a specific page to view the recommendations, and then click a button to accept them, versus a system where it's just default system plans and they would have to override it.


A lot of that job was selling the optimization to the planners because they didn't have to use it. They might have some pressure to use it more. We were doing a lot of stat collection of who's using it and how much. It was a combination of the sales role with the engineering, with the operations research. I would actually travel to different locations because a lot of them were on site, not at corporate. It was sitting with the planners, seeing how they thought, seeing how they made tradeoffs in their mind, and thinking, “What if this, what if that?” A lot of the things that they would plan would be a local optimal, if they were just looking at one resource in one task, whereas the optimizations were doing a little more of a global optimum. That was definitely a challenge because that requires a lot of trust from the planner to know I could make this one assignment better, but I'm trusting the system that it's going to make other things better if I do it one way instead of another.


**Ryan O:** Sitting with the ops person really is the key to the job.


**Ryan M:** I was forced into it on that one, and I'm glad I was because I learned that I enjoyed it and I learned how important it was. If I had been thrown into a different role, I might not have had those experiences.


**Ryan O:** Chris, you've done very similar things working with some of the users early on.


**Chris:** I love that you brought this up. That is a big part of what we've done. A big part of this is establishing that confidence and trust, and that can be complicated. I’m curious how you manage that from project to project? Did you interact with multiple teams and have different styles or approaches to manage that confidence?


**Ryan M:** Definitely different teams for different projects. Psychologically, there's the math and the software and the assignments, and then there's also the person in front of you and who they are. Part of it is just building that relationship, letting them know that I'm a person, just like they are. I'm not just sitting on a computer screen coding things and forcing them to do what the system wants them to do. Building that trust, asking them a lot of questions so they know I'm curious and interested.


I would also tell them, the system we're building is your system. When you find things that we're doing incorrectly, we can put your logic into the code and now it's a living, breathing thing that you helped build. That was helpful as well.


**Ryan O:** This is almost like how you deploy the model itself with the operations people. There's also the aspect of how we get these models into production environments.


Chris, you mentioned that early on you were doing a lot of analyst style work, one-off kind of studies, and now you're working on digital twin simulation. How do you put these things into production environments? What components do they have that are the same? How do they differ?


**Chris:** The first thing that comes to mind is actually using an example of where it didn't work out, where we had a lot of challenges or struggles, and actually embedding that in a process that existed. A lot of it is analyst workflow. I look at that as capabilities that other people want to be able to tap into. We had a very specific team who wanted to leverage a driver scheduling program that we were working on as part of a larger study for a customer of ours. We were trying to figure out how to ship that for that team and make it available for them. It’s about having the ability to not only spin up the different resources available and make them compatible with the procedures and logic in place around data security to make this a live business process. It's also about understanding what resources do you have available to you and how you plan to use them and make them compatible with other systems and processes.


**Ryan O:** What challenges are there? What makes it hard to tie optimization into software?


**Chris:** One, there isn't a clear interface. So if we're talking about a vehicle routing solver, for example, you don't have an API that is exposing that to other systems and making it available. You don't have underlying resources making that scalable infrastructure either.


**Ryan O:** Ryan M, have you ever been in this situation where an optimization model was under-resourced for the task that it needed to do and it wasn't able to accomplish it?


**Ryan M:** Yes, and it can be too big of a problem for an optimization solver to solve, so you have to shift to heuristics. The amount of data when you're provisioning and deploying, maybe it's consuming more memory or more CPU than you allocated. Usually that's a quick fix to increase the resources, but then you might bump up against limits that are set in place and you might have to go back into the code and try to make things more efficient.


In terms of what Chris was saying earlier, about building UIs and APIs that expose and visualize things, often you start with a model that's outputting to a spreadsheet because that's what existed before, but you really need to build out something that's more robust and more scalable. People like using things that are visual, so that's a big component of building something that they can see and play around with.


**Chris:** If you think about the driver scheduling problems, what we were building was for a dedicated fleet team, and that team has maybe 10 or 15 engineers on it that would use that. The underlying component, if we were to make that available for more people, is that they could have different problem structures themselves, which has an impact on how you want to use the underlying resources too. Being able to provision machines, flexible for those resource requirements, is super important. If you're making this available for teams with many shifts trying to assign versus another team with a few shifts, that problem looks different.


**Ryan O:** One of the interesting conversations I've had with people when we were with SRE types or software architecture types, who were onboarding optimization technology into a platform, was about scaling and the unpredictable nature of optimization. When you talk to the software people, they want to know how many inputs and does it scale up. I can't really tell them that because I can give it very small problems that, in certain circumstances, the optimization won't be able to solve in the time that you want. I can also give it very large problems that will. It's just not a clean predict response curve. It can't necessarily be divided up horizontally either.


**Chris:** It's a very observation driven workflow. It requires iteration. It is unique to this industry or to this area or specialty. Having a robust capability to be able to iterate and observe changes and test. An experiment makes these projects more scalable in nature in how you build it.


**Ryan O:** Let's talk about the other stakeholders of these optimization software systems. At Nextmv, we think of this optimization infrastructure in terms of what we call DecisionOps. That's basically the glue for getting and keeping models live, keeping them happy and healthy. Can you describe DecisionOps as a concept and practice to stakeholders and peers in your organizations and network?


**Ryan M:** I have two answers, depending on who the person is. If they’re in data science and machine learning, the simple, easy cop-out answer is it's MLOps, but for optimization. Similarly, for a software engineer, it's DevOps for optimization. The added complexity is A/B testing, for example. In the software world, you could just say, “Do one treatment for this customer, and one treatment for that customer. Do that a bunch of times and do statistical significance.”


In the OR world, what we do for one customer affects the other customer. It's not as isolated and independent, and that leads to more sophisticated methods that you need to use in DecisionOps. For someone that might be non-technical or a business stakeholder, I would use the metaphor: We're building you a house and DecisionOps is the plumbing, the electrical wires, and everything under the hood that you don't see that keeps it in perfect shape. It alerts you when things are off a little bit. It allows you to test that improvements are going to perform well before or during your production rollout. It’s the under-the-hood stuff that takes time, takes thought, takes strategy, but that wouldn't be directly exposed to in the business.


**Ryan O:** Chris, you've had a lot of conversations at NFI about this sort of thing. How do you talk about DecisionOps there?


**Chris:** Coming to this panel, I was thinking about it in the same exact way. MLOps for optimization. It's the specialization that is optimization, and how that applies to DevOps. I think that was a great way to put it.


**Ryan O:** One of the interesting things about optimization, in particular, is that there are a lot of problems you can solve with it. Whereas, if you're looking at things from the machine learning lens, there are a lot of techniques. The output of those techniques are usually regression classification or a few other things. There’s a small number of final applications, so you're applying those models in different ways, but the models themselves, the basic structure of them, is somewhat uniform. There's just not that many categories. When you go into optimization modeling though, the sky is the limit. It adds so much more complexity to the operational aspect of this.


If you all could go back in time to your earlier career selves and give a couple pieces of advice related to the software engineering side and deploying models, what do you think those things would be?


**Chris:** If I look at intern me, what kind of advice can I give myself? The first thing that comes to mind is, time to value. It’s a really important metric to use as a rubric or guideline. Let's say we’re doing a vehicle routing study, and looking for enhancements, improvements, or inefficiencies with existing plans. We’re trying to improve them and explain what those improvements are. Understanding what that value is, what that means to a customer or to an internal operator, and being able to bring that value as quickly as possible to those end users, is really powerful. It’s an incredibly important skill.


Another piece of advice I'd probably give is, don't be afraid to or embrace failure. Sometimes you're going to try something, it's not going to work out, and it's crucial to embrace that as a learning opportunity and a process for improving. People like to call it, “fail fast." That's how I like to think of it.


**Ryan O:** You have to develop tenacity, thick skin, and be willing to just walk away from something that you think is beautiful and will work beautifully, but it doesn't.


I worked with Karla Hoffman as my advisor. And she would always tell us, the first thing you want to do when taking on a new project is implement the simplest model, give them the output that they're expecting, and show that you understand the problem. Do that as quickly as possible to get stakeholder buy-in, then go from there. Don't go off and build some complex thing that accounts for everything and takes six months.


**Ryan M:** Building on the time to value advice, that's one of the things that might separate academia from industry. It's often, go with the simplest thing that works, that you can build quickly. I've found that means there are a lot of things on the software side to fix before you go deep on the optimization algorithms. I've gravitated in that direction sometimes because I can just identify a lot of quick things on the software side that make things better.


On the point of failing fast, you learn the most through failure, especially in the optimization world. For stakeholders and planners, it's much easier for them to tell you what’s wrong than what’s right. Getting something in front of them, where it's tangibly wrong, leads to better feedback. Failure can be a good thing, and part of that is just communicating about the prototype. We're going to build and iterate. You're going to help us identify what's wrong. and then we’ll be iterating quickly to show that you're making it better. Build that trust.


With respect to earlier in my career, I wish I had put more focus on testing. Building and writing tests that last. When I would make changes, I would test it, but it was more of a manual test of running the code, verifying what I expected it to do. I didn't leave behind a lot of regression in unit testing, so that when the next person came along, those tests would be a safeguard to make sure that things didn't break in the future. Later in my career, I learned all sorts of other integration and end-to-end testing. That really makes for a more robust system.


*Check out*[the techtalk recording for the full interview](https://www.nextmv.io/videos/the-path-to-production-exploring-the-software-interface-with-or) *on the factors to consider when integrating a decision model into a live business process.*
