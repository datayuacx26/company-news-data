---
schema_version: "1.0.0"
document_id: "4fd6e6b49107d20dbd0fe1c89dd6718e0beb9714aa96c1092cdcea2305cb4e4b"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/careers/dig-deeper-eliminating-bias-in-code-test-reviews"
published_at: "2022-12-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:c311466c1de04f1a1e36876de3d212bbda539c6f447d3c616196d7cfad7f1c5c"
---

# Dig Deeper: Eliminating Bias in Code Test Reviews

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Careers](https://www.guidewire.com/resources/blog/careers)


- Dig Deeper: Eliminating Bias in Code Test Reviews


Hi, I'm Xinyue. I'm a Software Consultant II on the CAPS (Cloud Assurance Production Services) team in Dublin and have been with Guidewire for almost three years. About a year after I first joined, I heard about the opportunity to be part of our candidate selection process. I was excited about the chance to make an impact on our team and wider department. After shadowing several code test reviews and zoom interviews, I was soon ready to support the team with our hiring.


At Guidewire, there are three phases in candidate selection for tech roles– the code test, a Zoom interview and a final face-to-face interview.


All employees are encouraged to be part of the candidate selection process to create a wider pool of interviewers and help minimize any potential bias. When candidates are picked by a recruiter, the recruiter will ask the people in the team to jump in to review the code test or take the Zoom interview. Then, people like myself, will volunteer to take one or two of the code tests interviews. This randomizes our interview panels for candidates, which again, helps minimize potential bias.


In the past few years I have done 20+ code test reviews, and our team provide clear, general guidelines on how to approach reviewing code tests. In this blog, I’d like to share more about my personal experience on how I actually make the decision during code test reviews.


As mentioned, the code test is normally the first evaluation candidates will come across when interviewing with Guidewire for technical roles. It’s a good way to pick candidates who can write beautiful code and would be delightful to work with. Do keep in mind that this is not a full examination of candidate’s coding capability, but more like 'testing the waters' to see if we want to move forward with a candidate.


### Reset Your Mind


Before looking at code test reviews, it's really important for reviewers to set their minds to an unbiased state. LinkedIn Learning has a great


[unconscious bias course](https://www.linkedin.com/learning/unconscious-bias-14822310?u=2092596) that our team have access to! 😄


Our brains are built for efficiency. That means they always want to burn the least amount of energy to give us the quickest answer or solution. This mechanism makes our brains really good at taking short cuts, i.e., jumping to conclusions. This is basically what we call our 'first impressions', and regardless how biased it could be, we sort of, stick to it.


But for interviewing candidates, we need to be the total opposite.


We should ONLY come to conclusion AFTER we put all the evidence from different perspectives together. To keep myself away from any possible exposure that will lead to my brain make a decision before I do, I consciously avoid checking a candidate’s name and or looking at their resumes for extra information that my brain might want to judge them on. By keeping this top of mind, I start my reviews with a clear mind and only evaluate my candidate based on their code.


So, let’s start- The first thing you will see on a Codility test review is the large score on the right side. It is almost inevitable that this will give you some ideas.


For instance, when you see 100%, your brain will naturally give you a ‘wow, impressive’. And when you see a lower score, your brain will be shouting ‘this is a waste of time'. But we try to not follow this idea or judge candidates yet.


I have come across quite a few candidates with lower scores, and some turn out to be quite good coworkers later. So, yeah, loosen up a bit!


### Plagiarism? Bad, bad!


Before we spend time on code test reviewing, we should always check for plagiarism, especially because certain code test questions have been used for a while. A green tick means ‘haven’t find any similar solution from the Codility database’, but a yellow warning normally means the candidate's code test might be plagiarized (keep in mind it could be a false alarm as well!).


We can spot whether a candidate has plagiarized by their pattern of behaviors easily. If you go through the task timeline on top of the code, you can spot where they pasted in snippets of code and then try some other snippets, like a developer trying to decide which keyboard to buy! A candidate might've tried many different versions of code but none of them worked 😕 (colored triangles indicate suspicious behaviors).


In the timeline you can spot where a candidate actually spent time typing in the solution line by line. If the candidate pasted in a whole chunk of code in one go, and this happened for multiple solutions, they could be doing coding in their own IDE (Integrated Development Environment) and pasting it in. There isn’t a hard line that says 'this is definitely plagiarism', unless it is really obvious.


If it


*is*


pretty obvious plagiarism, the candidate is very likely to sink. I would give them a ‘No Hire’ mark to save other people’s time.


But, when we are not sure if they are cheating, a second input from a phone screening is a good idea. Generally, we'd draft our concerns in the review report and maybe give them a ‘Neutral’ for a second round. If they are cheating, they will likely not perform well in the technical phone screen.


Once the candidate pass “moral test”, now we can examine their codes!


### In Code, What Are We Looking For?


There are three perspectives I evaluate in a code test review, including- logical thinking, coding style, familiarity with the language in order of importance from high to low.


## Logical Thinking


Logical thinking includes the main stream of code logic, edge cases covering and error handling. This is the skill that needs the longest time to train. It would be very expensive to hire someone without good capability in logic thinking as a developer.


For graduates, I wouldn’t be expecting too much for extreme edge cases or error handling. I do expect to see a clear approach to solve the problem. Candidates could be a little bit messy in coding (such as using bad variable names like a,b,c), or maybe don't know some less common packages in Java, but I do expect to see they can solve, or, at least, have good direction to solve the problem on a pure logical level.


I must admit, there are definitely gold diggers in job market who studied computer science because “that’s where the money goes to”, quote from the local butcher. But if they’re not able to think like a developer, they're unlikely to be a good fit for my team.


## Coding Style


Someone's coding style is a good demonstration of their personality. It shows how much a person has attention to detail. This includes naming, encapsulation, coding convention, comments, etc.


I personally do have a preferable flavor in coding, but when I’m evaluating the candidate’s code, I am only looking for consistency.


We shouldn’t be judging our candidates by if they’re using;


1. if () {


or


1. if ()


2. {


Different styles are trainable. We would never shut the door to candidates just because they put the brackets in the 'wrong' place! We look for consistency in style, which means the candidate is aware of their coding style and know they need to follow it.


If someone is using variable names like a,b,c or writing confusing logic without any comments, I would think they might not have the readers on mind when coding. This could either caused by a lack of team work experience or they don’t really care about their readers. Either way it can raise alarm bells, therefore I always mention it in my report so other interviewers can pay more attention to see if the candidate is a team player or not.


## Familiarity with The Language


Of course, it would be nice to have someone who has tonnes of experience coding in Java already, rather than someone who thinks like a Python developer in a Java project. But I do think this is only a nice to have rather than a must-have. What's important is that candidates can demonstrate that they can think in an OOP (Object-Oriented Programming) way.


A candidate who can write a HashMap impresses me much more than a candidate that knows how to use it. If they use fancy packages like Collections, PriorityQueue, that often tells me that they have in depth exposure to Java (or advanced Google skill 😂). But if they can solve the problem without using fancy packages, that will work just fine too.


---


I approach all questions in the test review using the above three perspectives, and write comments about each. Then I evaluate my own comments and make a decision on the candidate.


Any decision that pops up in my brain before this time should be and shall be merciless ignored.


We could even do math on this. In the table below, we have a candidate who scored 21 out of full score 30. We are using a simple analytic hierarchy process (AHP) model here to make the decision, where total score = weight of each criteria multiplied by criteria score. So in the below example, our candidate scored 21 (3*5+2*2+1*2) out of full score 30. This will be at 70% which is a decent candidate overall.


Another candidate with following scores (below) will be at 50%, which sounds more like a ‘Neutral’.


I want to mention again that this is a personal experiment, and the weight is based off my own importance ranking. Scores could be further detailed into documents to aid transparency. The general idea behind the scene is to remove yourself or your personal opinion from the code review and only keep the professional ones.


### To Summarize


This has been my personal thoughts so far in code test reviewing. Overall, I think it’s very important to set the same evaluation perspective for all candidates for fairness. We all have bad days, but we can’t let a bad mood evaluate candidates. We have all been through the anxiety of interviewing with companies we like, so, let's allow the code do the talking in a code test, nothing else.


**


*Want to work alongside Xinyue and the Guidewire CAPS team? We're hiring for multiple roles across the globe,*


[view open roles here](https://careers.guidewire.com/jobs) *.*


[See All](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
