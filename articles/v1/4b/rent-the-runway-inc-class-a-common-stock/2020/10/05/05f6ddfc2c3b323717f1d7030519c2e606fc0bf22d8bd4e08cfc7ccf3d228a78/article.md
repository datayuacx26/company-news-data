---
schema_version: "1.0.0"
document_id: "05f6ddfc2c3b323717f1d7030519c2e606fc0bf22d8bd4e08cfc7ccf3d228a78"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/woke"
published_at: "2020-10-12T15:51:00+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:65838a825d17258a41425d93905abbf2148282aec8872a9c9b10c3417e853eed"
---

# Adopting Inclusive Language in Tech at RTR

*Rent the Runway Engineering is using a new open source tool called*[woke](https://github.com/get-woke/woke) *in our code review process to help ensure we are using inclusive language in all our code.*


##
HOW RTR IS TAKING ACTION


We at Rent the Runway are committing $1M to black designers and pledging that 15% of our marketing, ambassadors, content, and talent in front of and behind the camera will be black talent because representation matters. We are taking thoughtful action to support long term change for the Black community and in the industry of which we are a part. We have built a formal Diversity & Inclusion competency within our HR department and are building a comprehensive Diversity & Inclusion strategy that includes company-wide training, recruiting initiatives, employee feedback, and other steps to embrace and celebrate differences.


You can read more about our commitments at[https://rtrshift.com/jenn-hyman-rtr-black-lives-matter/](https://rtrshift.com/jenn-hyman-rtr-black-lives-matter/) .


## WORDS MATTER


As a female engineer, throughout my career, it has not been uncommon to find myself in meetings where I am the only woman. Recently I've started to notice just how often I hear certain phrases along the lines of "Okay guys, let's get started" or "talk to you guys later", regardless of the gender makeup of the meeting. Today, the gendered term "guys" has become interchangeable with "everybody" and can easily be disregarded. It is particularly poignant in these meetings when I am the only woman. In hearing these phrases, I feel invisible, as if my presence has been forgotten or ignored. I acknowledge that this is not the intention people have when using this term, but nevertheless, it is alienating.


The fact that phrases like these are so commonplace in our daily conversations, to the point where we don't even realize we’re using them, is part of the problem. "Guys" is just one example; within the technology industry as a whole, terms such as "master/slave" to describe database relationships, or "whitelist"/"blacklist", which uses "white" to signify something that is allowed and "black" to signify something that should be blocked, are racially charged and problematic.


We, our colleagues, our friends, have to hear these terms on a daily basis and feel the pain, anger, or sadness that is associated with them, but it doesn't have to be that way. These phrases all have perfectly viable alternatives that are not offensive, insensitive, racist, or sexist. Although there is undoubtedly effort involved to make these changes, it is worthwhile if it means creating a more inclusive, positive environment. The effort needs to start sometime. Why not now?


## CULTIVATING INCLUSIVITY


If we want to cultivate a more inclusive environment, we all have a responsibility to each other to look inwards, at the words we say and write. We need to hold each other accountable and push each other to be better. Language is one of the most powerful tools we have as humans—it connects us, and we need to use it in a way that makes everyone feel included and welcome. Making a commitment to use inclusive language is asking us to consider the implications of the words and phrases we use, to have empathy for others, and to imagine an experience that is not our own. It is a small but powerful change.


I don't claim to be an expert at language, I am an engineer. But I do know the impact that these phrases have on me, and I can only imagine the impact they have on others. We don't need to be experts to be empathetic and supportive.


## TAKING ACTION


With the murders of Black men and women such as George Floyd and Breonna Taylor, and with the ensuing “Black Lives Matter” protests and the call for racial justice, the tech industry is finally taking a long, hard, public look at ways we can combat racial discrimination. One important issue that came under some much-needed scrutiny was the terminology we have adopted and continue to use to this day. We are still regularly using terms that should have been removed from our codebases and our conversations a long time ago. The push in the tech industry has really been inspiring, where we’re seeing companies like[Apple](https://developer.apple.com/news/?id=1o9zxsxl) ,[Twitter](https://twitter.com/TwitterEng/status/1278733303508418560) ,[Google](https://chromium.googlesource.com/chromium/src/+/master/styleguide/inclusive_code.md) , and[GitHub](https://github.com/github/renaming) take real, concrete action towards ridding themselves of this offensive, outdated phrasing.


When conversations on the topic of adopting inclusive language within Rent the Runway Engineering's team began, (thanks to Fabiana Scala) I was inspired! I started thinking of ways I could contribute. I was interested in finding a way to maintain our commitment to using inclusive language long-term. While giving a few engineers a sprint or two to go through all our codebases in one shot would be a great step forward, is it scalable? How could we realistically maintain that cadence? How could we ensure that *everyone* would maintain and contribute to this commitment? I wanted something more sustainable, something that let engineers know immediately when they committed code that used insensitive language.


> “
>
>
> woke is a text file analysis tool that detects non-inclusive language in your source code. ”


[https://github.com/get-woke/woke](https://github.com/get-woke/woke)


I came up with the idea for[woke](https://github.com/get-woke/woke) ** when a colleague mentioned that a friend at another company received an automated comment on a Pull Request that they should avoid *dummy value* and instead use *placeholder value* . This started my search around the open source community for a tool that would accomplish something like this. When I came up short, I decided to build it myself. I created *woke* to fill this gap, providing continuous feedback when insensitive language is used within source code. *woke* is also an open source project, and contributing to the open source community is very important to me. I wanted this tool to be available for anyone to use. The more *woke* is used, the more inclusive the community and the tech industry as a whole will be.


A little about *woke* ... it’s a tool that scans your source code to find usages of insensitive, non-inclusive language and provides you with alternative suggestions. You can configure *woke* to run on your Pull Requests ( *woke* has two GitHub Actions and it can easily be integrated into any CI pipeline), or locally as a pre-commit hook, to make sure that all the code your team is committing lives up to the commitment you made to adopt conscious language. There are other common linter features, such as ignoring files, in-line ignoring, rule customization ( *woke* comes with a set of default rules, any of which you can disable, or you can create your own), and changing the exit code when violations are found (if you want to notify that violations occurred, but don’t necessarily want to prevent code from being merged).


It's common that the people who are least impacted by or unaware of insensitive language expect those who are most impacted to speak up or educate them. This puts a lot of undue pressure on those who are impacted, when it should be all of our responsibility. *woke* helps by moving this burden off the individual to suggest or enforce inclusive language and onto an automated check. Just like other linters, where the goal is to promote best practices and avoid common bugs, *woke* removes individual opinions, making the review process less about personality or ego, keeping the focus on content.


I had a ton of fun building *woke* and am so excited to be able to share it. And since it’s an open source project,[contributions](https://github.com/get-woke/woke/blob/main/CONTRIBUTING.md) are[welcome](https://github.com/get-woke/woke/blob/main/CODE_OF_CONDUCT.md) !


## WOKE AT RENT THE RUNWAY


Rent the Runway has always been a diverse company and supported diversity and inclusion, it's one of the reasons I wanted to come work here. But just because we value diversity doesn't mean we don't also have more work to do, and we are committed to doing the work! Once I started working on *woke* in my free time and passed my idea around to a few people, I was thrilled that there was interest from tech leadership to roll this out across our git repositories.


Read more about how Rent the Runway as a whole is taking action at[https://rtrshift.com/jenn-hyman-rtr-black-lives-matter/](https://rtrshift.com/jenn-hyman-rtr-black-lives-matter/) .


**Within RTR Engineering:**


-


**We will be rolling out *woke* to run on all Pull Requests for all active repos**


-


We will be following GitHub's recommendation and will be moving away from the *master* branch convention.


-


We have committed to using new terminology within our code, documentation, and communications.


By integrating[woke](https://github.com/get-woke/woke) into all of our active repos, we are committing not only to an inclusive, welcoming work environment, but to ensuring this environment persists over time.


Will changing these terms to better alternatives solve decades of discrimination, sexism and racial injustice? No. But discontinuing their use is a great step forward. It signals to others, our colleagues, our friends, that the language we choose to use is important, and is of inclusiveness, empathy, and respect.


#BlackLivesMatter //[VOTE](https://www.vote.org/)


Want to work at Rent the Runway?[We’re hiring!](https://boards.greenhouse.io/renttherunway)
