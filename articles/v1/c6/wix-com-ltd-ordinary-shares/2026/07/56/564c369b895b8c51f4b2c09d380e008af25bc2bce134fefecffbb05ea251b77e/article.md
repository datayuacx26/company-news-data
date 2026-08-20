---
schema_version: "1.0.0"
document_id: "564c369b895b8c51f4b2c09d380e008af25bc2bce134fefecffbb05ea251b77e"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/the-end-of-determinism-what-s-left-for-engineers-when-ai-writes-the-code"
published_at: "2026-07-19T10:33:35+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:a1749a7f141d6f2561cd2e33efc18d419adf14a073da1ca97f7be33a1001d3d6"
---

# The End of Determinism: What’s Left for Engineers When AI Writes the Code

During 2026, I shipped code that I don't fully understand. It was written in a programming language I'm not even fluent in. A year ago, this would have been a fireable offense. There's no way I would have stood on a stage at a Wix Engineering conference, my employers in the crowd, and said that sentence out loud. Last week, it was just another Tuesday.


And here's the part that's been keeping me up at night: I was proud of it.


That's a paradox, right? If someone else wrote it, I can't be proud of it. And if I'm proud of it, there's no way someone else wrote it. Except both parts of that sentence are true. And I think unpacking why tells us something important about what our job actually is now and what it always was.


##


**The Question We're All Avoiding**


There's a feeling in the hallway of every major tech company right now. No one talks about it directly, but everyone feels it. It's the feeling of existential fear.


If AI writes all the code, what are you here for?


Is the job we spent decades perfecting - the craft we built our identities around - completely obsolete? Anyone who tells you they're not scared about this is probably lying. I feel it everywhere.


I want us to sit with that for a moment before jumping to reassurance. Because the honest answer is more interesting than either "you're fine, don't worry" or "yes, you're replaceable."


Our job has shifted in three fundamental ways. In each of them, there is still a massive difference between the engineers who will do extraordinary work and those who are, yes, probably replaceable. The difference is not what most people think.


##


**Shift I: From Monologue to Dialogue**


Not too long ago, writing code meant opening an editor and facing a blinking cursor. You were the only thinking participant in the room. Writing code was a monologue.


Now, writing code is prompting. It's iterating. It's pushing back. It's a conversation.


This may sound like a minor change in workflow, but it’s not. It's actually a complete shift in cognitive mode. Dialogue is a fundamentally different mental activity from monologue and we've known this for about 2,400 years.


Socrates never preached. He never wrote anything down. He walked around Athens asking people questions, listening carefully, and following the logic wherever it led. He saw his job as a midwife: helping others deliver ideas they already had inside them. He said he couldn't teach anyone anything - he could only make them think.


*Socrates would have been extraordinary at Claude Code!*


Here’s the thing: not every chat is actually a dialogue. When you're in the state of mind of -


*make this work, ship it, make it faster* - you're not doing engineering. You're a customer dropping a car off at a mechanic. You're not engaged, not present, not thinking. That will not yield good results. That is not a dialogue by socratic standards.


On the other hand, when you treat the machine as a peer and ask:


-


"Why did you choose that approach over a simpler interface?"


-


"What are the assumptions underneath this logic?"


-


"What breaks if traffic is 10x's?"


Then you're involved. You're pushing the software to actually become better. That's engineering.


The shift isn't just that you have a new tool. It's that you now need to be a good dialogue partner. That's a skill. It requires presence, curiosity, and the ability to hold a mental model of what you're building while simultaneously interrogating it.


##


**Shift II: Authorship Is Distributed. Ownership Is Not.**


git blame


still points at me. But something about that answer doesn't feel quite right.


In 1967,


**Roland Barthes** wrote an essay called


*The Death of the Author* . He wasn't talking about AI, he was talking about literature, but his core claim turns out to be remarkably literal: any text is


*a tissue of quotations.* There is no original. There never was.


Think about how you write code. You use design patterns you learned from others. You pull ideas from Stack Overflow answers you absorbed years ago. You depend on libraries written by people you've never met. Authorship has always been distributed; the single author was always a convenient fiction. In the AI age, we just can no longer sustain it.


But distributed authorship doesn't mean distributed ownership. And this is where it gets serious.


Vercel published a document called


[Agent Responsibly](https://vercel.com/blog/agent-responsibly) that gets this exactly right. They offer a litmus test for AI co-authored code:


*Would you be comfortable owning a production incident tied to this pull request?*


Notice what that question is and isn't asking. It's not asking whether you can explain every line. It's not asking whether you understand every abstraction. Experienced engineers know we never understood our code in that exhaustive way. We comprehended the software. We knew the architecture. We knew the strengths and the weaknesses. We knew where the bodies were buried.


That's the distinction that matters: the difference between


*relying* on AI and


*leveraging* it. When you rely on AI, you outsource your thinking. The agent wrote it, tests passed, and shipped it. You would not be comfortable being woken up at 3am when it breaks. When you leverage AI, you own the outcome. You have a mental model. You'd stand behind it.


Authorship is now clearly distributed. Ownership is not. Someone stands behind this. That someone is you.


##


**Shift III: Taste Is the Work**


If the how changed, and who changed what remains that is truly, uniquely ours?


Taste.


**Greg Brockman** , president of OpenAI, called it "a new core skill." I'd push back on the word


*new*


**Steve Jobs** was talking about taste in the 90s. But it's definitely the core skill now.


Some people in this room are thinking: taste? Isn't that just subjective personal preference?


Here's a test. Imagine two paintings side by side. One is worth $5 million. The other was made by a sixth grader.


If you can't tell which is which, it doesn't mean taste is subjective. It just means you haven't spent enough time in museums. Taste is real. And it's


[learnable](https://paulgraham.com/taste.html) .


Think about the code you wrote five years ago. If you look at it now, it probably looks like a mess. That's not because your personal preferences changed. It's because your sense of smell for good design got better. That improvement is real, and it's yours.


But taste alone isn't enough.


**Paul Graham** put it well: when anyone can make anything, the differentiator is


*what you choose to make* . That's judgment. An LLM can generate a thousand solutions to your problem. Only you know which one is right for this system, this team, this moment. The judgment is yours. Increasingly, that judgment


*is* the work.


##


**Moving Up the Ladder**


Uncle


**Bob Martin,** not a tech hippie, not a naive optimist, the clean code guy said something striking recently:


*what we're losing to AI is syntax, and good riddance. The less our brains are occupied by semicolons and braces, the more room there is for more interesting problems.*


We've been here before. Every time we moved up the ladder of abstraction from assembly to C, from C to managed languages, from managed languages to frameworks someone screamed that this wasn't


*real* engineering. Each time, they were wrong. Moving up the ladder freed us to deal with harder, more interesting problems.


This is probably just the next rung. But it does feel different from the others. Every previous step had a gate: you needed to study computer science, you needed to be comfortable with code. This one doesn't.


*The hottest new programming language* , as


**Andrej** **Karpathy** put it, is English. Anyone can build software now, A doctor with a workflow problem, a teacher with an idea, a child with a dream. That is genuinely remarkable.


It also means we're now standing on the same rung as everyone who speaks English.


What we can see from that rung though, if we look carefully, is something they can't: the architecture underneath. The tradeoffs. The failure modes. The difference between a solution that works today and one that holds up at scale. The ability to conduct a real dialogue with the tools not just issue commands, but push back, question assumptions, stay present.


**Dialogue. Ownership. Taste.**


These are not soft skills. These are engineering skills. They always were. We just couldn't see it clearly because we were too busy typing.


**The code got easier. The questions got much harder. Welcome to the job.**


*This post is based on a talk delivered at Wix Engineering Conference 2026. The presentation itself was built in dialogue with AI. The irony is intentional. Who made it? That's exactly the right question to ask.*


This article is based on


**Tom Enden** ’s talk at Wix Engineering Conference 2026, "The End of Determinism: What’s Left for Engineers When AI Writes the Code".


Here’s what


**Robert Martin** ("Uncle Bob") had to say about the session:


**Watch:**


**Tom Enden**


You can


[follow him on X](https://x.com/tomenden)


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/)


|


[Instagram](https://www.instagram.com/wix_eng/)


|


[Facebook](https://www.facebook.com/WixEngineering/)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)
