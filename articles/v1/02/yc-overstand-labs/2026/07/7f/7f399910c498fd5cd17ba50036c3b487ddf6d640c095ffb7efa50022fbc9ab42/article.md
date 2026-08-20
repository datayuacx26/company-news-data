---
schema_version: "1.0.0"
document_id: "7f399910c498fd5cd17ba50036c3b487ddf6d640c095ffb7efa50022fbc9ab42"
company_key: "yc-overstand-labs"
company: "Overstand Labs"
source_id: "yc-overstand-labs-news-import-7afefb015732"
canonical_url: "https://overstandlabs.com/blogs/trust-but-verify"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-24T08:01:10.100806+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:9e975ca62a5fbb4b21c5b22878af2f7ee530aecb477ffa3701af771ffecda829"
---

# Trust but verify: How to prevent AI from making things up

Imagine an AI gives you an answer, complete with a source quote attached. You take a second and read the answer, and it definitely sounds right. Then you look at the source quote, and that also seems realistic.


There’s only one problem — the quote is completely made up. It’s fake and not in the actual source at all.


Not fake in any dramatic way. Not fake in a way that immediately jumps off the page. It’s just close enough to the original quote and captures its meaning that, if you were skimming, you probably wouldn’t notice. The source may be real, but the actual words are not there.


## The problem isn’t being wrong. It’s being almost right.


Hallucinations are one of the stranger problems with AI. Sometimes AI doesn’t fail by being obviously wrong. It fails by being almost right. And “almost right” can be a dangerous thing, especially when you’re using AI to process a lot of information that you don’t have time to read yourself.


The solution is not to stop using AI. It’s to treat AI the way we treat any tool that can be useful and fallible at the same time: trust it where it helps, but build in fool-proof ways to check its work. As President Reagan popularly repeated during the Cold War’s nuclear disarmament efforts: *Trust, but verify.*


It’s a useful phrase when reflecting on today’s situation with AI. Trust does not mean blind faith. It’s earned through reliability, transparency, explainability, and security.


Anyone who has worked with AI for long enough knows the basic problem. AI can be incredibly useful, but it can say something confidently that is not actually true. It can invent details. It can misread data. It can give you an answer that feels correct right up until the moment you inspect it closely.


## Separate what needs judgment from what can be checked


The first technique in our series of AI for Normal People is simple: separate the parts of the answer that require a level of judgment from the parts of the answer that can be checked mechanically.


Let’s say you run a business and receive a lot of customer emails. Too many for any one person to read carefully. You want AI to help you find the cases where a customer is upset because of something your company did or failed to do.


This is exactly the kind of thing AI is useful for, because it requires a little bit of judgment. The customer may not say, “I am upset because your company failed me.” They might just describe what happened. They might be polite. They might be vague. They might be annoyed without spelling out the entire situation.


You need the AI to read between the lines a little bit.


But you also do not want the AI to make a vibes-based call without citing evidence. You do not want it to say, “Yes, this customer seems upset,” and then expect you to trust it. You want the AI to show *why* it thinks this customer is upset.


## Give AI a task, not a question


So instead of asking the AI something vague like, “What are customers upset about?”, you give it a very specific task for each email:


```text
Read this customer email.


Is the customer upset because of something our company did or failed to do?


Answer with exactly 4 values:


* Yes or No
* What happened, in one sentence
* How serious it seems: low, medium, or high
* The exact words from the email that prove it


Only say Yes if the email clearly shows both:
1. the customer is upset
2. our company caused or contributed to the problem


Email:
[The email here]
```


You are asking the AI to make a judgment, but you are not asking it to make that judgment in the dark. If it says the customer is upset, it has to tell you what happened, how serious it seems, and most importantly, which words in the email support that conclusion.


That last part is where the validation comes in. The “exact words from the email” are not just there to make the answer look more professional — they are there because exact words can be *checked mechanically* by a human. Either the quote appears in the email, or it does not. There is no ambiguity there; even a computer from the space race could make this call.


So now let’s say one of the customer emails looks like this:


```text
Hi Sarah,


I wanted to raise an issue to you about the quality of the
double pane windows that we had installed. The windows are
unable to block even some minor sounds from the outside.


I'd appreciate it if we could get this looked at and fixed
as soon as possible.


Mark
```


The AI responds:


```text
Has Issue: Yes
Summary: Customer says the windows are not blocking outside sound properly.
Severity: High
Direct Quote: "The windows cannot block light sounds"
```


At first glance, this seems pretty reasonable. The customer is complaining, and the issue is about windows failing to block outside sound. The AI has understood the basic situation.


*But the quote is not real.*


The quote is supposed to be the piece that helps us *trust the answer* . If the evidence was invented, then we’ve lost reason to trust the answer — or anything the AI may have produced.


## Set a rule the AI can’t break


Thankfully, this particular failure is also very easy to catch. It does not take an intelligence (human or artificial) to decide whether a quote appears in an email. All we have to do is check it with the same basic idea as Command-F.


Does the exact quote appear in the original email? If yes, keep the answer. If no, reject the answer and send the AI a simple correction:


```text
The quote "The windows cannot block light sounds" is not in
the given email. Please re-evaluate the email and reassess
your result.
```


By baking this rule mechanically into the AI, we can first allow AI to make the judgment call — is the customer upset — and then verify it to make sure the evidence for the judgment is exact.


We can be 100% certain that the quote we get back is a *real quote from the source text* and have greater confidence in the AI’s judgement of the situation. It could still make mistakes on the severity of the problem or whether the company actually caused the problem. But we can be certain the evidence it provided is real and use that as a basis to investigate further.


## Treat AI as a process, not the final word


This is the basic pattern behind a lot of good AI systems: let the AI make the judgment call, then use normal software — plain, rule-following code behind everyday tools — to verify whatever can be verified.


That might mean checking that a quote exists. It might mean checking that a number adds up. It might mean checking that the output follows the format you asked for. The exact check will change from case to case, but the principle is the same.


AI does not become useful because it becomes perfect. It becomes useful when we stop treating its answers as magic and start treating them as things we can check.


Trust the AI where it helps. Verify wherever you can do so, mechanically. And when verification fails, make it try again.


Next steps


[Book a Free Consultation →](https://overstandlabs.com/#demo)[See how it works](https://overstandlabs.com/)


Related


[Overstand Labs](https://overstandlabs.com/)


Frequently asked questions


An AI hallucination is an output that sounds plausible but isn't true — an invented fact, a misquoted source, or a fabricated detail presented with the same confidence as accurate information. The problem isn't that the AI is obviously wrong; it's that it can be subtly wrong in ways that are easy to miss.


Because they're almost right rather than obviously wrong. A search engine returns a wrong page and you know it immediately. An AI gives you an answer that sounds correct, cites a source that's real, and fabricates the quote — you'd have to check the original document yourself to catch it. That's a much higher bar to clear on every answer.


Ask for the exact words from the source text as a required output field. If the AI must return the verbatim quote alongside its judgment, you create a checkable artifact — either the words appear in the original or they don't. That's a mechanical check, not a trust question.


Command-F. Paste the AI's quote into a search of the original document. If the exact string appears, the quote is real. If not, reject the answer and send the AI a correction asking it to re-evaluate. It takes seconds and catches the most common type of hallucination.


Ask the AI to include the exact words from the source that support its conclusion, then mechanically check whether those words actually appear in the original. If they don't, reject the answer and ask the AI to re-evaluate. This turns quote verification into a simple yes/no check that doesn't require human judgment.


It separates judgment from verification. The AI makes the calls that require reading between the lines — is this customer upset, does this contract clause apply. Then normal software checks whatever can be checked mechanically: does the quote exist, does the number add up, is the output in the right format. The AI is one step in a pipeline, not the final word.


Not fully — AI can still misjudge severity, causality, or other things that require judgment. But verification gives you a reliable foundation: you know the evidence is real, even if the judgment still needs your review. That's a much safer starting point than trusting the whole answer at once.


Written by


Mihir Patil · Overstand Labs


The Overstand team builds infrastructure for evidence-grade data work — from large-scale discovery to privacy compliance to clinical trials.


Continue reading


[→ How to make DSAR Compliance simple Respond to DSAR requests faster with Overstand. Locate personal data across systems, generate complete records, and meet compliance deadlines with confidence.](https://overstandlabs.com/blogs/privacy-dsar-rtbf)
