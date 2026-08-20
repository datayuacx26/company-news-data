---
schema_version: "1.0.0"
document_id: "32b8aa1a592931893cedf8138e2fbe34ffecba944ea16ad8dbb05c1a4afc6fa4"
company_key: "yc-recidiviz"
company: "Recidiviz"
source_id: "yc-recidiviz-news-import-9fcc86c2623e"
canonical_url: "https://www.recidiviz.org/updates/how-to-constrain-ai-to-do-its-job-well"
published_at: null
first_seen_at: "2026-08-14T02:42:11.268050+00:00"
fetched_at: "2026-08-14T02:42:12.592924+00:00"
content_hash: "sha256:039a2d9bf53ae3ad0b2e899f1400b43541dabc524e4a82c65206630ccafb964f"
---

# How to Constrain AI to Do Its Job Well

Generative AI has produced more than[1,000 bogus citations in legal filings](https://www.scientificamerican.com/article/why-lawyers-keep-citing-fake-cases-invented-by-ai/) and once led to a draft police report that claimed an[officer turned into a frog](https://www.forbes.com/sites/larsdaniel/2026/01/04/cop-transforms-into-frog-according-to-ai-generated-police-report/) . Given this track record, how can corrections departments trust AI tools to help them manage important details?


The key is building constraints to make Generative AI, which we’ll just call AI here, work reliably and predictably. With the right guardrails, engineers can craft a system that reduces the risks and keeps the benefits. AI is the best tool available today for making sense of messy language, adapting to unexpected inputs, and generating text that humans can read. For corrections, this means AI can help staff who would otherwise fill out forms manually, comb through decades of case notes, and track down local resources on their own. When AI works well, it can help staff save time and accomplish more.


## **Predictably unpredictable**


We won't catalog every way AI can fail, but a few matter most for corrections:


- AI doesn't always follow instructions the way you'd expect, and how you phrase an instruction can make a surprisingly large difference in the AI’s output.
- It can give different answers to the same question.
- It has trouble admitting when it doesn't know something, like an agency acronym, and can sound confident even if it’s wrong.
- AI misses cues a human would catch, like sarcasm.
- It “hallucinates,” or makes up resources, outcomes, facts that sound real but aren’t.


The solution to most of these problems is the same: make processes as structured as possible and leave as little as possible to chance. Wherever precision matters more than flexibility, predictable tools like filters and lists work better than AI. Providing detailed instructions, constrained outputs, and layers of automated review can keep AI’s outputs predictable and easy to check.


## **Tightly structured inputs and outputs**


### Filtering inputs


Some inputs should never reach AI at all. We run filters looking for specific terminology and patterns that catch attempts to manipulate the system, including[prompt injections](https://www.gravitee.io/blog/owasp-top-10-for-llm-applications-2025-a-practical-guide) , a form of attack meant to mislead AI. Our system separately flags crisis signals like self-harm disclosures. These situations need a person, not AI.


### Specific prompting


AI responds to prompts, which include both content and instructions that tell AI what to do with that content. Getting those instructions right is one of the most important, and tedious, parts of building a reliable AI system.


How you word things matters more than you might expect. Asking the AI to “summarize what was discussed” opens much more space for interpretation than “extract only what the client explicitly stated.” Prompts that seem clear to a human often lead to imprecise or wrong results, so we’ve refined our prompts through extensive testing to find the wording that AI reliably follows. Because each model and version of AI responds differently, our prompts are fine-tuned to a specific one and retested whenever we switch.


[Negation](https://arxiv.org/abs/2605.03052) requires particular care and accuracy. When a client says they didn't use drugs last week, the AI needs to register that as a denial, 100% of the time. This goes back to the[“not all errors are equal” principle](https://www.recidiviz.org/updates/how-we-deploy-ai-and-why-we-do-it-carefully) . If AI is accurate 98% of the time, but the 2% it gets wrong involves dropping the word ‘not’ – turning ‘the client did not use drugs’ into ‘the client used drugs’ – then getting that wrong in case notes can lead to poor outcomes for the client and the wrong administrative response. AI requires explicit instruction to guard against this.


We tailor prompts for each state we work with. The terminology, acronyms, and procedural details that feel obvious to a Nebraska case manager may mean something different, or nothing, to the AI without explicit guidance. Every state deployment goes through our[multi-stage evaluation](https://www.recidiviz.org/updates/how-we-deploy-ai-and-why-we-do-it-carefully) because we need to be absolutely sure that our adaptations get it right, and more importantly, don’t get it wrong.


### Structured outputs


The more constrained and structured the AI’s outputs are, the better. When we constrain both what the AI gathers and how it reports back, we get[structured data](https://simonw.substack.com/p/structured-data-extraction-from-unstructured) instead of narrative text. That structure serves several purposes: it reduces the chance AI writes whatever it wants (including hallucinations) and limits the scope of errors, it’s easier to fill in lines on a form or update a database for the case manager, and it makes population-level analysis possible.


One way we structure outputs is by telling the AI exactly which categories of information to generate so that it works through them one by one. For drafting a case note after a meeting in our[Meetings module](https://www.recidiviz.org/platform-modules/meetings) , this might look like structuring the case note by pillars of re-entry support: housing, employment, support system, etc. Case notes include other constraints, like following the specific template for a state, covering specific topics in the summary, or having a specific structure for action items. For extracting information from existing case notes, this might look like telling the AI to look for information related to housing categories: primary housing status, housed type, dependent housing type, etc.


Another way we structure outputs is to limit the options AI can select, rather than writing freely. In this example, AI fills in the blanks of a structured form like a “Mad Lib.” Our Case Notes Insights functionality works like this, turning free-text notes into structured data. This helps prevent the AI from inventing agencies or services and also makes it possible to evaluate at scale, since it’s easier to grade a multiple choice test than an essay.


## **Following the thread**


We also build something called “entity resolution” into case note summaries. It's how the AI recognizes that “Pizza Hut,” the misspelled “Pizza Htu,” and "the restaurant on 3rd Ave" are all the same place, or that “Sheila,” “my cousin,” and "she" might all refer to the same person. Clients and case managers don't speak in structured data. They use nicknames, shorthand, and casual references. Without this layer, the AI might treat each variation as something new, missing that the same employer or person keeps coming up across conversations.


This is what “leave as little as possible to chance” looks like in practice. Catching misspellings and known aliases doesn't require AI; precise, rule-based matching handles it better and more predictably. The state-specific glossaries give the AI a reliable anchor rather than allowing it to guess. But recognizing that “the restaurant on 3rd Ave” is the “Pizza Hut” mentioned earlier in a conversation does require AI, because no reference table can anticipate every way people might refer to a person, place, or program.


## **Several layers of quality checks**


Quality control happens while the AI is working and after it produces an output. As the AI works, we make it show how it got each answer, evaluating it against auditable criteria. We use several mechanisms, including:


- **Relevance checks:** Before the AI processes a document in full, we have it check whether the document contains anything relevant at all. It's a simple early gate that catches mismatches before they compound.
- [Citations](https://arxiv.org/abs/2305.14627) **:** The AI must point back to the source text for any claim it makes. A separate automated check then verifies that the cited text actually says what the AI claims it does.
- **Confidence levels:** Rather than asking how confident the AI is, we ask it to label how it got each piece of information: was it stated explicitly, inferred, or a guess? This produces more honest and useful outputs than a simple confidence score.
- **Null reasons:** If the AI can't extract information from something, it has to say why. This catches a different class of failure: the things it skipped rather than hallucinated.


In the simplified example below, you can see what some of these structured outputs and mechanisms look like. Here, the AI looks at a sample case note and pulls information from predetermined lists of categories for primary housing status, housed type, dependent housing type, housing duration, and new housing search. The AI must also select its confidence level and include a citation back to the source text to justify its output.


We also prompt the AI to argue against its own answer before committing to it. If it can produce a reasonable counterargument, it has to flag the output as uncertain. Just as with people, when the AI knows it will have to justify its output, it tends to do a better job.


Once the AI produces an output, we run two layers of automated checks through a “defense in depth” approach. The first layer focuses on structure, verifying that the output is usable: it has the right format and length, required elements, and no prohibited phrases. The second is interpretive, which we described in our[last post](https://www.recidiviz.org/updates/how-we-deploy-ai-and-why-we-do-it-carefully) . This includes the LLM-as-a-Judge review, where a separate AI model reviews the content. Similar to how it’s easier to fact-check an existing essay than to write and cite one, a second AI model with similar overall capabilities can still catch the first model’s mistakes. Human reviewers are also part of this stage—manually annotating outputs and verifying that the automated judge is well-calibrated.


The structural layer catches what's broken before it advances, and the interpretive layer catches what the structural checks can't see.


## **Constraints are intentional and necessary**


Generative AI is only a few years old, so making it predictable and reliable means experimenting with different prompts, workflows, and techniques. We’re applying what we can learn from[other researchers](https://www.oreilly.com/radar/what-we-learned-from-a-year-of-building-with-llms-part-i/) , and trying some new routes of our own, all within corrections.


The result is a tool that case managers can trust and that agency leadership can stand behind, because it was built with safety and accuracy standards at the forefront rather than retrofitted to meet them. And just as important, we make adjustments for each state. When states ask us to expand what AI does for them, that's a signal the approach is working.
