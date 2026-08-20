---
schema_version: "1.0.0"
document_id: "ee125f87b32c6842cdc66adec1fb1350d9611e763624c346a4baab62264b63d7"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-william-woodruff"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:3d942d4e2509f61162cb7329c0705a47ace8bced74a46fec37a7c1c28176fcf3"
---

# Once a Maintainer: William Woodruff

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[William Woodruff](https://github.com/woodruffw) , contributor to[Homebrew](https://github.com/Homebrew) ,[PyPI](https://pypi.org/) , and creator of several open source tools including[zizmor](https://github.com/zizmorcore/zizmor) , a static analysis security tool for Github Actions. William is currently Engineering Director at[Trail of Bits](https://www.trailofbits.com/) , a security research and consulting firm in New York.


**How did you get into coding?**


I don’t actually have an academic background in software, really. My degree is in Philosophy. I got into it as a hobby when I was a high schooler. I had a computer and I wanted to run different software on it. And I found Homebrew back when it was like a build from source package management ecosystem. I had a lot of fun compiling things on my machine and installing a bunch of random languages, which I never really used. So I began to contribute to Homebrew and make patches for the formulae that it was using. And then when I went to college, I had nothing to do one summer, and I applied to[Google Summer of Code](https://summerofcode.withgoogle.com/) . I did that for two years with Homebrew and I think it took me from sort of a passive interest to really actively doing software engineering.


**You mentioned installing a bunch of different languages at first - when did Ruby enter the picture for you?**


Ruby was the first full-fledged language I think I wrote software in. I’d written some Perl and Python before, but those were like little scripts.


**After graduating college, did you go right into software development?**


The company I work for, Trail of Bits, they hired me right out of college. And pretty much since then I’ve done open source. I’ve done a bit of proprietary stuff, but most of my career has been in open source.


**Awesome. So what would you say the structure is like, working on open source from within the umbrella of a company?**


Yeah, as I’m sure you’re familiar with, the incentive structure for open source is very complicated. Like if you're a big company, the incentive is to extract value from open source, but not necessarily invest in it unless it's a direct selling point. But Trail of Bits is a pretty small company. One of the nice things is that the incentive structure gets inverted a bit at that smaller size. Because we're a consultancy, we can actually sell our expertise to larger companies who do need to invest in open source. And so I look after an entire team that does pretty much full time open source engineering. It's me and about five others who are actual day engineers as well as a couple of project managers working on Homebrew, but also RubyGems. A little bit of standards work there, and a bit of standards work in the Rust ecosystem. And also one of our really big areas is the Python ecosystem. So we do a lot of Python security engineering.


**What was the inspiration behind zizmor?**


Zizmor is a side project of mine. It started because for about the last five years, I've worked on the Python package index. I'm not a maintainer of it, but I've contributed a lot to it professionally. And so I built up all these security features with the help of a bunch of really fantastic people, the actual maintainers of the project. And as part of that, I noticed this trend that's happening in open source, which I think is ultimately for the good, but has some significant downsides, and that is this push towards putting things into CI/CD platforms. So things like GitHub Actions are alluring because you no longer have to have all this development state locally. You can sort of push it and compartmentalize it into a platform. But the downside to that is it’s a black box. You sort of throw your code and your build steps into it and pray you get functional and integral build products out of it. And so, you know, I believe pretty strongly that a core part of being a security engineer is not just trying to get people to do the secure thing, but meeting people where they are. People are going to use Github Actions and Gitlab’s CI/CD, whether or not I think the fundamentals are secure or not. And so the question is how to make those things as secure as possible.


So similarly with the PyPI stuff, we built this feature called 'Trusted Publishing’, which basically allows credential-less authentication between Github Actions, Gitlab and PyPI. You don't need long lived API tokens anymore. And that got me thinking like, well, how much do I actually really trust the security of the average Github Actions workflow? And I started looking into ways to statically analyze Github Actions workflows and action definitions to see whether or not my assumption that this was a more secure by default posture was well founded. And I don't know. I think the results are mixed. I think the answer is that you can write very secure Github Actions workflows, but by default Github Actions exposes a really large number of footguns that have recently led to some very high profile breaches in the last couple of months.


**What are the kinds of risks you’re worried about? Like I as a package maintainer, I'm trying to build a new version of my package. It's being done on GitHub Actions and the resulting build is not what I expect it to be because I installed some wrong plug-in and it's malicious or something? Or someone else was able to publish a version of my package to PyPI because I didn't set up my Github Actions permissions properly and they were able to intercept it? That sort of thing?**


I think both of those are major concerns of mine. I am especially concerned about the case where I think everything is integral. I think I've produced a hermetic build within Github Actions, but in reality there's a cache poisoning vector or a trigger that I think only I can trigger this workflow. When in reality, anybody who submits a pull request can trigger this workflow with elevated privileges. So I'm really worried about that case where everything looks like it's going perfectly. Even if you use all these modern supply chain standards, things like[SLSA](https://security.googleblog.com/2021/06/introducing-slsa-end-to-end-framework.html) and[Sigstore](https://openssf.org/community/sigstore/) , which are supposed to give you attestations and strong evidence of a place of origin. But if the origin itself is compromised, then these attestations are only an attestation of malicious activity. They don't give you the protection that many people assume they give you. And so I'm really worried about that type of vector.


**I’m sure you get asked this question a lot, but as a security engineer, on behalf of your customers what do you prioritize? Because there’s so much that goes into cybersecurity, what do you start with to get your house in order before moving onto the next level of problems?**


Yeah I mean certainly with PyPI we’re now on year 5 or 6 of this collaboration. And we started with, as you said, get your house in order steps. The most basic one was that five years ago, PyPI did not have API tokens. You would authenticate PyPI with a username and password pair. So if you were say Google or Amazon, you had employees who had the keys to the kingdom for your entire namespace on the index or your entire account rather on the index. And this violates a principle of least privilege, right? It violates longevity guarantees around tokens. Tokens should ideally have mandatory expirations. If they can't, at the very least they should be identifiable and traceable and not entirely random. They shouldn't be user controlled credentials. And so the very first thing we did was add API tokens. So you know, they have global controls. That was a really basic baseline thing.


Then the question from there is, well, we know for a fact that users will still normalize deviance, they will still create global scope tokens that don't expire. So how do we give users a default path that doesn't encourage them to create non-expiring global tokens? And the answer to that was Trusted Publishing. And it had two factors so that people can't just log into the same credential. Then let's add this self-expiring, self-scoping mechanism. And then finally, now that we have this self-expiring self-scoping mechanism without identity control, you've got to do attestations by default. So that's where we currently are. And I guess that gets back to what I was saying earlier, I really love open source. I think it's amazing what we've built on platforms like GitHub and Gitlab, but I am very worried about this semi-autonomous machine that we've built that runs at all hours of the day. I'm really worried that one day someone's going to find something that no one else has thought of yet. And we'll basically have a new version, a much, much worse version of the[xz attack](https://en.wikipedia.org/wiki/XZ_Utils_backdoor) or[Heartbleed](https://en.wikipedia.org/wiki/Heartbleed) , one of these Internet-breaking attacks.


**Have there been any contributions to zizmor that made you think oh, I never would have thought of that? Something that really surprised you?**


Definitely. There've been a couple. I mean, I've had a couple of really fantastic contributors come in and submit audits that I had either not thought of or I was not thinking about structurally the way they thought about them. Like I was like, oh, you just sort of scan for this pattern and hopefully you'll catch the really bad things. But they built up the right machinery to detect, you know, malleability in the pattern and they rigorously thought through the problem whereas I had only a vague sense of what the check needed. That's been really nice. And also people have been filing issues for new audits. You know, people just want new features. That's natural. But I've been trying to burn those down.


**Do you have a formal roadmap for the project or is it more organic? How much would you say it kind of lives in your head versus managed by the community?**


Definitely it mostly lives in my mind at this point. I mean I track everything with Github issues and I have milestones for things I want to accomplish, but big picture things I'm not tracking anywhere but in my own way at the moment. It’s still only a six month old project. So it's mostly just me sort of feeling through where it needs to go. And eventually what a 2.0 release will look like because that'll be where I can begin to break things and try new directions.


**What are some other open source projects that you think are really interesting right now, or people in open source that are doing something interesting to you?**


There are a lot. I do a lot of work as part of my day job with PyPI and the Python package index maintainers. I think[Warehouse](https://github.com/pypi/warehouse) itself, the backend of PyPI, is a really fascinating codebase and it doesn’t get the kind of attention it deserves. It’s a really under-appreciated codebase given that it’s a monolith that controls the world’s largest by volume packaging ecosystem. And they do that with an almost entirely volunteer staff and the shoestring resources of a nonprofit foundation with a few grants. And it’s my opinion as someone who’s contributed to it, who isn’t a maintainer but who’s really read through a lot of the codebase, that it’s a really well architected and tested codebase that’s held up under a lot of unpredictable stresses over the years, like ways it had to evolve very rapidly or had to grow an entirely new feature surface which could not be predicted. I’m sure the maintainers there could talk much more intelligently than I could about those pressures. So that's people like[Mike Fiedler](https://github.com/miketheman) and[Ee Durban](https://github.com/ewdurbin) and[Dustin Ingram](https://github.com/di) . And then I know you’ve already talked to[Mike McQuaid](https://github.com/mikemcquaid) , who I consider a great mentor. He’s one of the first people who got me really into open source on a more serious level than just sending patches every once in a while.


**I have one more question since it seems like you have a breadth of experience across ecosystems. And different open source ecosystems to me have different cultures, like what the JavaScript community will create packages for versus what the Rust community creates packages for, etc. Do you have any thoughts on the way that different ecosystems are doing things from a security perspective?**


Definitely. The difference between them can be very legible at times. I would say that six years ago, if you had asked me that, I would have said that Python was pretty far behind in terms of their security practices. I believe at that point RubyGems and npm already had API tokens. And I believe npm had already enabled two factor authentication at that point. So I would say that Python back then was trailing the pack. And these days, I would say that Python is towards the head of the pack because it pushed so hard on these newer ideas similar to trusted publishing.


**Is there a community consensus on what it could look like if we didn't have the baggage?**


I think especially for Python, there’s a big wish list of things that could be different if only we had known 20 years ago to set aside conceptual space or standard space for this. I know especially with Python, a huge desire the community has, which is very hard to solve technically, is namespaces. npm had a pretty decent amount of community pain when they did it, but it paid off long term. Now there's been some discussion around Python standards for adding namespaces to PyPI and other indices, but there is some packaging that is so old that it's a really significant lift. That's one thing. The lock files are another thing that are really conspicuously missing from Python, unfortunately, in my opinion. And now there's[PEP 751](https://peps.python.org/pep-0751/) , which is the lock file standard. And I'm really hoping that sees more adoption over the next months and years.


‍
