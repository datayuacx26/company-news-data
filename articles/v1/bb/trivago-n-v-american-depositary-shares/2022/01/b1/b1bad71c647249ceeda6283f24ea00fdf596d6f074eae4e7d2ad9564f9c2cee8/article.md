---
schema_version: "1.0.0"
document_id: "b1bad71c647249ceeda6283f24ea00fdf596d6f074eae4e7d2ad9564f9c2cee8"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2022-01-12-engineeroncall/"
published_at: "2022-01-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:cee9826d4733e5e844ef7320a9d6fb16be5e5ec9e0356b1f05a8732a91187a09"
---

# Being on-call as a software engineer - a challenging and fast learning experience

At trivago, we run webservices with complex backends in different regions around the globe 24/7. Our system is being iterated and developed on a daily basis. Naturally, mistakes will be made and something will break eventually. Engineers being on-call are the first responders to issues with negative impact on our users and the business.


Once an alert is indicating potential issues with the system, the on-call engineers scope the issue and take meaningful action to mitigate it. They also follow-up with relevant engineers and their teams the next working day to initiate follow-up actions for root-cause analysis to address the underlying issue. Post-mortems, like[this one](https://tech.trivago.com/2021/10/05/postmortem-removing-all-users-from-github.com/trivago/) describing how we accidentally removed our org-account from GitHub, are created to share knowledge and learnings about incidents company-wide.


In this blog post, Stefan Nothaas, one of our backend engineers, shares insights and his experience as a software engineer being on-call for the last 1.5 years.


### Hey Stefan! Thanks for chatting with us today. Can you tell us a bit more about your journey at trivago so far before we deep dive into the on-call role?


I joined trivago about 2.5 years ago as a Java backend engineer. So far, I have been roaming around different backend projects which also lead me to dive into infrastructure related topics. I highly appreciate the open culture, scale of tech and operation, and professional development opportunities. Interesting challenges everyday, many cool and inspiring people, and the learning never stops.


### Why did you volunteer to be on-call?


My primary motivation was and still is that on-call is an amazing learning platform for an engineer. I learned a lot in a very short period of time about the business and the technology driving our infrastructure. This includes various managed services on Google Cloud, kubernetes, Kafka, a lot of useful CLI tooling, and how in general our systems are architected with its benefits and shortcomings. Naturally, I also got to know the many different services and the teams developing them. In summary, I am confident to say that on-call pushed my engineering knowledge and skills to the next level.


### How often are you actually on-call?


Booking your shifts needs to fit your personal schedule and preference. I prefer longer continuous shifts of around a week over fragmented single days. This setup is easier to manage with my private schedule. But, we also have engineers who prefer multiple fragmented 1-2 day shifts over the month.


### What were your biggest learnings since joining on-call so far?


My personal key learnings were:


- How the various sub-systems are working together on a tech-level
- How to maintain a more structured schedule due to an increase in responsibilities
- Improved my communication and writing skills, e.g. in the case of incident management, writing post mortems
- Got to know and work with incredible people who know a lot about the tech and systems
- Staying calm even when everything is on fire and confusing! Only a calm mind can come up with a structured approach to successfully take action and do the right thing.


### Can you recall your most memorable event/incident when you were on call?


There are actually two, one very positive and one not so positive event, that I would consider memorable.


#### Very positive


Multiple incidents during business hours: I didn’t have to take action because the first responders were actually engineers of the team responsible for the affected services. They took ownership immediately, communicated efficiently and resolved the issue quickly and professionally. I was really happy to see the ownership mindset in action by everyone involved.


#### Not so positive


Unforeseen events due to bad timing and simply bad luck are fortunately rare, but unfortunately they still happen. Once, I got alerted in the middle of the night due to issues with one of our Kafka clusters in the US datacenter (time zones, yay!).


Although there are always two engineers on-call, my colleague didn’t go online and I couldn’t get him to respond using our escalation tools. The next day, we couldn’t figure out why his phone never received the alerts. On future shifts, this never happened again.


After analyzing the symptoms of the incident, I realized that the issue at hand is something that apparently never happened before. Therefore, our existing operational documentation couldn’t cover it in detail, only some rough edges.


In hindsight, our Kafka experts figured out that a configuration issue and bad timing was the root-cause. Since I’m not a Kafka expert, I lacked the necessary knowledge during the incident to understand why something was going wrong. At this stage of the development of the on-call process, there were no further levels of escalation implemented at this time.


In the end, I did the best I could do to limit the impact of the issue. However, we pointed out the short-comings in a post-mortem and the team took the necessary actions including documentation and Kafka resilience improvements. Future incidents allowed us to escalate such issues directly to a newly established 2nd level on-call group successfully.


### After an incident leading to (longer) follow-up work to properly fix the root-cause, how do you balance this with an actual running project with tough deadlines? Do you go into a negotiation every time with your Product Manager?


Teams with engineers on-call and their managers are aware of the additional duties and responsibilities. Ensuring the health of the status quo of our systems and keeping any negative business and user impacts low has the highest priority. Communication to our teams and managers is essential to create transparency regarding additional effort and time required to support other teams in root-cause analysis. I never had to go into negotiation with my managers about this as they are aware of the business value of those contributions by engineers on-call.


### Are there any large technical or process issues that you wish someone would solve for on-call? Maybe in terms of monitoring tech, etc.


I’m happy to say that I think our foundation regarding observability, documentation, alerting, engineering exchange/communication is already quite good and has improved significantly over the last years. Spreading awareness of the relevance of up-to-date documentation, for example, and an ownership mindset among engineers and teams supports engineers on-call well.


### What can other engineers do to make on-call engineers’ lives easier? What can company leadership do?


Take ownership: You build it, you run it… and when it breaks, you fix it. This includes participation in the on-call rotation. This often requires a major shift in mindset for engineers until it can be fully embraced and established in the engineering culture of the company. This is[not a new idea](https://paulosman.me/2019/12/30/production-oriented-development/) and has already been described[by other amazing engineers](https://blog.pragmaticengineer.com/the-product-minded-engineer/) in great detail.


### What are typical excuses/sentences you hear from your software engineering peers on why they are not interested in going on-call? And how did you overcome them?


What I heard often when I talked to people about on-call and bringing up the question if they might be interested in giving it a try boiled down to “It’s too difficult”, “I don’t have the required knowledge/skills” and “I am anxious that I cannot handle a serious and stressful incident situation”.


Getting started is always difficult, but a good on-boarding process, shadowing engineers on-call on incidents during business hours and #FanaticLearning helped me to increase my confidence in taking on real shifts after 1-2 months.


Furthermore, if you cannot or do not want to continue being on-call for any reason, you are always free to exit the rotation.


### What would be reasons not to join the on-call rotation?


Your responsibility to be available and take action once you get alerted doesn’t end at the end of the business day. This is important to take into consideration for your personal circumstances as you can get alerted at any time. 3 AM incidents are unfortunately one of the trade-offs. However, such a night is always balanced out thanks to understanding colleagues who appreciate the effort, flexible working hours to catch-up on sleep and a big cup of coffee the next morning.


### What advice would you give someone who was thinking about joining the on-call rotation?


Try to get a feeling for what it would mean to you when being on-call. Potential steps to take might include:


- Talk to a few other engineers/SREs who are on-call to get an authentic experience from individuals within your company who are regularly getting their hands dirty.
- Understand how the on-call process is defined and figure out what impact it would have on your daily work and private life.
- Start shadowing people on-call: ask to join their incident sessions/calls and observe what they are doing, how they are doing it, how they communicate etc.
- Practice, practice and practice.


### What advice would you give to engineers at a company that doesn’t have any on-call process?


I think it depends on the company and product, the engineers and whether an on-call rotation is the right tool to solve a problem they may (not) have. On-call is required if a business needs engineers to take action due to unforeseen events that could have a negative impact on the business. If your business is regularly facing technical challenges that have a major business impact, for example due to system outages during the night, on-call rotation might be a helpful tool to consider.


---


Did this article spark curiosity about on-call in the engineering role and our engineering culture at trivago? Don’t hesitate to reach out to us with your questions on Twitter[@trivago_tech](https://twitter.com/trivago_tech) – we’re always up for a chat!
