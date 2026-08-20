---
schema_version: "1.0.0"
document_id: "d5a3de79f1b6a17e2145d7aec785ca00f541b6c36af83762c935379985750a76"
company_key: "yc-gigs"
company: "Gigs"
source_id: "yc-gigs-news-import-0fc2b850cd38"
canonical_url: "https://gigs.com/blog/assume-you-won-t-work-on-it-again"
published_at: "2025-09-11T15:38:34.457+00:00"
first_seen_at: "2026-07-21T21:37:08.403736+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:4ea90efadd00a82dba723df638796eb0705ce4377253f88f7adfe9e8e95b9059"
---

# Assume you won't work on it again

Picture this: You're diving into a project, excited to make progress, when you encounter some existing work, maybe a piece of code, a design file, or a half-finished document. But there's a problem: you can't quite figure out what's going on. Crucial context is missing and to make matters worse, the original creator is unavailable. Sound familiar?


Over a year ago, our team faced a recurring challenge: we kept encountering work artifacts that lacked essential context, making it difficult to pick up where others left off. We discussed this issue during one of our retrospectives and realized we needed to do better going forward. What came out of this is a simple guideline:


***Treat every artifact you create as if you won't be the one to finish it.***


This approach has worked well for us over the past year. In this article, we'll explain why missing context is a problem in the first place, what exactly we mean by "treating artifacts as if you won't be there," and share practical examples from our daily work.


## Why missing context hurts


It may not sound like a big deal at first. After all, we have countless ways to reach people and ask questions. But this apparent solution masks a deeper problem that unfolds in predictable stages whenever someone encounters work without proper context.


The first thing people generally do is try to figure things out on their own. They'll look through the code, examine the design files, or read through documentation until they realize they are missing crucial context in order to make meaningful progress. They then go and try to find this context elsewhere. This means either searching on their own or asking around.


Searching on your own means going through messages, tickets, documents, and other places where information might be hidden. In addition to eating up more time, this is also annoying. Instead of adding something valuable to the product you’re stuck playing detective.


If searching doesn't yield results, you start asking colleagues: "Hey, do you know anything about this feature?"; "Were you in the meeting where this was discussed?" Each person you ask needs to stop what they're doing, try to remember details about something they might have worked on weeks or months ago, and then get back into their own work. Research shows that[even a five-minute interruption can derail focused work for up to 20 minutes](https://www.fastcompany.com/944128/worker-interrupted-cost-task-switching) , if the interruption is not about the topic a person is already working on. One quick question can cost the team nearly half an hour of productivity.


Some try to avoid this by being protective of their time, maintaining their own productivity by deflecting interruptions. But this just shifts the burden back to the person seeking information, who might wait hours only to receive a "Sorry, I don't know" response, making the process even more frustrating.


Eventually, people feel pressured to move forward with incomplete information. They'll make assumptions that seem perfectly reasonable given the information they have. Maybe they'll take an educated guess about why something was implemented in a certain way, or what the customer actually wanted. But without the full context, these assumptions often lead to solutions that either miss important requirements or ignore constraints that were discussed before.


What makes this especially frustrating is how preventable it is. In most cases, the missing context existed, it was just trapped in someone's head or buried in a forgotten Slack thread instead of being attached to the artifact itself.


## What adding context costs


We've talked about the cost of missing context, but what about the other side of the equation? What does it actually cost to add proper context to our work? Surely writing all of those things down comes with a cost as well.


It does, but it might not be as much as you think. Adding context doesn't mean writing extensive documentation for everything you do. Sometimes it's as simple as adding a quick to do list to a pull request, or linking to the relevant chat message where the requirements were discussed. Other times it might be a one-line comment explaining why you chose a particular approach over another.


The key is making it a natural part of your workflow. When you're in a meeting where important decisions are made, take a minute to write that down and share it where it makes sense. When you're working on a feature and realize there's a non-obvious constraint, add a quick note about it in the Pull Request description. The moment you obtain information that might be relevant later, share it where it needs to be.


A note about tools: Tools change as a company evolves. They might become too constrained, there might be a better alternative, or they could shut down. This means we have to keep the longevity of the information in mind. If it's only useful for the period in which a feature is developed, adding the information to the pull request or ticket is likely fine. If it's important for as long as the code is used, then a code comment would make more sense.


## Why wouldn't you work on it again?


You might be thinking: This all sounds good, but I usually finish what I start. Why would I need to write things down for others? It's a fair question. After all, we generally plan to complete the tasks we begin. But work transitions happen more often than we might expect, and for all sorts of reasons.


Sometimes the interruption is planned: you're going on vacation, and despite your best intentions, you couldn't quite wrap everything up before leaving. Other times it's unexpected: you wake up with a fever, and suddenly that critical feature needs attention from someone else. As companies mature, naturally people will also start leaving for new roles, making it even harder to obtain the information that has not been shared before.


But even when you're present and healthy, work often shifts in unexpected ways. A critical bug needs immediate attention, pulling you away from your current project. Or a key customer requirement changes, and suddenly your team needs to pivot to a different approach. In these cases, it doesn’t even have to be someone else from the team who is picking up the work. Maybe it’s you a week later. With that recent context switch, do you trust yourself to still have all the required details in you head?


The reality is that software development, like most modern knowledge work, is a collaborative effort. Projects often evolve in ways we don't initially expect, requiring flexibility in who works on what. When we write things down with this reality in mind, we make these transitions smoother for everyone involved.


## Making it work: Practical examples


Let's look at how this approach plays out in our daily work with a few examples.


-


When working on a Linear ticket, don't just update the status. Take a minute to explain why the status was changed. Instead of just adding the Awaiting Requirements label, add a note: *Added awaiting requirements - current spec mentions the flumberdubmer should be dubblewubbled, but in standup we discussed it might need to be chombawombad instead. Checking with Sarah about this discrepancy.* Better yet, link to where you asked Sarah for clarification.


-


Imagine its late in the day, you worked on a feature all day and aren’t done yet. You decide to call it a day and continue tomorrow. Push your work! Having a few commits in a branch documents what work has been done already. In order to make this easier to find for your coworkers, open a Draft Pull Request. Since its a draft, you might be tempted to not write a description. You definitely shouldn’t write an essay, but if you can write down what still needs to be done and what uncertainties you found, this will help whomever picks this up next.


-


When you're in a meeting where important decisions are made, take a minute afterward to capture the key points in the relevant places. Did you decide to take a different technical approach? Add that context to the pull request. Did you clarify some requirements? Update the ticket. Capture the information where future readers will need it.


-


For more complex topics: Write an[architecture decision record](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions.html) . ADRs are helpful in aligning everybody now, but they also act as a great way of documenting past decisions.


-


If the code you just wrote leaves you with the feeling that the way you wrote it feels off because of requirements (business or technical), chances are high the next person feels the same. Write a comment and outline the *why* behind the decision you made. Writing automated tests for this can help as well. First, they also act as documentation on the intended usage. Second, they underline the explicitness of the behavior: If someone took the time to write a test for this, there[must be a reason it was built this way](https://thoughtbot.com/blog/chestertons-fence) .


Remember, the goal isn't to document everything exhaustively. Instead, focus on capturing the kind of information you'd want to have if you were coming to this work fresh. Would you want to know why certain decisions were made? What approaches were already tried? What constraints need to be considered? These are the breadcrumbs that help others follow your path.


This approach has greatly changed how our team works together, making us more efficient and resilient to the natural changes that occur in any project. If this resonates with how you think work should be done, check out our[careers page](https://gigs.com/careers) : We’re hiring!
