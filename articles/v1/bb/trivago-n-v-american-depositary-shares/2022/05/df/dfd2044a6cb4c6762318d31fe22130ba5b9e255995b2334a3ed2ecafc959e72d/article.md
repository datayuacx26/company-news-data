---
schema_version: "1.0.0"
document_id: "dfd2044a6cb4c6762318d31fe22130ba5b9e255995b2334a3ed2ecafc959e72d"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2022-05-16-warp-a-web-application-rewrite-project/"
published_at: "2022-05-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:10a50e8e17fd63297914d94331485f7662f68dd18a6aec3d7aecef25e617cbfd"
---

# WARP - A Web Application Rewrite Project

WARP - A Web Application Rewrite Project


From April 2020 until the end of 2021, we have put trivago’s web frontend on a new tech stack. Having moved away from a quite large PHP codebase and our home-grown JavaScript framework[Melody](https://melody.js.org/) , trivago now runs on a[Next.js](https://nextjs.org/) application, written in[TypeScript](https://www.typescriptlang.org/) .


*This is the first of a series of posts about our “big rewrite” project. It deals mainly with organisational challenges, not with technical ones. The[second post](https://tech.trivago.com/post/2022-08-01-three-learnings-switching-to-typescript/) is about our main learnings with TypeScript, and the[third post](https://tech.trivago.com/post/2023-02-22-technical-decision-making) is about decision making during the project.*


## Why


Our home-grown JavaScript framework had served us well. It made the trivago web application slimmer and faster. With it, our app ran smoothly even on weak Android smartphones, which had previously sometimes been overwhelmed by the amount of JavaScript code we shipped. Our engineers had got used to it and many enjoyed working with it.


However, it was also always a risk. There was only one, maximum two, core maintainers. With scarce engineering resources, development and maintenance of the framework itself was slow, and never a priority. Its documentation was incomplete. Developers, especially new ones, often struggled to find examples for certain things they needed to do. They could not simply go to Stackoverflow or do a web search for “How do you do X in Melody”. Also, some people were concerned that they were building up knowledge that they could not use elsewhere.


The most horrible thought of all: What if the most important core maintainer wins the lottery and leaves?


In short, we were in a situation where we had to decide: Do we double down on our framework development efforts, or do we change course? Do we commit significant engineering time and modernise the framework, continuously keep it up to date, provide training to potential maintainers, and create quality documentation?


After weighing all the aspects of the problem for several weeks, a tough decision was made: We would stop developing trivago on Melody, and do a re-write.


## How


The next questions were: If not Melody, then what? Do we need a framework at all?


In some hackathons and side projects, several trivago engineers had already experimented with other tech stacks. In particular, there was a lightweight web client called Feather, which was based on Svelte and greatly simplified the code, yet was remarkably feature-complete. Additionally, there was another prototype called Plume, based on Next.js, Preact, and TypeScript. Plume successfully demonstrated how the development flow was improved by the support that modern IDEs have for widely used technologies.


We also experimented with Ember.js and saw that it had a lot going for it. In the end, the superior market share of React and Next.js, as well as the good experiences with Plume, tipped the scale slightly towards Next.js with Preact.


With the question of the tech stack out of the way, the coding could begin, right? Well, kind of. Because React (and Preact) is rather a library than a framework, it gives a lot of freedom to the engineers. This can be good, because it means flexibility. However, it also means that you have a lot more decisions to make, especially when starting on a green field:


- Which libraries do we use (utilities, date calculation, etc.)?
- How do we organise our CSS?
- How do we maintain application state?
- How do we transmit events?
- Do we statically pre-generate HTML pages?
- What should our page and URL structure be?
- How do we do application initialisation?
- How do we design component APIs?


On some questions, there was quick agreement: We would use the[functional way of defining components](https://reactjs.org/docs/components-and-props.html#function-and-class-components) , and we would use a new URL structure. However, on many other issues, there were different opinions, so discussions and debate were necessary.


## Making decisions


With such a lot of decisions to make, and engineers needing to find alignment on a lot of things, there is real danger of slowing down and postponing decisions.


Therefore, we designed a[pragmatic approach](https://tech.trivago.com/post/2023-02-22-technical-decision-making) that made sure all voices were heard, but also that decisions were reached in a timely manner. Its main points are:


- A decision document where all the relevant facts and viewpoints are collected and organised.
- A decision owner who curates the decision document, prepares the decision meeting, and is responsible that a decision is reached.
- A decision meeting, where viewpoints are exchanged and discussed, and a decision is made at the end.


With this process in place, we made a lot of decisions in a rather short time. Not all of them stood the test of time.


For example, we designed and implemented an application initialisation process which had some advantages like automatic dependency resolution among pieces of initialisation logic. However, the process felt too complicated for many developers, so we changed course and went with a more standard Next.js and React approach.


Even if you make a decision with your best knowledge and intentions at that time, an actual implementation of your ideas might bring new insights. The important thing is to get commitment on your decisions, but to also keep an open mind so you can course correct if necessary.


## Remote collaboration


Needless to say by now, with the project starting in the spring of 2020, all of this happened in a remote-only fashion. While it feels like remote has become the new normal, it does pose a challenge to a greenfield project with tons of discussions to have, decisions to make, and ideas to develop. The bandwidth of video conferencing is lower than with true in-person interaction, and it is sometimes harder to make sure really everyone voices their opinion.


However, during video chats, we also had a lot of laughs with people in different timezones brushing their teeth during a long mob coding session, or with fun backgrounds and Snap Camera lenses.


## Managing the team size


When you develop something on your own, and you change your mind, you can simply delete half the code, and rewrite it in a different way. With a team of two or three, this is probably also still possible.


However, when you go beyond that, the cost of changing your mind goes up rapidly.


This is important to keep in mind during the early stages of such a project, where things can change very quickly as you write experimental code and gather new information. Doing this with a team of more than five or so people can lead to a lot of communication overhead, wasted effort, and frustration. Part of the team will be on hold while another part is experimenting.


On the other hand, at some point, you need to onboard more engineers to the new code base, to get the work done, and all the features ported over. Ultimately, the number of web developers on the project would grow to 30. Hitting the sweet spot here – keeping the team small until the code is stable enough, but then growing the team to pick up speed – is highly challenging.


Frankly, we did not hit this sweet spot. We grew the team slightly too fast at the beginning, with some important decisions still pending. This led to a lack of clarity, and is one of the most important learnings we took away from this project.


## Catching up with the predecessor


Once the core functionality is there, and your re-written application is in a state that is already useful to the user, it is time to expose it to the real world. Being a data-driven organisation, of course we wanted to produce hard numbers that would give us a clear picture how the new application compares to the old one.


So we set up a series of dashboards, checks, and comparisons that served as guides where to focus our attention next.


Key topics of the various dashboards and comparisons were:


- **User interaction:** Did users interact with the new product in the same way as before? Why did they not open certain panels as often as usual? Is there a bug in the UI, or is it simply a logging problem?
- **Revenue:** With the new application, do we forward users to booking sites as often as before?
- **Types of searches:** On trivago, we sometimes adjust the search parameters automatically to give the users better results. For example, if the user’s destination is a small village that has only very few or even zero available accommodations, we automatically switch to a perimeter search to list accommodations nearby, and we consider their distance in the result sorting (see screenshot). This kind of automatic adjustment leads to characteristic ratios of search types. When the ratio of search types in the new product differed strongly from that in the old product, this was a sign that some parameters were not set correctly, and a reason to investigate.


## The finish line


After several months of comparing the new application to its predecessor along several KPIs, we were finally able to flip the switch: All user traffic now went to the new application. How did we feel? Relieved, mainly. There had been many difficulties and setbacks on the way, but now we could reap the reward.


### User benefits


One of our motivations was to make our web application run smoothly, also on weaker hardware. The slowest mobile clients we have at the time of this writing are, arguably, Android 6 devices, which make up around 0.5% of all Android clients. Our tests, both automated and manual, show that the user interface is fluent to use.


Fluent UIs are user friendly, and so is a fast application startup time. This startup time depends heavily on the size of the code we ship to clients. Since we rely heavily on open-source libraries (and also give back to open source) like Next.js, Preact, react-use, etc., we have to watch our code size, so let us take a look at that.


Admittedly, it’s not quite fair to compare the code size of a brand new application to one that is several years old. However, the amount of code that is shipped to the client has a large effect on the performance of the application, so we should at least take a brief look.


With the new product, we reduced the page weight from 2.1 MB to 1.7 MB for the home page, and from 4.1 MB to 2.6 MB for result pages. This is a reduction of 19% and 37%, respectively. Breaking up our single-page application into multiple pages and making use of the automatic code splitting feature provided by Next.js have been very beneficial at this point.


### Developer benefits


Apart from our end users, our developers also benefit from the rewrite. The code base is in a cleaner state than before, and better documented. This makes feature development faster. Because a lot of people had been there “from the start”, they were also much more motivated to write documentation and explain design decisions to their colleagues.


Most new developers joining trivago feel instantly at home, with a relatively standard setup based on widely used technologies. Therefore, they are productive more or less from day one, and don’t have to learn many new technical concepts.


Because React has a giant ecosystem, most of the common problems you encounter have already been solved. These solutions are available as open-source libraries like` react-use` , so you can easily pull new functionality into your project.


While no definite proof of faster development, the numbers of merged pull requests per month are higher in the new code base than they ever were in the old one (see chart). With roughly the same people working on both, and following the same process, this does indicate more activity on the new code base. Through continuous integration, it is now common to have 10 releases a day, instead of one or two. This way, it is faster for a developer to put something live, which gives this extra motivational boost.


After some more cleanup, we will be able to switch off lots of legacy systems that had been troubling us for a long time. This frees up mental (and machine) resources that can then be used more creatively.


## So all is well, now?


Despite all the benefits, we should not forget that a “big rewrite” comes at a cost. Development of new features is practically stalled for a long time, which can hurt the business. Additionally, the new product was missing some optimisations and tweaks which, initially, resulted in lower revenue. Ironically, the pandemic with its reduced travel activity helped us in this case: Revenue was lower than usual anyway, so accepting some revenue loss during the development of a new product did not hurt as much.


Apart from the business, the development teams are also affected by putting feature development on hold. At some point, all features are ported to the new application, and teams want to move on. However, we had to make sure that everything was working as desired, and that the numbers were fine. This took time, during which teams could keep working on new things, but not send anything live in order not to hurt comparability between the old application and the new one. If this state carries on for too long, it can hurt motivation.


Once the freeze was over and the new application was officially accepted, however, new features and improvements were added to the code base at a fast pace. There was excitement about discovering new ways of doing things with the new tech stack, and of course some uncertainty: “In the old app, we were able to do X this way. How can we do this in the new app?”


This feeling of being a novice again in some respect is uncomfortable - but growth typically does not happen in your comfort zone.


I think it’s safe to say that every person who worked on the project learned a tremendous amount - some about project management, some about communication, some about technical topics. It is a great feeling when you see many people learning and growing together.


And that is our main takeaway.
