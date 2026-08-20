---
schema_version: "1.0.0"
document_id: "0b661826e7c5386fd51cc473269ee47f9b98ca162e143ebc7e6d38c4322aeea9"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2015-08-31-culture_of_quality/"
published_at: "2015-08-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:37.879348+00:00"
content_hash: "sha256:0db8c123dae718e1c7919fd04252408fd59ba95ce45d37a535c1aa6248f90f77"
---

# Creating a Culture of Quality

You know those bugs, like, **those** . Where the application state dances around you like a crazed Polynesian fire dancer. Where changing the sorting order of a search in London reverts the result list back to Paris… Seriously? Unfortunately, a lot of us are specialists in dealing with this kind of bug.


I know I am, I remember saying things like “I’m not scared of legacy code” and other stupid, masochistic statements when applying at trivago a few years ago.


Fact is, back then we needed people who were not scared of adding the 107th` case` to a` switch` . Full-stack developers, who could implement a feature in PHP and then again in JavaScript, because that’s how we rolled. We were “isomorphic” before it was cool. People who understood which one of the three (half-finished) frameworks would be most effective, who were down with jumping into the mud pit to wrestle with our “application architecture”, that old hairy-chested, stinking, profanity-spewing architecture from… Wait… We were talking about bugs, not waxing nostalgic about legacy architecture. But, as we all know, those two often go hand in hand. Today, I would like to tell you about our journey and how we tamed the monster we called “trunk”.


## “trunk is broken”


The biggest issue we faced as a development team was “trunk is broken.” Yes, we used SVN until recently, and yes, there are better version control systems out there, but more on that later.


All developers could always just push to trunk. Of course we had unit tests, doh… But no-one ever executed them. I certainly didn’t.


”trunk is broken” is where we’ve come from.


So, sometimes, you just coded blind, at least you could test on the staging system. And when that was not working, then a release and the production system would also do the trick. But, of course, testing in production is really not a good idea, which brings us to the first step towards improving quality.


## The Release Process


So, what do you do when developers use trunk as a temporary testing environment and parking spot for their bugs and unfinished features? Well, we defined trunk as inherently unstable, once a week we created a “release branch”, and then bug-fixed that until it was ok to release. A relatively simple process:


` development -> code freeze -> testing / bug-fixing -> release`


We quickly realised the time between the code freeze and release was the crunch point. So we started adding dedicated QA resources, process changes to allow the developers to focus on bug-fixing during that period, and we started playing with the timings. Switch to two week cycles, code freeze on Monday and release Thursday, no, let’s try moving the code freeze to Friday, but then the developers have less time to work on features, so move the code freeze to Tuesday and release the day after, but, oh dear, now QA has too little time to check everything, so move everything around again and again and… you get my drift.


The introduction of *release managers* , who were given the task of coordinating the chaos, led to high stress levels and mis-directed pressure onto an individual who had no influence on the situation to begin with. Unhappy system administrators, in charge of physically deploying the new versions onto servers, would encounter errors only a product developer could solve or even begin to evaluate the severity of.


All these changes made a difference, some negative, some positive, but, in general, the improvements were minor. It took a while for us to realise the fallacy of trying to improve the stability of a software by changing the way you release it.


## Growth and a Relaunch


One of the problems with a monolithic legacy code architecture is that getting it stable **at all** is very difficult. Legacy code tends to be very resistant to automated testing. And gets very rebellious and unhappy when you throw modern tooling at it. It is also very difficult and time-consuming to refactor even small parts.


> *Poking the monster from the left makes it fart out its right nostril.*


Fortunately, in 2012, we got the chance to do a full relaunch of trivago’s main product, our hotel search. We decided to go with a single-page application, built on the Symfony 2 framework, giving us the chance for a fresh start.


At the same time we began to grow our team very rapidly, adding new developers every month. Our feature rate also began to rise rapidly. We used to have 5 - 10 A/B tests in our code base, shortly after the re-launch we were pushing 20, then 50, then we hit 100. What started out as a well-designed application began to look surprisingly similar to the monolith we were trying to leave behind. And then we started seeing **those bugs** again…


So we turned to tooling and process changes:


- The introduction of a *CI server* and *automated test jobs* with XMPP broadcasts to all developers on failure.
- We appointed a *“trunk owner of the week”* , who, in addition to his normal duties, was made responsible for monitoring and chasing down bugs and issues when broken versions that were deployed to the staging system prevented QA from testing.
- A growing number of *unit / integration tests* began to appear, like white unicorns in the darkness, grey Gandalfs on our bridge to a culture of quality.
- Our QA team started working on *acceptance tests* , new *testing frameworks* and *requirement specifications* .
- Operations worked hard on improving the *automation of the release process* .
- We migrated our repositories to *Git* . Finally.
- The introduction of a *standardized development environment* in a provisioned Vagrant machine also made everyone’s lives easier. As did the introduction of *free hardware choice* , giving developers the chance to work with the tools they preferred.
- *Recruiting efforts* were ramped up massively and recruiting practices were improved. We started attending and sponsoring a large number of conferences.
- We put a lot of effort into the local[PHP User Group](http://www.meetup.com/PHP-Usergroup-Duesseldorf/) , which hit record attendance numbers for Germany.
- We also started focusing on hiring *specialists in one technology or language* , instead of “full-stack” developers. But we made slow progress. And we were still plagued by delayed and unstable releases.


The biggest change came due to growth. We added two new development centers, physically removed from our main office. This forced us to start tearing apart our monolithic hotel search product and defining re-usable services and packages. The first steps were very difficult and we still have a long way to go. But, making smaller teams of maintainers responsible for some code started to show a positive effect on that code. It became stable. Quality software. A lesson learned…


## Company Culture


Before I tell you about where we are now, I’d like to talk about company culture.[trivago is a start-up](https://www.youtube.com/watch?v=aFy5sLc8Sjs) . Or, no, trivago *was* a start-up. But this mentality is apparent and actively lived in the company. So time-to-market and development speed remain a critical aspect of our approach to daily business. Add a very strong culture of measurement and proof, with many iterations and fine-tuning on all products and features. Coupled to very young, mostly under-staffed teams in development and operations, all working on a platform that has at least doubled its traffic for seven years on the trot.


It’s an incredible challenge, a potent mix for success, failure, growth and improvement. All the process changes, tooling, version control systems, unicorns, packages, and even Gandalf had a positive influence, but still only dealt minimal damage to this monster of a problem: balancing out business needs with a sustainable development culture. A culture of quality.


One interesting balancing act we deal with is mitigating the negative factors that top-down or sideways pressure can inflict on a development team. trivago lives and thrives due to a hands-on approach. But a dangerous line is crossed when that turns into micro-management or when personal agendas are pushed too hard. As a development team, we need to ensure that we can be trusted in our ability to properly estimate and deliver. We need to ensure we are capable of making the right decisions and always staying professional. Always ensuring objective, rational decisions and effective solutions quickly subdues conflict, the need to “push back”, and constantly increases trust.


Time-to-market is also an essential key to success. But this can quickly lead to high workloads and stress levels on individuals, narrow-minded approaches and tunnel vision due to only concentrating on *your* work and getting the feature *you* are working on out the door. To counterbalance this, focus on documenting and defining standards, principles, and best practices is essential. As well as finding application architectures that are not just effective, but understandable and flexible enough to support team growth and impressive change rates.


Adding resources and growing teams to alleviate pressure also holds great importance. But, this is not only challenging from a recruiting perspective, it is also very difficult to sustain from a cultural perspective. As a rapidly growing team, we can quickly lose sight of our common goals and this directly affects the way we write and maintain our software. Culture is no longer something that is just there, it is essential to actively develop, protect and nurture it!


A massive paradigm shift came after dropping our standard deployment cycle from two weeks to one day. Although fraught with many challenges, this also massively reduced development turn-around times!


One of the most challenging aspects from a technical perspective, yet one of the most important keys to our company’s success is the strong culture of measurement and proof. This finds expression in large amounts of A / B tests and a very iterative approach. Hundreds of small changes and tweaks while always qualifying new approaches against an existing base-line, stand in direct conflict with software maintainability. Adding functionality parallel to existing functionality inherently introduces complexity. And knowing that one variation will invariably be removed also has an effect. This fascinatingly challenging balancing act makes up most of our daily business and has so many facets.


Modularity or compartmentalization. Re-use or customization. Design consistency or visual flexibility. UX or data-driven insights. Innovation or core competencies. All moving targets, all intrinsically important, and all directly influenced by both business needs and technological sustainability.


We constantly strive to find the balance between responsible, sustainable solutions, speed and business requirements.


## A Culture of Quality


The most positive effects began to emerge from the formation and growth of teams, moving the focus off the individual. We found amazing developers all over the world, and, as each of them came up to speed and added their abilities, we saw so much positive growth. Even though many of our smaller problems were multiplied by this growth, the over-whelming trend was positive. Teams are taking ownership of the quality of code packages, libraries, and the parts of the application they work on.


We are working hard on introducing and improving *agile processes* . Cohesive, *cross-disciplinary teams* are being formed and are becoming increasingly effective. Further *development workflow* changes make use of the migration to GIT and Gitlab, *feature branch development* , and protecting branches with *CI jobs* and *peer reviews* . We also introduced a new matrix organisation to support our SCRUM teams, introducing *core competency guilds* made up of the developers that specialize in that field, language or technology. The creation and role definition of *supporting teams* outside the influence of daily business is also a work in progress.


> It’s the teams that are making the difference!


There is still so much to do… We need to improve and continue our efforts to create *effective standards* , *development principles* , and *documentation* . We’re working hard on strengthening our culture of innovation with *hackathons and internal workshops* . We’re trying to focus on *personal growth* and promoting *big picture thinking* instead of only shoving our noses into our daily business.


We will keep fighting for the culture we want, find and remove obstacles and negative influences and keep these important discussions alive. We will publish more blog posts like this. And we will continue to grow in all directions. It’s an amazing experience!


But despite all the challenges we still face, we have come so far from “trunk is broken”. By taking the most amazing people and empowering them and the teams they formed. By creating and nurturing a culture of quality.


Tooling and processes will continue to change, but the culture is here to stay!
