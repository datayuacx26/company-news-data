---
schema_version: "1.0.0"
document_id: "2a4ebcc72a4a0ea754cb76ed841032e831c35dc7edfff78b200e1e8ef12751b7"
company_key: "yc-convictional"
company: "Convictional"
source_id: "yc-convictional-atom-2bd8494c6cc9"
canonical_url: "https://get.convictional.com/posts/butterfly-effect-applied-ai/"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-08-09T21:02:07.552290+00:00"
fetched_at: "2026-08-09T21:02:10.293363+00:00"
content_hash: "sha256:171bd407b168ad8ab8404b08666473af1735b54c5a32a6624490d85c5ecaf464"
---

# The Butterfly Effect in Applied AI and Human Subjectivity

#


# The Butterfly Effect in Applied AI and Human Subjectivity


## Tl;dr takeaways


- **Stop trying to engineer away AI unpredictability** . Slight variations in AI deep research outputs from run to run are caused by fundamental computing limits ("machine noise"), not software bugs. You cannot engineer this away.
- **Fix your company's brain** . Scattered, high entropy business data worsens this machine noise. The highest ROI comes from structuring your internal data so the AI can easily find all the pieces to the puzzle.
- **Humans are actually the biggest variable** . In our experiments, people disagreed on the quality of the *exact same* AI report by up to 30%. Human subjectivity creates 2-3x more variance than the machine noise.
- **The future is judgment work** . Because objectively perfect AI reports are impossible, your team's main job will shift to using those AI outputs to collaborate, debate, and make decisions. To support this, AI tools must be built with transparency in mind, so your team actually trusts the data they are debating.


---


If you ask your company’s database for last quarter’s revenue, you get the exact same number every time. If you ask your sophisticated AI agent about a complex topic like last quarter’s strategic missteps, you get a coherent, seemingly comprehensive answer. But, if you ask it the exact same question again right after, you will likely get a slightly different, though equally plausible, report. As our internal research recently revealed, this differing output isn't a flaw in the AI's intelligence. Instead, it's a direct reflection of your company's chaotic data, and the subjective ways we evaluate it.


For business leaders accustomed to standard dashboards and reporting tools, this unpredictability feels like a bug. Beyond that though, this introduces a real risk to decision making. You cannot confidently execute a strategic decision if the information you are working with shifts depending on when you ask the AI.


Our Research team has noticed variations in how our deep research agent produces reports. We often conduct experiments that involve running the same input questions multiple times while developing. Understanding the potential business impact is important given the decisions these reports can inform, so we decided to dig into it further.


We recently conducted an internal experiment to put numbers to this "noise" in our AI deep research pipeline. The results challenged our assumptions that AI can act like a perfectly predictable software tool (under certain conditions). We realized that getting reliable business value from AI doesn't come from buying a smarter model; it requires understanding fundamental limits of computing, structuring your unorganized data, and accepting that human subjectivity makes a perfect AI report impossible.


## The Butterfly Effect: Machine noise


In chaos theory, the[butterfly effect](https://en.wikipedia.org/wiki/Butterfly_effect) is a principle that describes how a tiny change in a complex system can have large, unpredictable consequences. The classic example is a butterfly flapping its wings in Brazil and setting off a tornado in Texas. To understand how this applies to AI, and why its output might vary given the exact same inputs, we must first consider how LLMs actually work.


### How LLMs actually write: Constantly calculating the next most probable word


Given an input, an LLM generates an output as a sequence of words (actually, it's tokens, but we don't have to worry about this for the sake of explanation). When generating the sequence of words, the LLM uses the existing sequence of words at that time and computes the next most probable word. This is repeated over and over until completion of the output.


### LM temperature: The setting that controls creativity and randomness


Next, there are a number of factors that control computing the next most probable word. Some of these are contextual, like the[specific language or phrasing used to ask the question](https://lab.cloud/research/the-language-is-the-lever/) , which can affect the model's bias. Others are architectural. But for argument's sake, I want to focus on two purely mathematical factors that I think are important: LLM temperature and floating-point rounding errors in computing.


LLM temperature is a parameter that is set at the time of making a request to an LLM. It basically decides how *creative* the LLM should be with its output. In theory, setting temperature to zero tells the LLM you want minimal creativity, and basically it should strictly stick with the next most probable word when generating the output sequence of words. Conversely, setting a higher temperature tells the LLM it can be more liberal when generating a sequence of words, and so the output is (a lot) less deterministic.


### Rounding errors: The hardware limitation that prevents consistency


[Floating-point rounding errors](https://en.wikipedia.org/wiki/Round-off_error) come from the fact that computers can't truly represent continuous numbers. This is largely due to the fact that a computer has finite memory to store this data, and therefore cannot store an infinite sequence of numbers - the precision of the number is finite.


For example, the representation of the number 10.333... in a computer is effectively rounded at some number of decimal places. When you perform arithmetic on a series of these representations, those rounding errors compound across the calculations. The result is that the final number is not perfectly accurate, as it would be if you could keep infinite precision. This is similar to calculating a household budget and having the final percentage add up to a little over 100% (e.g. 100.01%) instead of exactly 100%.


So, even if you ask an LLM a question and set the temperature to 0, the calculations it performs in the background to respond to you process floating-point rounding errors, causing the output to vary.


### The Butterfly Effect in practice: How tiny variations compound across a multi-step research agent


To further complicate things, there are typically many steps in a deep research agent. We use a recursive tree approach. I won't go into the full details, since it distracts from the main idea of this post, but the gist is that there are multiple iterations of generating search terms, retrieving internal documents based on those search terms, extracting learnings from the retrieved documents, and formulating follow-up questions.


Basically, there are a number of intermediate LLM-generated artifacts that power the current iteration and the next iteration. The extracted learnings from each step are all combined in a final step to generate a report for the user.


If there are slightly differing LLM artifacts early on in the chain, those differences can compound to different search terms, retrieved documents, learnings, etc by the end of the chain. Just like the flap of the butterfly's wings, a small variation at the start of the process creates a different report.


## Piecing Together the Puzzle: High-Entropy Data


We have extensive prior research into business entropy at Convictional (see[here](https://get.convictional.com/posts/context-entropy/) ,[here](https://get.convictional.com/posts/why-searching-cant-solve-business-complexity/) , and[here](https://get.convictional.com/posts/just-in-time-compression-for-better-deep-research/) ). In short, entropy is a measure of how scattered information about a given topic is across your company.


Imagine sending your AI deep research agent into the "library of your business," which holds every document, Slack post, email, etc. For a simple, low entropy question like *What is our remote work policy?* , the AI just grabs the single book called *HR Policies* . The butterfly effect is rarely an issue here because all roads lead to the exact same PDF.


However, the most important business questions tend to be high entropy. If you ask, *How did our ideal customer profile evolve over the last five years?* , the context is spread across thousands of Jira tickets, emails, Slack threads, and meeting notes. The AI has to check out many books and piece together a massive puzzle.


However, an AI agent has a practical limit on how many documents it can retrieve at one time. Let's say there are 1,000 pieces to the puzzle, but the AI can only collect 800 per run. Because of the machine noise we just discussed, the AI will generate slightly different search terms each time you ask the question. This means it takes a slightly different path through the library stacks each time.


For one run, it grabs one set of 800 puzzle pieces and paints a highly plausible picture. Run it again and it takes a different path, grabs a different subset of 800 pieces, and paints a slightly different picture. It isn't wrong, just different. However, the details of those variations *could* be the difference between a green light and a no-go on a decision.


Effectively, when the AI deep research agent can't grab all the pieces of a high entropy puzzle at once, it makes the machine noise problem even worse.


## Our Experiment


Up to now, we have discussed a lot of theory. We know deep research outputs differ from run to run, but practically speaking, how important are these differences really?


We designed an experiment to find out. We ran multiple deep research runs for the exact same low and high entropy questions. Since humans are the actual consumers of these reports, we had our team grade the head-to-head comparisons based on information accuracy.


For each pair of reports, our team rated them using this 5-point scale:


- Report 1 is much better
- Report 1 is slightly better
- They are the same
- Report 1 is slightly worse
- Report 1 is much worse


When we averaged the scores for high entropy questions, we found a spread of about half a point. This means if you run the same question twice, the quality of the second report will typically swing halfway toward "slightly better" or "slightly worse" compared to the first.


Interestingly, for low entropy questions, the spread dropped by half, to just a quarter of a point. Practically speaking, this means the ratings barely moved off "They are the same." This confirms that simpler data is much less affected by these fluctuations.


Finally, because we could see every step the deep research agent took - from generating search terms to retrieving documents - we tracked exactly where each run started to drift apart. We found that the divergence happens almost instantly. In fact, the very first search terms generated in step one of the pipeline were often not exactly the same!


## Enter Human Subjectivity


So far, we have found that deep research output variations differ for low and high entropy data, and the difference is at most half a point on our scale. That doesn't sound bad at all, does it?


However, those were *averaged* results. Averages are great for measuring the machine's baseline, but they ignore how much the human graders disagreed with each other. Since humans are the direct consumers of these deep research reports, that rater disagreement - the actual perceived quality of the report - is important to analyze.


When we looked at how individual human raters scored the exact same outputs, we found a spread of 1 to 1.5 points for high entropy data, and about 1 point for low entropy data. On a 5-point scale, that translates to a 20% to 30% variation in perceived quality from person to person.


This means the exact same report read by two different people could result in a 20% swing in how it is received! One executive might read a report and think it is great, while another reads the exact same report and finds it relatively lacking.


This difference is significant. It indicates that the subjective nature of human perception is 2-3x the "machine noise." This result calls into question the idea of trying to make AI outputs objectively perfect, since the audiences reading them will never universally agree on what perfect looks like.


Of course, this isn't to say that information accuracy on the part of the AI doesn't matter. Rather, it assumes the baseline accuracy is already relatively high, and shows that even in a best-case scenario, humans will still naturally disagree on the exact quality of the research.


## Conclusions


We are left with a paradox: we are deploying AI systems that are inherently non-deterministic, aiming them at chaotic, high entropy business data, and evaluating them using highly subjective human standards. Yet, in much of the day-to-day applied AI engineering, we try to engineer away the unpredictability as if it were a typical software bug.


Based on our findings, this is likely not the correct approach. Instead, we should approach the problem space more pragmatically:


1. **The ROI of AI engineering:** Stop trying to "fix" the AI agent, and start fixing your company's brain. If you do not fix how your company preserves context, even the smartest research agent will fail. Machine noise is an unavoidable reality of the AI math. Because you cannot engineer your way out of this unpredictability, your time is better spent organizing the scattered data you feed into the AI as it's within your control.


2. **Data architecture:** As we have seen, high entropy data makes the machine noise problem worse. This shows why we need to handle high entropy data directly. Summarizing the data ahead of query time ([pre-compression](https://get.convictional.com/posts/why-searching-cant-solve-business-complexity/) ) or at search time ([on-the-fly compression](https://get.convictional.com/posts/just-in-time-compression-for-better-deep-research/) ) are two ways to do this. Dealing with high entropy data directly makes sure the AI agent finds the whole puzzle, no matter which path it takes.


3. **Building trust for judgment work:** Perhaps the most important takeaway for product and business leaders is the reality of human subjectivity. Using the AI output to collaborate, debate, and make decisions - what we call judgment work - will become the main work people do at your company. Because objectively perfect reports are impossible, we must build for trust through transparency. You need to show the user exactly which "library books" the AI checked out and how it reached its answers. If you give your team clear, transparent evidence, they can confidently debate the facts and drive the business forward.
