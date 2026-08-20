---
schema_version: "1.0.0"
document_id: "551cb3e66794f46c9d456ef0317e2d2a9a2977d9418804aa2c337b6607c60ced"
company_key: "yelp-inc-common-stock"
company: "Yelp Inc."
source_id: "yelp-inc-common-stock-news-import-edc60bdde10d"
canonical_url: "https://www.yelp.com/engineering/whats-it-like"
published_at: null
first_seen_at: "2026-07-22T20:50:01.059675+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:446a84877d665ab4d7d7779a47169eb808cc33504df01147b8564f5c32526a13"
---

# What’s it really like to work at Yelp?

## Questions


- Why did you decide to work for Yelp and what keeps you there?
- Hey, I heard you took down the site once. What happened?
- What was the process of switching teams/roles like?


-


### Why did you decide to work for Yelp and what keeps you there?


-


Clare C.


Software Engineer


Revenue


The friends I graduated with could be split into three categories when it came to finding a job — people who interviewed everywhere, people who interviewed nowhere and people who were going to grad school. I interviewed everywhere. It was exhausting, but come decision time I was fortunate enough to have a few competitive offers from companies all over the US. All of them offered me the opportunity to work on interesting, challenging technical problems, with a variety of perks and teams full of very smart, talented people. The reason I chose Yelp over all of these other options came down to two things - the people and how much of an impact I could make.


First, the people who work here are brilliant. Not only are they incredibly talented, but they’re also helpful and humble and friendly. I learn something new from my team every day and genuinely like interacting with them. You can have lots of talented people, but if they’re not also personable you’re going to have a bad time. I really like and value how everyone I work with takes an active part in creating the engineering org community instead of having the community created for them.


Second, I like to build things! Being able to make an impact without too many meetings or administrative overhead is really important to me. At Yelp the engineering team is small and flat enough that I get to make really impactful changes to our system. It’s an awesome feeling to be able to redesign large portions of a system that sees so much throughput and not worry about ownership or deferring to someone solely because they are at a higher level than me. We’re a team, not a hierarchy.


-


Tianyu L.


Software Engineer


Messaging


Yelp has grown a lot in the 6 years I’ve been here, and there’s been a noticeable increase in the resources it can dedicate to individual wellness and development. There are a lot of opportunities to attend conferences that appeal to you (Grace Hopper, PyCon, etc.), and a lot of encouragement and support to speak at public events. Considerable effort goes into maintaining culture and fostering a supportive engineering environment in the office as well.


I value the flexibility Yelp affords. My managers have been very supportive when I’ve needed a change, whether it be shifting my schedule to learn German after work, switching off web development to try Android, or working remotely from the NY or London offices to complement a trip out of town. Yelp keeps me busy and engaged without requiring me to sacrifice extracurriculars.


-


### Hey, I heard you took down the site once. What happened?


-


Wing Y.


VP Engineering


Consumer Product


In my first five years at Yelp I deployed code to our production servers nearly 300 times. Not all of them went well; some of them resulted in triggering huge numbers of errors, others seemed fine at first but later we’d find breaking issues. One of my most spectacular failures involved taking over a failed push from a colleague. We had decided to roll back a change, and instead of rolling things back to the last known good branch, I accidentally deployed a completely broken branch that had been sitting on our staging environment from the night before. The entire site became unresponsive. I immediately went to talk to someone from the Infra team and we figured out what happened and rolled out the right branch to production. I was mortified because I was completely responsible for making a silly mistake.


I wrote up a postmortem with some action items and got several helpful replies from our Ops team about how they could make the deploy scripts less error-prone, which they started working on soon afterwards. We continue to learn from our mistakes, and this is one of the things I really appreciate about our engineering team.


-


Wei-Hong C.


Tech Lead


Search Recommendations


When trying to remove thousands of erroneous rows in our MySQL database, I ran the (notorious)` delete`


command,` delete from \[TABLE\] where \[COLUMN\]=\[VALUE\]`


, which caused a replication delay of one minute or so. I was pinged by DBA immediately about this incident, and was encouraged to write a minimortem as well as to add a batch script for future deletion.


Yelp hires people of different backgrounds for specific positions, so it’s likely there are technologies and systems that one isn’t familiar with in the beginning. For example, I am a machine learning geek and didn’t know that much about massive database deletions and their performance implications. I am grateful that Yelp offers me the opportunity to own a real system, and at the same time allows myself and others to learn and improve from my mistakes without finger pointing.


-


### What was the process of switching teams/roles like?


-


Olivia G.


Software Engineer


Yelp Reservations — Core


It’s not uncommon for software engineers to job hop after the first 1—2 years at a given company. I considered myself more likely to stick around than the average SF software engineer, but sure enough, just after hitting 24 (albeit nonconsecutive) months at Yelp, I learned that my team would reorganize to join newer, more focused teams at Yelp. Given this transition, I stepped back and asked myself, if I were joining Yelp as a new hire today, what team would I want to be on? What problems do I want to think about? Who do I want to work with? I knew that I liked Yelp, and that I could find a good outcome within the company.


To answer these questions, I read through sets of team goals across Yelp Engineering, perused in-progress tasks for teams that interested me, and looked up groups of employees on each team. Yelp is still small enough that I knew a couple of people and was familiar with a couple of projects on each of the 5-6 teams I looked into. After a week of exploration, I brought my decision to switch teams to my existing and potential managers. We talked about the vision for the new team and how I could be part of it. From then, it was only a question of when I could start.


I now understand that a team switch can be a means for growth, not just job hopping for the sake of something new. With my new team, I’m excited about our purpose, eager to tackle new problems, and ready to learn from the engineers around me. And I’m grateful that Yelp Engineering’s transparency helped me find my new role so easily.


-


Anusha R.


Engineering Manager


Ad Delivery Infra


My transition from being an engineer to an engineering manager happened over 1.5 years. I wanted to try out something besides engineering, and I wasn’t sure how to choose between product management and engineering management. It all started by calling out this dilemma in a quarterly review with my manager.


Communication skills were identified as a big area of improvement for me, irrespective of which track I was interested in. Over the next few quarters, my manager identified a few opportunities for me to work on my communication and also flex my muscles in leading projects, mentoring and helping others grow as engineers and working with stakeholders. Quarterly conversations helped me get timely feedback and identify more opportunities. Soon enough, I realized I might like engineering management more and I conveyed this to my manager. My responsibilities on the team became bigger and bigger and I transitioned into managing my peers who were working on the projects I was leading.


There is a steady stream of opportunities at Yelp to pick up and hone new skills in engineering. In addition to opportunities to lead team projects every quarter, our internship program provides opportunities for engineers to mentor folks, handle multiple projects and get better at multitasking. This helped remove a lot of uncertainties around timelines for my transition. I felt challenged every step of the way. As I made this transition, every engineering manager I reached out to was also very supportive and made time to meet and share lessons.


One last thing to highlight is that I had 3 different managers during the last 1.5 years, and my progress was never stalled due to this fact. I was always given the support I needed for my development.


-


Michael B.


Software Engineering


Performance


Yelp made the process of moving from a manager role back to an engineering role seamless and surprisingly easy. I had an amazing manager throughout who never made me think I was making a mistake or that I should suppress my desires for the good of the organization. It went roughly like this: I casually mentioned in a few 1:1s that I was feeling the engineering itch again. When this had happened a few times, my manager straight out asked if I wanted to return to an engineering role. After several weeks of deliberation, I said that I did, and from then on it was just logistics - talking to other teams to find out where I wanted to go, finding and training up my replacement, and managing the transition. My manager was as helpful as possible, which in turn made me want to do my best to facilitate a smooth transition.


Now I’m back as an individual contributor full-time, and I couldn't be happier or more productive. All the skills I gained during my management stint - planning, organization, effective communication, time management - help me on a daily basis. I now have a better understanding of our engineering organization as a whole, and have gained a new appreciation for it as a result.
