---
schema_version: "1.0.0"
document_id: "0a2d06829b8ce8a9b899b5a2b3bc71a59df4955df8604c16f9a350b6d49f7d9b"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-sean-law"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:35ac5945f681846e698090358ea613ae54512f85e5d4b90ce0bee84fe68500a2"
---

# Once a Maintainer: Sean Law

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[Sean Law](https://seanlaw.github.io/) , creator and core maintainer of[STUMPY](https://github.com/TDAmeritrade/stumpy) , a powerful and scalable Python library for time series data analysis. Sean and team recently released STUMPY 1.13.0, which includes an easier to use matrix profile data structure, NumPy 2.0 support, pyproject.toml adoption, Python 3.12 support, and improved documentation and testing. Sean is currently Principal Data Scientist at a Fortune 500 finance firm.


**What was your first exposure to open source software?**


I started off in the early 2000s, working as a scientist and then eventually going on to grad school. But I happened to get lucky and worked in a field called computational chemistry. If you've ever heard of the protein folding problem or a company called DeepMind, that was the area of expertise that I was in. So even though I'm a biochemist by training I'm actually more of a computer person. We were working on computer simulations of how DNA, proteins, and RNA interact with each other. It was 2013-ish when our academic grandfather won the Nobel Prize in chemistry for producing the earliest biomolecular simulations. What people refer to as data science today, I kind of half jokingly say that we just call it “doing science”. Certainly data has to be a part of it, and it's finding the right tools or sometimes making up your own tools to tackle the task at hand.


Originally I programmed a lot in Fortran and then Perl, and eventually C++ and Python. I would argue that for scientific computing, Python was not really the tool of choice until like the late 2000s, around 2010. One of the toughest parts was package management. Until package management tools like those developed by[Anaconda](https://docs.anaconda.com/) , and then later[conda-forge](https://conda-forge.org/) made things much, much easier, people didn't really look at Python, especially in the data space.


**Did you foresee writing any open source software yourself at the time? Or was it more happenstance?**


The short answer is “no”. I don’t think anyone goes into it expecting that they’re going to do open source. When I started doing software development especially in academia, it was more of a means to an end. You didn’t write code in anticipation that someone was going to use it. You wrote code to do the quick and dirty analysis without any unit testing, but it works. And then maybe a year later you come back to it because a reviewer of your paper asks you a question and you need to go back and look at it and you've probably forgotten what you did in the first place.


But then around 2010 when GitHub started to become more popular they made the developer/code versioning experience much nicer. I think it was around 2011 when I even considered taking some of my academic code and open sourcing it, putting it on places like GitHub or even PyPI so that other people could consume it. But in those days you weren’t thinking that a lot of people in the world were actually going to use it. You’d be happy if twenty years later, 10 people downloaded it. So as part of my postdoctoral training I decided that as a sort of companion to publishing the paper I also provided some code to reproduce the work. And I wanted to contribute back to the scientific community.


**And that was also in time series data analysis?**


No, the work back then was the earliest stages of applying a more novel or sophisticated approach to predicting protein secondary structure using machine learning. Also I built a package for analyzing simulation data. But again, very limited usage and adoption. This was right around the time that I decided to leave academia and move into industry. And so my pursuit of it was never really about people using it, but that hopefully it could inspire somebody else’s work as a starting point more than anything.


**Great, now let’s talk about STUMPY. First, do you pronounce it “stum-pie” or stumpy, like the word?**


I’ve learned from Travis Oliphant (NumPy) that we shouldn’t spend time arguing about these things, so I try not to correct people but personally, because the word stumpy is in the dictionary I naturally gravitate toward that. But some people pronounce it “stum-pie” and that doesn’t bother me.


**How did STUMPY come to be?**


So at the end of 2016, a pair of research labs at the University of California, Riverside and the University of New Mexico published two back-to-back papers detailing this concept called a[matrix profile](https://matrixprofile.org/) that, once you can compute it, will allow you to perform a variety of time series data mining tasks. And then within the papers, they presented a couple of algorithmic as well as algebraic improvements that allow them to generate a matrix profile really, really quickly. And being somebody who had worked with time series data for a very long time, I was very skeptical at first. And because of the nature of my work I spent time reading through those papers and validating whether it was snake oil or whether there was something there. It took me about a month to develop a basic implementation based on some pseudocode. And my first few attempts, I was like, oh, something's funky here. I’m not getting the right result.


If I implemented it naively, I knew what the answer should be. It's pretty straightforward. But to implement the high performance version of it, there were some oddities. So eventually I went back to the original authors and was like, hey, I'm getting stuck on this part. And they were like oh yeah, there's an error in our pseudocode. An off by 1 error (but their actual MATLAB implementation was correct). Once that was clarified, then everything became unlocked. This published research was indeed much faster than what a naive person would have implemented. And so I thought there's probably something there. And then I tried it on slightly larger data sets and things started to slow down.


So I tried to leverage some of the PyData set of tools, in particular[Numba](https://numba.pydata.org/) , which is a just in time compiler that takes your Python code and compiles it to much more performant and faster machine code. And after trying it out, it seemed to scale very nicely. At that point I knew there was something there. There are some significantly tricky bits to implementing the performance version but I thought as a society, we shouldn't be reinventing the wheel, right? And that's when I decided to open source it.


So it was around April or May of 2019 when we open sourced STUMPY. And at that point it was more of a nice thing to do without any expectation that people were going to use it let alone contribute to it.


**How did you think about those first couple of contributions or those first couple of issues as they came in, from a human perspective?**


I think there are probably different camps. But I think for myself having seen a lot of successful packages being open sourced, I had a reasonably good idea of what I wanted to aspire towards. Before we open sourced it we made sure that we had 100% code coverage, which is a high mark to maintain. We opened it in 2019 and five years later, we still have 100% code coverage. So everything that we add and that we've added since the beginning is heavily tested. In fact, we probably have more unit tests than we have functions.


From day one, if I'm the single person maintaining this, I needed to do everything I could to make my life easier. Just having unit tests run every time that there are contributions certainly has saved myself and everybody a lot of headache and this decision has served us very well. Now that also has its challenges of making the bar to contributing a little bit more challenging, if people don't have experience writing tests. But that's where the human side comes into play. We have to have a willingness to serving our community and guiding contributors who want to learn how to write unit tests. We need to remember that people genuinely want to contribute, and they have taken that first step, and that first step is really hard. And when you recognize that, then it's humbling to realize that somebody's willing to spend their time on your project.


For example, very early on STUMPY didn't even have a workflow or CI/CD for testing as new commits and PRs came in. So I created an issue for it, because that world was completely new to me. Then a few days later somebody said, “Hey, I'd like to help you.” It was somebody I didn't know that was in Australia. And when I was sleeping, they were working on it. And very quickly the magic of open source allowed us to have a regular pipeline for automated testing. And then when they were done, they handed over the keys, and they vanished. And I was just like, wow, this is a side of open source that people don't get to see. It inspires me to know that good people like that exist in this world and it inspires me and keeps me going as a maintainer.


**Do you have a core team of people that manage the roadmap, or are you still primarily doing it all yourself?**


Today, I'd say that at least 50% is by myself. Earlier on we had a contributor from Germany who was pretty active and then they stopped contributing once they graduated from school. But more recently we added a new core maintainer,[Nima Sarajpoor](https://github.com/NimaSarajpoor) , and, together we've been thinking about how to improve the performance of STUMPY. This really requires a deeper understanding of how the package itself is designed and so it's really mainly two people running STUMPY. But again, because of some of the proactive approaches we've taken and a lot of the automation that we've done beyond adding features and improving what currently exists, there's surprisingly not a ton that we need to do.


**In terms of focus for the next year or two, would you say that improving performance is where the focus is?**


Yes, I think that’s always the case for us because that’s what STUMPY is. It computes something that if you did it in a brute force way would take forever and with some better algorithms and compilers we’ve gained a lot of benefit. Even knocking 20% off the computation can drastically improve the user experience. I think it was[Leland McInnes](https://github.com/lmcinnes) , who created packages like[HDBSCAN](https://github.com/TutteInstitute/fast_hdbscan) and[UMAP](https://github.com/lmcinnes/umap) , that likes to remind us that as a data scientist, there are different bins of time, right? There are tasks that take the time to go grab a cup of coffee, and then come back and it's done, to go grab lunch, and to go to bed and what we're all trying to do is to always move the process up to the earlier levels to eventually get to interactive time scales, where you hit enter to execute some command and it's just done so that you don't have to spend time context switching. And that's what we're always striving for. In one of our recent commits we actually did improve our CPU computations by about 15 to 20% which is very, very rare when you're talking about code that is already performant. In fact, we think there's more juice to be squeezed from this orange.


**How do you track that? Like can you say we’re twice as fast as we were in 2019 when we came out? I know that that's a complicated question because it might be fast on hundreds of machines or fast on a single machine or fast on a CPU, fast on a GPU, there's a matrix of the places where people want to run your library.**


What we can't do is rerun it on the hardware that we originally tested on. All we can really do is run it on multiple different types of hardware and operating systems using the previous version of the code and then the changed version of the code, but also try to be a bit more meticulous in terms of identifying the precise line or lines of code that had the most impact. As a reformed scientist, I'm usually very, very skeptical when people claim that there's a 90% improvement in this machine learning model - like sure, right. Trust but verify is sort of the name of the game.


So we loosely look at performance, but at the same time, what matters more than performance from an open source standpoint is usability - the user interface, the API, how simple, how familiar it is. We often think about STUMPY as aspiring to be what NumPy is to numerical computing. What is important about open source too, is fighting the urge to be everything for everyone and to realize that if I build the software in such a way so that it is modular and composable, then people can build on top of it like they did with NumPy and SciPy or even pandas for that matter.


**Have you gotten any input from the community where you thought wow, I couldn't have imagined that someone would have taken it and done this with my package?**


Yes. I would have never imagined that people were using STUMPY at CERN, the Large Hadron Collider. That surprised me. When we open sourced this package, we also published a very short[article](https://joss.theoj.org/papers/10.21105/joss.01504) in the Journal of Open Source Software, JOSS. Mostly so that we could get a sense of what people were using the software for, at least in the academic world. When people cite that paper, we get a glimpse into this. We'll see people using it for looking at energy/electricity usage, applications in particle physics, and even people using our package for sports analytics. There continue to be a lot of fascinating opportunities and I think we can safely attribute this to the fact that we have created a package that is general purpose and isn't hard coded for any particular field.


‍


‍
