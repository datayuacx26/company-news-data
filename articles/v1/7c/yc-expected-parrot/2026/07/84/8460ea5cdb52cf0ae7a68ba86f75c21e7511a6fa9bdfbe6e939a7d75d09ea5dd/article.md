---
schema_version: "1.0.0"
document_id: "8460ea5cdb52cf0ae7a68ba86f75c21e7511a6fa9bdfbe6e939a7d75d09ea5dd"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/test-your-survey-on-synthetic-respondents"
published_at: "2026-07-15T17:31:22+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:e9b0c5b1a7aab8d8c51ebff9cbbea48ccefec3a16e5b9a8babea2c9f3584c75a"
---

# Test Your Survey on Synthetic Respondents Before You Waste a Single Real One

# Test Your Survey on Synthetic Respondents Before You Waste a Single Real One


### How to do this easily with Expected Parrot


[John Horton](https://substack.com/@johnjhorton)


Jul 15, 2026


***tl-dr; If you run consequential surveys, you should be testing them with simulated respondents ahead of time to identify problems with the survey at both a technical level and a research question/conceptual level. Expected Parrot makes it easy to do to this.***


I was once involved in fielding a survey to software developers about their AI tool usage. The survey asked if they user had used “GitHub Copilot”; the survey methodologist we were working with had never heard of this and edited it to just “GitHub” assuming we had a typo. We missed his edit and as a result, nearly 100% of our developers reported “Yes.” Oof.


Thanks for reading! Subscribe for free to receive new posts and support my work.


I think nearly researcher who has run surveys knows this feeling: the data comes back, and a question that seemed clear turns out to have meant three different things to three different people; a skip pattern didn’t work right; a scale confused everyone; nearly everyone selected “other” which is not what you planned. By then it’s too late. You’ve wasted money & time. Even if the survey is fine, there is the similar feeling of “ah, we should have asked X” once you actually start doing the analysis.


## Cognitive testing / piloting are great but…


The known defense against this kind of failure is piloting and “cognitive testing”: run the survey past some real people first, watch where they get confused, and fix it. The problem is that good cognitive testing is slow and expensive. If done with a convenience sample, it also requires someone playing a role—actual target population of a survey—and it’s not likely we really understand this population with respect to the question (if we did, we probably wouldn’t be asking).


AI gives us two nice options to this slow, expensive and unrepresentative problems. We can now use AI to proofread a survey. Frontier models have some good practices “on distribution” i.e., they know a survey shouldn’t have a double-barrelled question. They know that people will tend to choose middle options and so on.


Suppose you are surveying customers and you draft this:


> **Q7.** How satisfied are you with the support you’ve received?
>
>
> ○ Very satisfied
>
>
> ○ Satisfied
>
>
> ○ Neutral
>
>
> ○ Dissatisfied
>
>
> ○ Very dissatisfied


You can give this to a frontier AI. Claude, for example,


[nails the issues](https://claude.ai/share/776fe271-0ab9-49be-879c-0d39a1309b10) :


But here’s the rub: If I am editing a survey on Qualtrics or Survey Monkey, how, exactly, do I get Claude to review? Maybe Qualtrics has some homebrew AI tool but it’s likely behind a paywall you’re on the wrong side of. And suppose it makes some nice fixes, how can I deploy my survey?


Not being able to deploy it forecloses an exciting option in design. We can have


[AI simulated personas](https://www.nber.org/papers/w31122) take our survey. To the extent those respondents are realistic, we can even analyze the data they return to pilot / stress test our whole analysis flow. But if I want to have simulated personas try out my survey, how do I do that? Do I give “them” a link to my survey? Paste the survey text into chat and the persona? How do I get the data out? I can ask Claude to simulate, but it will just “one shot” a plausible-seeming distribution of responses not actually have a model pretend to be a given persona.


If I want to do more sophisticated analysis on the responses, am I manually downloading a CSV giving it to Claude Code? If I learn something, I need to go back to Qualtrics or Survey Monkey. It sounds like a flow-breaking hassle.


And more importantly, it breaks the survey design process into distinct design & test phases where instead it should be happening at the same time. Imagine programming computers but you can only test the code you’ve written it and it’s expensive to do so.


## The Expected Parrot Approach


We’ll use the


[Expected Parrot](https://www.expectedparrot.com/) Research Agent as a design studio for our survey. It’s a conversational agent so you can just paste in what you want to do:


The research agent asks a bunch of clarifying questions to understand my intent and goals:


The research agent does not just keep all this in a chat context—it writes Python code in our


[open source package EDSL](https://github.com/expectedparrot/edsl) to create, in code, the survey being used, the personas being constructed, the data-generated and so on.


The personas here are just reasonable guesses but if you have past survey data, something a lot smarter can be done instead.


The research agent actually runs these personas through the survey using our API. Critically, EDSL has a standard for all the usual question types so the AI simulation very closely matches what would be send to actual humans e.g., free text, multiple choice, text-box and so on.


After this simulation, it finds the same issues as Claude (this is a pretty simple one) but it’s done by those AI personas actually going through the survey. This becomes critical with more elaborate personas, complex piping and skip-logic and so on.


It generates a written report on the issues with the “quotes” from synthetic personas:


But more importantly, it can make the necessary fixes and then re-test. Here we can see it deals with the “But I haven’t contacted support” issue that had previously caused a simulated subject to select “Neutral”:


One we feel good, the research agent can then refine the question and immediately deploy it via email, a URL or even Prolific:


You can take the survey


[here](https://www.expectedparrot.com/respond/human-surveys/3ae8df49-3fe3-498c-9944-bb0e293c05d9) .


The research agent can analyze the real survey and/or simulated survey:


Under the hood, the research agent is not just drawing on context to all this: it’s writing the Python code to actually analyze the data. This matters because it is easy to analyze more data when it comes in, the research agent (or any coding agent) can reason over what was done and find issues.


## Concluding thoughts


This is a toy case, and you might catch it on a careful read. The point is that a synthetic pass catches it


*reliably and instantly* , across every question at once, for the cost of a prompt—and the harder, subtler versions of these same problems are the ones you’d otherwise only discover after fielding.


## Some References


Beatty, P. C., & Willis, G. B. (2007). Research synthesis: The practice of cognitive interviewing.


*Public Opinion Quarterly, 71* (2), 287–311.


[https://doi.org/10.1093/poq/nfm006](https://doi.org/10.1093/poq/nfm006)


Presser, S., & Blair, J. (1994). Survey pretesting: Do different methods produce different results?


*Sociological Methodology, 24* , 73–104.


Presser, S., Couper, M. P., Lessler, J. T., Martin, E., Martin, J., Rothgeb, J. M., & Singer, E. (2004). Methods for testing and evaluating survey questions.


*Public Opinion Quarterly, 68* (1), 109–130. https://doi.org/10.1093/poq/nfh008


Tourangeau, R., Rips, L. J., & Rasinski, K. (2000).


*The psychology of survey response.* Cambridge University Press.


Willis, G. B. (2005).


*Cognitive interviewing: A tool for improving questionnaire design.* Sage.


Thanks for reading! Subscribe for free to receive new posts and support my work.
