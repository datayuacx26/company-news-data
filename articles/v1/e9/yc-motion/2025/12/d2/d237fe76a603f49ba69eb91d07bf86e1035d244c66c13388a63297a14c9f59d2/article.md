---
schema_version: "1.0.0"
document_id: "d237fe76a603f49ba69eb91d07bf86e1035d244c66c13388a63297a14c9f59d2"
company_key: "yc-motion"
company: "Motion"
source_id: "yc-motion-rss-f628c3046538"
canonical_url: "https://engineering.usemotion.com/if-evals-are-so-good-how-come-nobody-uses-them-527de5572ca0"
published_at: "2025-12-23T17:06:01+00:00"
first_seen_at: "2026-07-20T23:21:05.244128+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:0af658414309d947564bdcddaa24c6279aef20105bbf8d0584e5507ebe1797a0"
---

# If Evals Are So Good, How Come Nobody Uses Them?

# If Evals Are So Good, How Come Nobody Uses Them?


[wrannaman](https://medium.com/@AndrewPierno?source=post_page---byline--527de5572ca0---------------------------------------)


6 min read


·


Dec 23, 2025


--


Press enter or click to view image in full size


2025 Langchain[State Of Agent Engineering](https://www.langchain.com/state-of-agent-engineering) Report


Two and a half years into the AI wave, we all know that these models are non-deterministic. Just when you think you have a perfect prompt that captures every edge case you can think of, the model decides to swerve off a cliff for no real explainable reason. Being the good engineer that you are, you turn to the “experts” who all unanimously recommend evals. If you simply give the models enough examples of the behaviors you want, and grade them against it, all your non-deterministic problems will be solved. And by solved, they typically mean “technically correct”.


Except that technical correctness is not the only goal. Increasingly, as LLM output is the product, we need to also solve for product correctness. After all, how do you measure taste?


Industry standard evals rely on closeness. We have vector embedding distances, cosine similarity, and ROUGE scores. These are excellent tools for measuring drift or retrieval.


An algorithmic eval can tell you that a generated email is 98% semantically similar to your golden reference. It cannot tell you that the 2% difference made the tone sound condescending rather than helpful. It cannot tell you that the layout of a generated website is “technically” accurate to the prompt but “feels” cluttered. An LLM judge will be confident and (more or less) consistent, but it is still guessing what “good” means without actually knowing what customers care about.


We realized that “correctness” wasn’t a single metric. Is it any wonder that[95% of AI deployments fail](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/) ?


## “Correctness”


At Motion, we are solving the *hardest* of business workflows. These tasks are long-running, multi-step, and often squishy. They rely on humans in the loop precisely because, in many cases, human vibe checking is still the best judgement on quality! In more than one case, we’ve broken down a workflow into its discrete parts, put an independent eval set on each of the five or six criteria, had the tests pass with flying colors — only to find out that the composite output just… sucked!


Take our new AI website builder as a concrete example. Each website has several pages and sections (hero, testimonials, etc) containing text and images we generate. We create it all in one shot by scraping an existing url and using the copy and images of the old site as a reference. Like many of you, we started with a composition of evals (one mapping to each of the outputs that comprise the entire website).


Unfortunately, our evaluation process was doomed from the beginning. Whenever I made meaningful progress, I would generate the site locally, take a screenshot and throw it in Slack. People would then chime in with opinions, and I would try to take feedback like “I don’t like the H1 of this one” and somehow “fix” the prompt to output something “better”. Across 10 websites and 10 internal stakeholders, we would have 20 different opinions. “I like the hero copy but hate the image”. “I hate the image but like the copy”. “I like the image for this site but the text for that site”. “It used to be better”. If this sounds like a great setup to exacerbate male pattern baldness, trust your instincts.


The core of the problem here lies in correctness:


1. There is correctness from the pure development perspective — did it grab the images from the team section, did it accurately output reviews, etc.
2. Then there marketing correctness — is that H1 going to convert better than the previous H1?
3. Growth correctness — is the above-the-fold content going to make people click the CTA?
4. And lastly, product correctness — will this website convert better than the previous one?


All this, and we still haven’t even gotten to the *paying Motion customer* yet!


## EvalLama


The turning point came when I decided to stop fighting our stakeholders and embrace them. Our marketing lead was the lead for a reason. She had *great* instincts about H1 copy. Our growth person was there because he was *good* at optimizing for clicks. So the first step was to lean into humans as a judge, and use their taste as our baseline for correctness. To do that, we needed to:


1. Provide an *easy* way for non-technical people to quickly provide feedback
2. Ideally collate that feedback based on expertise (weigh the marketing folks’ opinions on marketing more)
3. Standardize it into a single score
4. Track it over time, over many iterations


The simplest first version would simply be me uploading a full page screenshot of the site to the tool and figuring out what, exactly, to measure. Our CEO Harry suggested we count up the number of defects (start from 10, subtract 1 for each defect) and use that as a score. Another member of the team recommended actually only using boolean questions so you don’t accidentally measure the individual’s variance between their own scores. Booleans also have a nice property of allowing people to change / add / subtract questions (even when previous evals have responses already).


## Get wrannaman’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


The idea is to load the original site on the left and the new one on the right so a reviewer can quickly look at both and score them on whatever dimensions seem suitable for the use case. In our case we asked about hero image + copy, the rest of the site’s content + images and a sort of catch-all “Would you ship this?”.


Press enter or click to view image in full size


An example input/output comparison in EvalLama


It was ugly, buggy, generally felt like I vibe coded it at 3am (which I did), but it did solve two “technical” problems and one social problem:


- It gave us a score across a standard set of websites that we could measure over time.
- It only took a minute or two to grade each site.
- It became a part of the workflow and anchored our discussions


Press enter or click to view image in full size


EvalLama scoring filter by the day — who loves it, who hates it


With data like this, you can have a meaningful discussion about what exactly someone didn’t like, what the general consensus is for a particular output, whose opinion is an outlier, and of course the all-important final score to see how it’s changing over time.


Press enter or click to view image in full size


The Evallama score of our website builder (so far!)


If you hold the websites constant and the evaluators constant, it’s a pretty good way to say concretely if the thing is getting better or worse. One happy accident is that each evaluator represents a specific area of expertise (growth, copy writing, product, etc.), so filtering by person gives you a rough filter by perspective, too.


Handing over AI outputs to customers is still a little terrifying. You have to ensure the product is great, the model isn’t hallucinating, and your CFO doesn’t hate you. And yes, there are a metric ton of tracing + eval tools in the market, but there really wasn’t a simple way to get a quantitative score on qualitative output.[Evallama](https://evallama.com/) is somewhere in between a survey tool and an eval tool specifically designed to help AI teams move faster on qualitative tasks. It won’t solve every hallucination, but for us at Motion, it turned stalled progress into a measurable workflow.


Perhaps most importantly, we now have an explicit *expectation* to iterate on AI deployments rather than throw AI at the problem and simply expect magic, even if our evals tell us the output is correct. AI is something between an art and a science, but regardless of the field, you get what you put in. Evallama has helped us coordinate just how much we’re going to invest as an organization and provided a common language to measure the rewards.
