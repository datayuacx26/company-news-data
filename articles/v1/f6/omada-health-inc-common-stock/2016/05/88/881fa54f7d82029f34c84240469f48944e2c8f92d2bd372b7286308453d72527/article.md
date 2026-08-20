---
schema_version: "1.0.0"
document_id: "881fa54f7d82029f34c84240469f48944e2c8f92d2bd372b7286308453d72527"
company_key: "omada-health-inc-common-stock"
company: "Omada Health Inc."
source_id: "omada-health-inc-common-stock-rss-9e1c3b1ccfca"
canonical_url: "https://medium.com/omada-health-tech/how-to-run-an-internship-program-2bb91065ac8"
published_at: "2016-05-04T17:00:59+00:00"
first_seen_at: "2026-07-20T23:18:22.449098+00:00"
fetched_at: "2026-07-28T22:27:31.756931+00:00"
content_hash: "sha256:8b2ab8f6d1d8372ca8c8caf8a6b35ac7205297276e2bdd71b810c44aa6790d0f"
---

# How to Run an internship program

# How to Run an internship program


[Omada Health Engineering](https://medium.com/@OmadaEng?source=post_page---byline--2bb91065ac8---------------------------------------)


6 min read


·


May 4, 2016


--


*Written by*[Lillie Chilen](https://www.linkedin.com/in/lilliechilen/) *, Engineering Manager*


Photo by[Tim van der Kuip](https://unsplash.com/@timmykp?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on[Unsplash](https://unsplash.com/s/photos/office?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)


In[a previous post](https://medium.com/omada-health-tech/internships-good-for-the-intern-great-for-the-team-8b13c61cf4a7) , I explored how running an internship program can benefit the team (and the company) more dramatically than you might expect. Now that you’re totally bought into why interns are awesome, you’re probably wondering how you can run an effective internship program at your company.


## How our internships work


Whether the internship is three, four, or six months long, an Omada engineering internship follows the same general format: join the team and pair with everyone, do a solo project for a spell, and then rejoin the team. We’ve had 13 engineering interns so far, and each came to Omada with a different amount of experience with our stack. Having them start with the team gives us a chance to share our practices with them, and dump a heap of knowledge on their heads while they learn their way around the codebase. The solo project then gives them a chance to integrate some of that knowledge, and code review helps us and them see what they are still struggling with. When the intern’s solo is over and they triumphantly return to the team, their growth is evident.


## Interviewing


Since interns are short-term employees, they go through an abbreviated application process — first, they complete a coding challenge that has some failing tests, then a general-purpose phone screen with questions about their technical interests and what recent code they’ve written that they are excited about, followed by a remote or in-person pairing session working on a small technical problem.


You clearly aren’t hiring an intern for their experience, so you should have a clear idea about what you *are* hiring them for. At Omada, we look for interns that ask a lot of good questions, display curiosity about how we ship software, and who seem to be able to pick up new concepts fairly quickly. Algorithms and white boarding sessions wouldn’t give us nearly as much insight into how the candidates actually write software, so we happily avoid them. (As we do with our full-time candidates, as well. White boarding interviews are terrible and people should stop doing them.)


## Pairing


The biggest part of why we’re able to have an intern join the team and write production code immediately is that we do pair programming by default. Depending on the team, this can range from 40% to 90% pairing. Practicing our pairing skills with a junior developer makes us better pairs when we work with all of our teammates.


(Pair programming is where you have two keyboards, two mice, and one computer, and you take turns typing, and you work through the whole problem together. Pairing is not where you look over someone else’s shoulder when they get stuck, or white boarding architecture with a group of people, or working on similar code next to someone on separate computers.)


If your company does not currently have a pair programming practice, I wouldn’t recommend starting with pairing with your interns. Pairing across experience level is is a substantially different experience than pairing with an engineer more senior than you or at your level. It’s a skill not to hog the keyboard or go too fast when you’re on a roll, and properly get your pair’s buy-in for your idea when they know a lot less about the codebase than you. Sarah Mei has a[great piece on precisely this problem](https://www.devmynd.com/blog/2015-1-pairing-with-junior-developers/) , and you should read it!


(I do recommend you start pairing! Just with people at the same skill level, then later try to expert-mode that is junior/senior pairing.)


If you do pair, some of these things have helped us let our juniors drive more:
* Really doing TDD and ping-pong pairing it (avoiding spikes, where the junior can get lost)
* Having toys and other things for the senior to do with their hands to avoid typing
* Unplugging the senior’s keyboard!


It’s **really** hard not to grab the keyboard when you don’t know why the test is failing. As much as you can, resist the urge, and work through the mystery with the intern driving. You can do it!


## On-boarding & Mentorship


Having regular interns keeps our on-boarding processes in tip-top shape. We also make sure that the intern’s laptop is used for coding as soon as possible — usually on the first day, but definitely by the second day of their internship. When you’re pairing, it’s easy to use the machine that’s already set up, but that’s a trap! It’s important for the new person’s environment to be set up ASAP.


Interns are paired with more senior developers who act as a mentor during the internship, answering technical and non-technical questions. The intern’s mentor isn’t the only person who reviews their pull requests, but especially during the solo project, the mentor checks in regularly with the intern to make sure they’re on a reasonable path with their project. Regularly, in our case, has often meant a daily, scheduled check-in, otherwise it doesn’t happen often enough. Having time every day during the solo has helped us prevent our interns from going totally off the rails on their projects. (Of course, if they have questions during the day, they’re welcome to ask any member of the team. We haven’t yet had an issue with an intern asking too many questions.)


## Solo Project


Omada engineering interns have a lot of say in what they build during their solo project. We’ve had some interns make tools that never got used (and they still learned things, which is valuable!), but we are especially excited when an intern can learn a lot while working on a project that is useful to the company. We’ve had especially good luck with having interns work directly with a product manager for their solo project, and letting them figure out what to build together.


If you’ve got an intern that really wants to work with a cool new technology that is completely unrelated to your current stack, I’d suggest gently reminding them that the value of their internship is not just in the code they write, but the process of working with your team. Trying out new tech is a great thing to do in your free time, when you don’t have the wealth of resources of a team and customers to build things for!


## Code Review


Since we pair program, we don’t normally do code review via pull request. The exception to this is when any engineer is soloing, and so interns on solo projects submit pull requests that are reviewed by their mentor or another person on the team. Since the intern has already had at least a month of pairing with the team, they are usually moderately aware of our practices and style, so the feedback on each pull request isn’t starting-from-scratch education about how we write code at Omada.


If your company doesn’t pair program, you’ll probably want to start with a project that is somewhat less than business critical. It is important, though, that the intern is collaborating with other people at the company as much as any other engineer would. You’ll only reap the benefits of having interns around if they are fully integrated into your typical engineering processes and working on your actual codebase.


## Hire them!


At the end of someone’s internship, if they’ve been awesome and we have an opening, we offer them a full-time position. This is an excellent way to further leverage the time investment we’ve made, but we still get a lot out of mentoring the interns that don’t end up in full-time positions.


At Omada, our interns have been fantastic teammates and many have stayed around in full-time positions. Having interns around regularly has helped us improve our communication skills, our codebases, and has really helped us increase the gender diversity on our teams.


(For more on why running an internship is incredibly valuable to the whole team, take a[gander at this post](https://medium.com/omada-health-tech/internships-good-for-the-intern-great-for-the-team-8b13c61cf4a7) .)
