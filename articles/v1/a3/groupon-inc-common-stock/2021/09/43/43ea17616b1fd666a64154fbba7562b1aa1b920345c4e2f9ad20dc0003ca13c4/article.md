---
schema_version: "1.0.0"
document_id: "43ea17616b1fd666a64154fbba7562b1aa1b920345c4e2f9ad20dc0003ca13c4"
company_key: "groupon-inc-common-stock"
company: "Groupon Inc."
source_id: "groupon-inc-common-stock-rss-99044b78d036"
canonical_url: "https://medium.com/groupon-eng/3-guidelines-to-reduce-implementation-flaws-958511e2a82f"
published_at: "2021-09-03T17:06:05+00:00"
first_seen_at: "2026-07-20T04:36:53.869647+00:00"
fetched_at: "2026-08-20T00:24:14.263767+00:00"
content_hash: "sha256:3948a7271b505307dc45c2e5eb31c64c79fc041c8c8fef98e04d9dc2d1ddba17"
---

# 3 Guidelines to Reduce Implementation Flaws

#### ***3 Rules/Guidelines to Reduce Implementation Flaws***


When I hear the word “rule”, I think back to grade school. “Be quiet when the teacher is talking.” “No interrupting, you must raise your hand with a question.” But what makes this different than following a set of guidelines? There was always that one kid who never listened to the rules. In coding, we may talk about rules, whereas in reality, we’re following some sort of guideline. Think of guidelines as a scene from the Ghostbusters movie when Bill Murray told Sigourney Weaver that he never sleeps with a possessed woman. Then she kissed him and he said, “Actually, this is more of a guideline than a rule.” A standard guideline is important because it helps improve the existing code quality, a guideline also makes it more readable and also somewhere sets the team culture.


Over the last few years of gaining more experience in coding, rules have changed into guidelines; however, there are three top guidelines I always follow. After being curious if other colleagues follow any specific coding guidelines, I reached out to several within Groupon and companies like Google, Morgan Stanley, Apple, Lutron, L Brands, Capitol One, GoPuff, and Vertex Inc. Specifically, I asked if they follow any guidelines that reduce implementation flaws. To not much surprise, we all follow similar guidelines.


Take a moment and ask yourself, do you have any specific rules or guidelines? We may all have similar guidelines that we follow, mine are listed below-


Guideline 1 → **Break down your functions**


My first rule is merely following the single responsibility principle. A function should be responsible for doing one specific thing. If a function has too many responsibilities, then it should be broken down into multiple smaller functions. Its logic is limited in scope, which makes it easier to implement and test its end-to-end functionality. A friend of mine once said that “If the function you end up unit testing requires tests for multiple separate pieces of logic, your function does not have one function then you should split it up.” The other benefit of such functions is it is easier to reuse in other parts of the codebase without having to rewrite the same logic and repeat itself. Rule number 2 will further explain this concept.


Guideline 2 → **Do not repeat and read your code**


Readable code is a good indication of properly structured/formatted code that better describes its purpose. It’s easier to follow and review; furthermore, readable code requires the code to avoid using any references in the codebase when not needed or used including comments. Why leave comments in the codebase when no test is going to complain about it when it eventually becomes outdated? Let the code speak for itself. Rather, add comments only where necessary for documenting complex logic since it can be hard to follow certain sections of the codebase. In conclusion, comments in some cases are helpful, in others they’re unnecessary.


Readability goes hand in hand with the DRY (Don’t Repeat Yourself) principles. We should avoid repeating ourselves for the same task in different areas of the codebase in order to avoid side effects and bugs with the changing requirements. In the event of an error, more repetition results in more wasted time; hence, overcomplicating the code and making it harder to pinpoint the source of a problem. Instead, define such logic in one place for consistency. If we ever have to update, it would just be done at that one place where that logic is defined.


Guideline 3 → **Tests don’t lie**


I cannot emphasize enough the importance of testing your code. While testing may be the most obnoxious part, it’s one of the most important. Make it a habit to always write, run, test, check, and then double-check the code to ensure the tests are performing as expected. For example, covering end-to-end functionality, hitting the full code path, performance is not impacted, and most importantly clearing tests after the tests are executed so it doesn’t pollute other tests. This ensures that the code is doing what is supposed to do and won’t break any existing behavior or functionality.


While we don’t want to be boxed in by a set of rules, we need a set of guidelines to follow as it ensures all edges are covered for writing clean code. While yours may be different than the above, having a set of guidelines will give confidence to other reviewers that the code is top-notch.


---


[3 Guidelines to Reduce Implementation Flaws](https://medium.com/groupon-eng/3-guidelines-to-reduce-implementation-flaws-958511e2a82f) was originally published in[Groupon Product and Engineering](https://medium.com/groupon-eng) on Medium, where people are continuing the conversation by highlighting and responding to this story.
