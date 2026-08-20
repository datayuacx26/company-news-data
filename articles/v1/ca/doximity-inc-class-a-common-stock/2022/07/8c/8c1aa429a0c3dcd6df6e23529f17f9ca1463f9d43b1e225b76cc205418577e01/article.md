---
schema_version: "1.0.0"
document_id: "8c1aa429a0c3dcd6df6e23529f17f9ca1463f9d43b1e225b76cc205418577e01"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/code-review-is-hard-work-if-you-do-it-right"
published_at: "2022-07-15T14:06:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:04cde623c960ee20933444efee45aaac2e26ea3654af6125b736055d7c9dc52a"
---

# Code Review Is Hard Work! (If You Do It Right)

We write a lot of code at Doximity, and every line that we ship to production gets reviewed. We spend a lot of time on code reviews, therefore it's wise to reflect on our process.


Everyone has their review process, but it all boils down to the three buckets that GitHub provides:


- Approve
- Comment
- Request Changes


But when should we choose "Request Changes"? That's often a tough line to draw. Sometimes, it even results in difficult conversations. Let this post serve as a framework for making that decision.


## The Easiest Part: Automated Tools


Rely on tools rather than human beings whenever possible. At Doximity, we use[Rubocop](https://rubocop.org/) and[Danger](https://danger.systems/ruby/) for our Ruby applications.[Grammarly](https://grammarly.com/) is great for prose. Automation is faster (and cheaper) for certain types of code review. There are arguments for and against consistent code style in an application. Recently, we've arrived at splitting out style configuration from best practices that we wish to enforce to reduce the amount of bikeshedding over how to break a statement into multiple lines, for example. When enforcing best practices in Rubocop and Danger, the author of a pull request can easily tell when there's something important to address. Ideally, the author will resolve all automated reviews before involving their colleagues. (However, keep in mind that not all automated reviews are actionable. "Resolve" may simply mean leaving a comment about why a particular notice is appropriate to ignore.)


That's the easiest stuff, though, and if code review could be completely automated, this blog post (and perhaps our jobs!) wouldn't exist.


## The Easy Part: Approving Without Comments


Sometimes, a change is very simple, such as a copy change. If that's the case, it's a great time to simply approve and say "LGTM!"


## The Harder Part: Approving With Comments


Every developer is at a different point in their journey. While reviewing, some code may not be ideal and have clear room for improvement.


Approving with comments is a good way to teach. Call out best practices that aren't yet automated and changes to improve readability. This applies to an author of any skill level but is especially important for those with less experience.


That said, don't let "should do" comments block forward progress. Approve the pull request and move on. The author can choose whether or not to accept the suggestions. The author has final editorial control, and the goal of the review is **not** to rewrite the PR as if the reviewer had written it originally. It's worth keeping in mind that not every comment is a "must fix."


If any of the comments are truly important, the next section applies. Such comments must be clearly marked to help distinguish their importance.


## The Hardest Part: Requesting Changes


Now comes the hardest decision. Should changes be requested?


A lot of the time, requesting changes isn't necessary. As a reviewer, think before choosing the button that makes a big red mark show up. Also, consider reaching out over chat, voice, or video. Sometimes there are just misunderstandings that can easily be resolved by talking for a few minutes.


However, that choice is there for a reason. It blocks the code from merging even if other reviewers approve. By requesting changes, the reviewer is stating, "this code should not deploy." Sometimes, that can be an uncomfortable choice, but with the right mindset, it can become an opportunity for teaching and increasing the overall health of the team. In the end, everyone is responsible for the maintenance of the code and shares in the ownership of it.


If the reviewer does request changes, they should be sure to call out which particular comments are blockers and why. Emojis can help make this more obvious at a glance. Personally, I use "🚫 *blocker* *because…"*


There are many valid reasons to request changes on a PR, so this is not an exhaustive list but rather a set of examples.


##### The Feature Isn't What Was Intended


This is one of the simplest reasons to request changes. Sometimes, there was a miscommunication or misunderstanding, and the code in the PR does something that *sounds like* the request but is actually wildly different. Other times, there are simply errors in the code, including bugs.


State the mismatch and request changes. In this case, it's often best to ignore other potential concerns, as they may disappear as the feature is reimplemented.


##### Possible Downtime


As we accrue experience, developers can notice situations that might incur downtime. These are often unobvious and would only be noticeable in production, running at scale rather than during development or QA. A canonical example would be long-running migrations.


Describe the problem and link to a relevant post-mortem document or wiki page. If none exists, write one, even if it's a couple of sentences with a link to the PR that's currently being reviewed. Over time, a library of writeups will develop. Consider further whether automation (such as Rubocop and Danger) would make sense and add a ticket to prioritize its implementation.


##### Poor Performance Characteristics


A close cousin of possible downtime, unoptimized performance can also be difficult to notice in development. However, there are acceptable tradeoffs for poor performance if the code in question is rarely used and is not expected to grow in usage.


Before requesting changes for performance concerns, spend some time evaluating the tradeoff and providing meaningful data to help in making a decision.


##### Design Flaws


On the theme of application environments, some design choices work fine for development and production but don't work for automated tests or staging environments. For example, the code may be simple enough to understand but is very difficult to write an automated test for. The design in the pull request may make multiple staging instances of the application infeasible. (This is particularly common with 3rd party services that rely on postbacks.) Special snowflake instances (i.e., the only one that works) should be avoided as they will slow down the team. Sometimes, certain design flaws are unavoidable, but they only become more expensive to fix after other code becomes dependent on them, so it is often best to address them before they are deployed.


Suggest solutions, or request more information about what the constraints are. For instance, a postback URL could be configured at runtime, or a webhook could be configured during deployment.


##### Security Flaws


Depending on the severity, a security flaw in the software is an immediate concern to flag during a review. One good approach is calling out the concern with a question: "what would happen if a user were to…" Consider including your security team if you have one.


Include a link to a write-up, and if none exists, write one (even if it's short).


##### Inappropriate Handling of Sensitive Information


Similar to a security flaw, sometimes data is not being handled properly. Depending on the industry, there may be compliance guidelines to adhere to. Doximity, in particular, must be HIPAA-compliant.


Again, provide a write-up on the best practices when a compliance violation is found.


##### Inappropriate Exposure of System Internals


Separate from sensitive user data is information about the production system. Often, habits from simpler projects can find their way into a pull request. For example, UUIDs are preferred over auto-incrementing primary keys.


Call out these situations and provide the reasons that it's not an acceptable solution for the application.


##### Choices That Are Incompatible with the Technical Direction of the Product


This is dependent on the tech lead for a team (or even the larger organization), but depending on the familiarity of the author with the architecture of the system, it may not make sense to take certain approaches.


Consider whether the choice is appropriate at the given time. If it's easy to change later, it may be best to move forward with the decision as it stands. If it clearly conflicts with other ongoing work, it may make sense to request a change.


##### Lack of Test Coverage


In some instances, developers with the laudable goal of moving fast skip adding any automated test coverage. This can sometimes be difficult to notice, as it's easier to focus on the *contents* of the PR rather than what it's *missing* . Ideally, test coverage metrics can make this obvious. Again, sometimes this is acceptable (e.g., for a prototype), but more often than not, it's good to be in the habit of writing tests for code that will reach production. Over time, the lack of automated tests will make *any* modification a challenge, so test coverage is best to ensure that new features will be able to be safely added.


Call out the need for tests. Watch for parts of the system that are particularly hard to test and consider how they could be designed to be more easily tested.


## When Changes *Shouldn't* Be Requested


There are a handful of situations that might initially appear to be reasons to request changes but, upon closer inspection, are not.


##### Differences in Opinion


The author and reviewer may not always be in agreement. Sometimes facts and opinions can get mixed up. Generally, the author gets final editorial control over what ships.


Ask whether the code in the PR isn't shippable for a reason like the previous section. If there is no true blocker, leaving a suggestion is fine, but it shouldn't hold up shipping. Approve the PR and move on.


##### The Situation Doesn't Call for It


Flaws can be acceptable. The previous section hinted at that fact. Here are some additional situations to consider:


- **Time to market:** Right now, the goal is to ship, and the flaw is an acceptable tradeoff. Consider documenting the flow in a TODO comment or a new ticket and re-evaluate later. (The offending code may not even exist in 3 months.)
- **Unlaunched features:** There is wiggle room before a feature is launched. As long as there is agreement about what will happen next, it may be best to move forward with the code in the PR.
- **Small user base:** If the feature is only used by a handful of people, many tradeoffs become acceptable.


##### Less Clear: Bad Precedent


Sometimes the tradeoffs make sense in one situation, but future developers aren't aware of that context when making other decisions. It's possible that they will cargo cult a decision that doesn't make sense in a different situation.


Call out the context in a code comment and list why it's an acceptable decision so that future developers are aware. Recall that GitHub comment threads are not discovered organically. Finally, weigh the cost of "doing it the right way" versus the risk of setting a bad example.


## Focus on What Is Going Well


After reading this blog post, it may be easy to fall into the trap of listing everything that's wrong with a pull request. As a reviewer, don't forget to call out all the great things the author did. A call out as simple as an emoji can go a long way and other words of encouragement are, of course, great!


Remember that the developers involved are human beings and humans make mistakes. Follow the golden rule: leave the types of comments you'd appreciate receiving yourself. (After all, the reviewer and reviewee switch roles very often!) It's best to be humble as well. When a mistake is made (as either reviewer or reviewee), note it and rectify it.


In the end, maintaining a codebase is a shared responsibility. The review process is an opportunity to learn for all involved.


## Other Resources


- [Playbook for software design and development by thoughtbot](https://thoughtbot.com/playbook)
- [Code Review Developer Guide by Google](https://google.github.io/eng-practices/review/)


## Acknowledgments


Thank you to all the kind people at Doximity who reviewed drafts of this post, including Tianyuan Cui, Samus Gray, Rob Malko, Bruno Miranda, Brent Redd, and Emiliano Zilocchi. I appreciate your feedback and support!


Thank you to[Rails Guides](https://guides.rubyonrails.org/) for sample code used in illustrations.


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you'd like to be notified about new blog posts.
