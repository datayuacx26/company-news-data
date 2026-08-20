---
schema_version: "1.0.0"
document_id: "b431edc9b60d60f588cdc48f40dc8d5272ea91325da3f59359706136f5481c3f"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2017-01-11-espresso-re-learning-initiative/"
published_at: "2017-01-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:29.470006+00:00"
content_hash: "sha256:f2cb2ed63f4a3357e0e070b27846d30ae78c416acc4080c62c1d46ad22ba0505"
---

# A Large-Scale JavaScript Re-Learning Initiative

### Or: What is this Espresso thing, and why is everybody doing jigsaw puzzles?


Because[our main application](https://www.trivago.com/) was getting ever more memory-hungry, our awesome JavaScript architecture team developed a modern, freakishly efficient component and rendering framework which we will open source and highlight in a series of future articles. In order to prove the framework’s maturity, we wanted to roll it out to the trivago core application.


There were a couple of challenges to overcome, though. One - rather minor - challenge was finding a project name for our initiative to marry the new framework to the trivago Hotel Search. Project names often appear sort of random at first, and our name, “Project Espresso”, is no exception. However: An[espresso](https://en.wikipedia.org/wiki/Espresso) is small and strong, as is our framework - admittedly, still somewhat random. But not totally. Our large Italian developer and product owner community loved the name, obviously, which made it a good choice after all.


However, there was a much greater challenge ahead: The new framework demanded a radically different development approach compared to the status quo. It is a lot more functional, a lot more modular, and it is component-based. This required developers to re-think and re-learn a lot of what they had practiced and known so far.


## Training


So be it, we thought! Let’s train everybody! Everybody? Yes! Well, JavaScript devs, obviously. But also backend devs, because, they touch JavaScript more or less frequently. And also UI devs, because we were introducing a template compiler that had some implications on the way user interface templates and functionality worked together. This meant that the affected developers added up to roughly 40 people.


All right, so we knew *who* . The next question was: *How* do we teach a complex topic to a heterogeneous audience of massively different skill levels with respect to JavaScript? We had a lot to cover: Theory on unidirectional data flow,[Redux](http://redux.js.org/) , implications on Twig templates, and component-based development, to name just the largest topics. Ideally, you would adapt the course content to the current skill level of each participant, or at least divide the participants into groups of similar levels. This would mean preparing teaching material tailored to each group, and setting up separate workshops. Unfortunately, the preparation and teaching time we had available was very little. Therefore, we ended up having only one teaching track that everybody attended, regardless of previous familiarity with the topics covered. As you can probably imagine, this lead to certain problems, which I will highlight further down.


## Outline


From the beginning, we thought that face-to-face training was important. We planned for a series of five workshops, during which participants would get some hands-on experience using the new framework. Since we did not want to hold workshops with 40 attendees, we limited participants to 6 to 12 per session, and held each workshop multiple times.


Also, we wanted to keep the workshops reasonably short, so we tried a dual approach. We prepared screencasts that everybody could watch at their own pace *before* the respective workshop, so that the workshops themselves would not turn out too long, and could instead focus on repetition and practical exercises. This method allowed us to keep the workshop sessions to one hour each.


Originally, we planned for 26 screencasts of three to ten minutes each (bite-sized, similar to what[egghead.io](https://egghead.io/) does). However, since they turned out to be very time-consuming to prepare and create, and since our teaching team comprised only four people who could work on the curriculum only part-time, the number of screencasts was later reduced to 15. We complemented the self-produced screencasts with publicly available ones. For example, since our framework uses[Redux](http://redux.js.org/) internally, we referred course participants to the[excellent Redux screencasts](https://egghead.io/courses/getting-started-with-redux) by Dan Abramov. Why should we reinvent the wheel, after all?


Producing screencasts had several benefits:


- Screencasts are asynchronous, so people can watch them whenever they want, and as often as they want.
- They will always be available to new developers without requiring a mentor’s time.
- The engineers who produced them gained valuable technical and educational experience, so they were presented with an additional growth opportunity.


## Teaching talent discovered


Speaking of growth opportunities: We intentionally got young, talented developers involved in teaching activities. In fact, one of these developers had just joined the company a few weeks before. Others were quite surprised when we asked them if they could hold one or two workshops, and worried if they would be up to it. In all cases, though, we reassured them that they would do fine, and we were right. Eventually, they did a great job and appreciated the feedback we gave them on how to become an even better teacher.


## Problems of the one-track approach


I mentioned above that our one-track approach lead to certain problems. Most obviously, some people were slightly bored during the workshops, while others were struggling or slightly overwhelmed. This was to be expected given the different skill levels we faced. However, a considerable source of frustration was something we had not anticipated: ES6 familiarity, or rather non-familiarity. We had been using ES6 language features in our code base for several months, and we had offered workshops and presentations on ES6 a while back. However, by far not all participants were exposed to, let alone wrote, ES6 code on a regular basis. Moreover, the ES6 workshops had been optional, and even if a developer had attended them, she might have forgotten a lot in the meantime due to lack of practice. In an attempt to remedy the situation, we injected small bits of ES6 recap (like an explanation of the spread operator) into the course material. Since time was short, however, these small bits could not fully fix the issue.


Besides time, we also lacked opportunity for practice. While some course participants could apply the new knowledge and skills in their work right away, others had no opportunity to do so. This might be because they were working on parts of the application where they rarely touch JavaScript, or because they were working on features that would not be rewritten to the new framework just yet. Whatever the reason, if you cannot apply things that you pick up during a workshop, your motivation will most likely be lower, as will the effect of the training, because you will forget faster. A possible solution might be to offer “just-in-time” training on request. However, this would likely result in many more sessions consisting of fewer participants each, thus making further demands on the teachers’ time. For now, we decided against it.


Instead, we created an additional, purely practical exercise that people were asked to work on in pairs. By putting at least one person with advanced JavaScript skills on each pair, as well as by offering close tutoring by the teaching crew, we tried to minimize the potential for further frustration. We think that this exercise (a small sample application) gave participants a bit of a feel for the new framework and development paradigm, and took away some anxiety.


Different levels of JavaScript familiarity did not only lead to difficulties during the workshops, but also during people’s “quiet work”. As mentioned, screencasts have the advantage that people can watch them on their own. However, there is also a downside to this: When people do not fully understand the contents of a screencast, they are on their own, as well. This happened, and lead to frustration and anxiety. When we learnt of this tendency, we talked to people who were unhappy with the teaching material. We also wrote an email to all course participants, trying to calm people down and emphasizing that it was not a disaster if you did not understand every detail of every screencast. This helped a little, but some concerns about what might still come remained.


## Rewards


Since we made people give up familiar ways and enter into unknown territory, and because everybody is busy and has already a lot on their plates, the Espresso teaching team thought it would be good to have a small gift to say “thank you” for the time and effort our participants invested. Ideally, it should be somewhat funny, creative, not entirely useless, and inexpensive. Also, the giveaways should indicate the participant’s progress in this workshop series - since we were planning for five workshops, we wanted to be able to split each gift into five parts.


After some thinking, we decided to create a custom-made jigsaw puzzle that we would hand out bit by bit, from left to right. It had a large heading “Project Espresso Certification Puzzle” on it, and “Level 1” to “Level 5” labels from left to right, with pictures of (mostly) celebrities with an espresso cup.


There are a lot of online services where you can upload a picture and get a jigsaw puzzle of it. The cheapest one we found offered a preview like the following, with a 3x5 layout:


We thought: “Great! Five workshops, five columns of puzzle pieces. Only 15 pieces in total, that’s easy to split!” So we went ahead and ordered 40 of them. What could possibly go wrong, right? Delivery was quick, but then: Surprise! 112 pieces per puzzle!


And no, they did not arrive in finished state, as the picture might suggest. What on earth was going on here? How could we have missed that? I checked on the vendor’s web site, and indeed: It *did* say that the puzzles consisted of 112 pieces each. But it did so on a page that was displayed before I had entered the order process - that was where Google had taken me, and I had not skipped back.


What to do? Since the first workshops would soon start, we decided to just go with it, split the work among the teaching crew as well as volunteers, and have jigsaw assembly sessions.


However, even the group sessions could not handle the huge load of puzzles. Since trivago is a big family that extends beyond the employees, even some of our kids jumped in and helped out (they were curious and bored, anyway ;-) ):


During one of the puzzle sessions, there was a second big surprise. Naïvely, we expected all the individual jigsaws to be identical, so we started out putting the different parts in paper envelopes labelled “1” for the leftmost part of a puzzle, “2” for the following part, etc. However, my colleague[Radovan](https://twitter.com/radovanjanjic) thankfully noticed that each jigsaw was *cut differently* . In other words, you could not combine part one of puzzle A with part two of puzzle B, because they would not fit together. We had to make sure that whoever got part one of puzzle A would in the future also get part two of puzzle A, and also parts three, four, and five of puzzle A.


The bad side of this discovery was that it invalidated part of the work we had done so far. The bright side was that the new labelling scheme we used instead was more personal, with a participant’s name (or nickname :-) ) on each envelope (if you don’t know who Pistenprinz is, check out[his site](http://creinartz.de/) ):


Together, we finally managed to get it all done, and to put all the pieces in their correct box or envelope. However, before we could hand out the first puzzle pieces, we had to store them in our lockers, which looked pretty funny sometimes:


All in all, I think the puzzle part made the entire project a little more fun, and showed people that we appreciated their effort. Some participants put them up on their desks for everybody to see.


## Conclusion


Our teaching initiative in numbers:


- We held 15 individual workshop sessions so far, spread out over roughly two months. Workshop rounds four and five are yet to come. However, these will be deep-dives into the technology, and will be optional for non-JS developers.
- Around 40 developers took part in total, making it the largest teaching initiative in our department so far.
- 15 self-produced screencasts accompanied the workshops (~90 minutes of material).


By now, several of our developers have successfully used the new framework to power some major features of our core product, and we have seen some impressive gains in memory and performance. We will keep you posted as we gather more data.


Maybe the biggest learning from the teaching initiative is: If you ask people to give up their expert status for something they are unfamiliar with and insecure about, then you better go out of your way to encourage and motivate them, eliminate frustrating experiences as far as possible, and help them succeed in hands-on exercises. Introducing a new framework, and even a new development paradigm, is a large change that needs careful management, and should not be approached lightly. Just because you are excited about something does not mean everybody else is. Keep that in mind and approach the people you want to learn from you with some humility.
