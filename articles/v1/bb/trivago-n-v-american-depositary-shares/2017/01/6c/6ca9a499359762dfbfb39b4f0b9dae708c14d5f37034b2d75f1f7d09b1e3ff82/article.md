---
schema_version: "1.0.0"
document_id: "6ca9a499359762dfbfb39b4f0b9dae708c14d5f37034b2d75f1f7d09b1e3ff82"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/code-review-best-practices-and-common-pitfalls/"
published_at: "2017-01-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:28.827282+00:00"
content_hash: "sha256:6273babef2405da096964a4478b06fcd1ab347a3848bb8f3db00be4422436d1d"
---

# Code Review: Best Practices and Common Pitfalls

At trivago we have been using code reviews as a part of our process for a good while now. In the beginning they weren’t used by many teams but as word of their positive impact spread, more and more teams started adopting this practice, benefiting every day from its many advantages. Like any new practice it has been a learning process from the start. In this blog post I will cover why code reviews are incredibly beneficial when done right and will share what we have learned and which best practices we employ.


While it might sound like a simple thing to do, there are many things that can be done wrong in reviews. They can damage the team atmosphere and can unnecessarily slow you down. So if you’d like to implement code reviews into your workflow or would just like to be reminded on what to keep an eye on during reviews do read on as this post is for you.


## What are code reviews?


Code reviews usually happen at a point where a developer believes a specific change-set they have worked on is finished and is ready to move forward. Code reviews can of course happen at any point. For bigger projects for example you might want to do a few rounds of review during the development phase already.


Once a change-set enters the review phase at least one other developer goes through the change-set and gives their feedback before the change would be tested and released.


The reviewer(s) will ask themselves a lot of questions while reading through the code, some might be:


- Does this change-set solve the problem it was supposed to solve?
- Does this change-set make sense in the current architecture?
- Can any of the code be written more efficiently?
- Is the code easy to understand?
- Does it increase or decrease technical debt?


There are many things to look out for and many ways to communicate your feedback. I will go into more detail about reviewing someone else’s code in a later section:‘Giving code reviews’ . Let’s first move on to the benefits reviewing each others’ code brings to the team and business.


## The benefits of a healthy code review practice


At a first glance it’s easy to jump to the conclusion that this step in the software development process is a waste of time. Instead of perhaps one person taking the time to write a piece of functionality now multiple people are involved but the outcome is more or less the same? Why should we invest time in this?


Once we consistently started doing code reviews at trivago the benefits became apparent really quickly. In the end the extra time spent on them paid off even more than we expected.


### Catch potential problems early on in the process


Having another pair of eyes look at the code and test its functionality helps catching potential problems much earlier than usual. The later a problem is identified in the process the more expensive it becomes to fix. Catching issues early saves everyone involved a lot of time.


### Share domain knowledge


The knowledge of the introduced change-set is instantly shared with the team. Everyone automatically gains a better understanding of what is going on in the project and of how certain things are implemented thus increasing[the bus factor](https://en.wikipedia.org/wiki/Bus_factor) . Silos of knowledge are reduced which makes it easier for other members of the team to become productive in the area where the changes were made.


### Strive for greatness


Code reviews are one of many great ways for more seasoned developers to share their experience and ‘mentor’ less experienced developers without getting too much in their way. It pushes every developer involved to deliver a consistent level of quality and will raise the overall code quality of the team as time progresses.


Having discussions about how certain changes are implemented ensures that each idea is considered and that the best one is chosen. Different points of view bring different perspectives to a problem. Reviews are one way for team members to help each other to grow, learn and become the best possible version of themselves.


## The dangers of doing code reviews


If done badly, code reviews can have a negative impact on your team performance and atmosphere. You’re giving and receiving feedback. We as human beings sometimes aren’t very good at this, especially when we approach it with the wrong mindset. On top of this most reviews happen by directly commenting the lines of code in an online tool like[GitHub](https://github.com/) or[Bitbucket](https://bitbucket.org/) . Written text always has some room for interpretation. Add the inherent complexity of computer code to that and suddenly there is a big risk for misunderstandings.


Luckily there are some best practices to keep in mind for both the reviewer and the reviewee to minimise the risk of egos being damaged or time being wasted.


### Receiving code reviews


#### Prevent style-guide violations and failing unit tests


As someone submitting your code for review you should make sure that it already conforms to the standards you as a team agreed upon. Style-guide violations will be caught in reviews but are a waste of time for both you and the reviewers involved. Make sure your linting tools are set up properly and that you run your tests before submitting your changes so that any violations or failing unit tests are caught automatically. This ensures that reviewers don’t have to waste their time pointing out these issues and thus will raise the value of the discussions held during review.


#### Split your change-set into sensible commits


You’re probably already aware of some[best practices regarding writing commit messages](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) . These become especially beneficial during reviews. Explaining what your change-set does in a commit message or defending any weird workarounds you had to add isn’t only helpful when digging through a project’s history but also when reviewing each others’ code.


Make sure each commit contains an isolated chunk of work, this will make it easier to isolate the changes and understand what they’re meant to do for the people reviewing.


Say for example you want to implement a feature through which you also have to touch some files which contain code that violates the current style-guide. You will probably want to implement your new functionality but also rewrite some of the code so that it conforms to the current style-guide. If you would do this in one commit this becomes very difficult to review. The reviewer somehow needs to figure out what was actually changed in functionality and what were just the style changes. It’s practically impossible to split the one from the other. This is an example where separate commits can save a lot of time. You make one isolated change in one commit and then move on to the other. This makes it much easier for reviewers as they can go into the separate commits and add their feedback there.


Properly splitting up and documenting your commits has many more advantages, in this article however I will only cover the effects on the code review process.


#### Reviewers are reviewing your code, not your person


As software engineers we like to be proud of the solutions we came up with. This can make it easy to sometimes take other people’s criticism personally. Always remember that it is the code that is being reviewed, not you as a person.


Code reviews aren’t a competition on who can come up with the smartest solution or who is the real ‘coding genius’ or ‘JavaScript ninja’. This[‘genius programmer’ myth](https://lwn.net/Articles/641779/) in our industry is a destructive idea in which we sometimes believe there are two categories of programmers: those that suck and those that have an IQ score of over 9000. And you’d better not be found out to be one of those that suck! Of course this is ridiculous, we all started from zero at some point and had to take every single step to get to where we are today, there simply are no shortcuts. Most of us are very average at what we do and are continuously striving to get better at what we do every day.


If you’re a less experienced developer, don’t be afraid to ask questions or give feedback to more experienced developers. The worst that can happen is that you might be wrong and will have learned something new. Reviews help us have a peek into each others’ thinking process and are a channel through which we can help each other to grow and improve. Keep this in mind when receiving (and giving) feedback of any kind.


#### Follow up on given feedback


Feedback is useless if it is not taken to heart. When the review rounds and discussions have finished make sure that you follow up on what was agreed upon. This means making changes to your code and triggering another round of reviews for people to have a look at the newest changes. The same principle of properly splitting up and documenting your commits applies here. If you don’t agree with a given comment don’t be afraid to challenge it, productive discussions are beneficial for each party involved.


Sometimes it can happen that an issue which is totally out of scope for the current change-set was highlighted during reviews. In this case you can note it down somewhere to make sure it’s followed up upon and will be worked on or discussed in the future. It’s important to identify these so that they don’t block the current work from moving forward.


### Giving code reviews


#### Take your time


For code reviews to be as helpful as possible you should take the appropriate time to read through the problem that the change-set is supposed to solve and to understand what’s going on in there. This also includes checking out the branch locally and actually testing the changes if that’s necessary.


In the day-to-day work it’s very easy to superficially glance over the changes and if nothing immediately jumps to the eye simply hit the ‘approve’ button. While under certain circumstances this may sometimes be inevitable, this scenario should be the exception, not the rule. This behaviour of reviewing will only catch very superficial errors such as code style problems. As mentioned earlier these superficial problems should ideally be caught by tooling anyway, not in reviews.


#### Put thought into properly wording your feedback


It’s important when providing feedback to put yourself into the reviewee’s shoes and imagine how they would feel about reading any comments you decide to post. Don’t post comments which sound negative or are blaming the author in any way.


Try to stick to hard facts and stay away from very opinionated ideas. A certain piece of code can be written in a million different ways. If you don’t agree with how something is written make sure that you have something to back your idea up if the reason might not be immediately visible. Try to avoid comments which are purely opinion-based and thus don’t add any value.


When reviewing a less experienced developer’s work it can be useful to link some relevant reference material. Say for example you suggest to rewrite some of the code to use a specific design pattern. Try to properly explain why that specific pattern would be useful in this case. Moreover you can link to a webpage explaining how the pattern works and what its general benefits are.


#### Value face-to-face interactions


The big advantage of having code reviews in pull requests is so that everyone can participate whenever it makes sense to them. As a downside you don’t have any in-person interaction and increase the risk of miscommunication. The[agile software development](http://agilemanifesto.org/) principle of choosing interactions over processes and tools also applies to code reviews. It can be much more efficient to go to the other developer and discuss anything regarding their pull request in person. This also counts if you’re submitting pull requests: if you think a comment needs more clarification don’t be afraid to go talk to that person face-to-face if possible. It will clear up any misunderstandings and will make sure you can better grasp what your colleague on the other end meant.


## Final words


Code reviews are a great tool for your team to increase general code quality and share knowledge with team members. Most of the advice in this post is common sense so if you’re not doing code reviews already do try to give them a go. There are many ways in which you can implement this practice; don’t be afraid to experiment a bit to figure out how they can be most valuable to you and your team.


Going full-circle: each blog post on our tech-blog, including this one, is submitted to a git repository and goes through several rounds of review before being published to the blog. So you too have already benefitted from our process as this article has been extensively reviewed by my colleagues, resulting in more concise and coherent content.
