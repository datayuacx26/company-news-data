---
schema_version: "1.0.0"
document_id: "62d419902a3bc4367cc8617aa07a7c9b706b28ccb6bde57fca6a13c49f806e3c"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/we-can-do-that-best-practices-for"
published_at: "2026-07-29T00:57:22+00:00"
first_seen_at: "2026-07-29T02:48:57.714245+00:00"
fetched_at: "2026-07-29T02:48:59.029198+00:00"
content_hash: "sha256:fbeb7b8b9be3c6b5cac303674bd32f3cae067592e7ec13f543284434d0cab7a5"
---

# We can do that! Best Practices for Dealing with Randomness in Language Model Outputs

# We can do that! Best Practices for Dealing with Randomness in Language Model Outputs


### How Expected Parrot embodies many best practices out of the box


[John Horton](https://substack.com/@johnjhorton)


Jul 29, 2026


t


**l; dr - A recent, [great paper](https://arxiv.org/pdf/2607.24372) discusses the problem of randomness in LLM responses and the problems in creates for research. EDSL / Expected Parrot deals with many of these issues.**


This is worth reading:


Many of the problems the paper identifies motivated certain design features in our Python package, EDSL. Rather than write out the connection myself (and insert my bias), I put Codex on the task, launching it from the current edsl repo directory:


Below are the responses, with some editorial comments from me (


JJH ), but the short version is Codex nails it:


<🤖> The paper’s central argument is that LLM-generated measurements should be treated as draws from a distribution, not as fixed data. EDSL is unusually well aligned with that view because it represents the entire data-generation experiment—prompts, cases, models, repetitions, and outputs—as structured objects.


Thanks for reading! Subscribe for free to receive new posts about our work!


## EDSL makes repeated measurement a first-class operation


The paper recommends measuring output variability instead of assuming that one call is representative. It specifically suggests repeated calls, potentially on a documented subsample when cost is a concern. The paper’s experiment repeats each classification 200 times, and its reporting standard asks for the number of draws, agreement measures, and sensitivity of substantive estimates. In EDSL, repetitions are part of the job specification


(JJH: and can be run from the command line or executed with a Python script) :


```text
ep run jobs.ep --n 200 --fresh --output results.ep
```


JJH: A “job” in EDSL jargon is a survey x scenario x agent list x model list. This can be serialized to an “ep” format, which essentially a git repo, zipped up (giving us version control on the object).


The resulting data contain an


` iteration.iteration` field, so each draw remains separately identifiable. Researchers can then export the repeated responses and calculate agreement rates, empirical response distributions, or a distribution of downstream regression estimates.


The


` --fresh` detail matters: a variability study should explicitly avoid substituting cached answers for new draws. EDSL records whether a response came from cache and its cache key, making this auditable.


## It preserves the prompts as executable, machine-readable artifacts


The paper says “prompts are code” and argues that screenshots are inadequate because they cannot be executed, searched, or compared. It calls for verbatim user prompts, system messages, and examples in the replication package. EDSL saves the actual rendered prompts with every result, including separate fields such as:


` - prompt.q0_user_prompt - prompt.q0_system_prompt`


This is better than saving only a prompt template. If a question contains scenario values, agent traits, prior answers, or survey instructions, the result contains the prompt that was actually sent for that observation. The underlying


` Survey` ,


` AgentList` , and


` ScenarioList` preserve the reusable experimental design as well.


That gives a verifier both:


-


the source specification used to regenerate prompts; and


-


the observation-level rendered prompts used in the original run.


JJH: Another way of putting it is that EDSL preserves both what was actually sent to the model as a prompt, as well the template it was constructed from, as well as the data used to populate that template.


## A saved Jobs object captures the complete experimental design


The paper asks replication packages to provide a route for rerunning the model-generation step. An EDSL Jobs object bundles:


-


the survey and its question wording


-


survey instructions and flow;


-


agents and their instructions or traits;


-


scenarios containing the input data;


-


models and their generation parameters.


A durable


` .ep` package can therefore serve as the machine-readable specification of the LLM measurement procedure:


```text
ep validate --file jobs.ep
ep inspect jobs.ep
ep run jobs.ep --fresh --output regenerated-results.ep
```


This reduces a common source of replication failure: code, prompt files, input data, and model settings drifting apart.


## Model configurations distinguish provider, model, and parameters


The paper’s minimum reporting standard includes the provider, exact model identifier, access route, and generation settings—especially temperature. EDSL model specifications serialize the inference service, model name, EDSL version, and model parameters. For example:


```text
{
“model”: “gpt-4o”,
“inference_service”: “openai”,
“parameters”: {
“temperature”: 0,
“top_p”: 1,
“max_tokens”: 1000
}
}
```


Researchers can construct heterogeneous model lists without hiding those choices in application code:


```text
ep models create \
--model-spec ‘{”model”:”model-a”,”service”:”provider-a”,”parameters”:{”temperature”:0}}’ \
--model-spec ‘{”model”:”model-b”,”service”:”provider-b”,”parameters”:{”temperature”:0}}’ \
--output models.ep


```


This supports both transparent reporting and robustness checks across models or providers. It also helps researchers follow the paper’s recommendation to use temperature zero when sampling is unnecessary—without implying that the result is deterministic. As the paper emphasizes, temperature zero does not eliminate silent model updates, batching effects, floating-point variation, or expert routing.


## Results preserve raw output alongside processed answers


The paper calls the deposit of every raw model output “the most important item” in a replication package. That permits exact reproduction of downstream analyses even if the original model changes, becomes unavailable, or becomes prohibitively expensive.


EDSL Results distinguish among:


-


the parsed answer;


-


the generated text;


-


the provider’s raw response;


-


validation status and comments;


-


token usage and cost.


For example, a result can contain both


` answer.q0` and


` raw_model_response.q0_raw_model_response` . This separation is important: a researcher can archive the original provider response, inspect how EDSL parsed it, and rerun downstream cleaning or analysis without making another model call. This directly supports the paper’s two proposed verification paths:


1. Reproduction from archived outputs: take the saved EDSL Results as given and rerun parsing/statistical analysis.


2. Fresh replication: rerun the saved Jobs specification and compare the new draws with the archived distribution.


JJH: All model outputs are stored in our[Universal Remote Cache](https://www.expectedparrot.com/cache-stats) and so if someone else tries to replicate your paper, they can do so at no cost: when they re-run, they’ll hit the cache, which doesn’t cost anything.


## EDSL exposes caching rather than making it invisible


Caching is valuable for ordinary reproducibility because it lets a researcher recover previously generated measurements without paying for another call. But it can invalidate a variability experiment if cached responses are mistaken for independent model draws.


EDSL addresses both needs:


-


fresh requests genuinely new inference;


-


Results record cache_used and cache_keys;


-


saved outputs allow the original response to be reused deliberately.


This makes the distinction between “reproduce using the archived measurement” and “draw a new measurement from the model” explicit—the same distinction made by the paper.


## It supports systematic sensitivity analysis


The paper shows that LLM variability can propagate into downstream regression coefficients and even change whether a result crosses a significance threshold. EDSL makes relevant experimental dimensions composable:


scenarios × agents × models × iterations × questions


Researchers can therefore vary:


-


temperature or other generation parameters;


-


model and provider;


-


prompt formulation;


-


agent instructions;


-


input cases;


-


repeated draws.


Because these dimensions are stored as structured result columns, outputs can be grouped and compared without reconstructing experimental conditions from filenames or logs. This is a strong foundation for agreement statistics and specification-curve-style sensitivity analysis.


## It makes cost reporting and affordable pilot studies easier


The paper treats token counts, monetary cost, and runtime as required replication-package information. It also recommends using repeated calls on a subsample when full repetition is too expensive. EDSL provides:


```text
ep jobs cost jobs.ep --iterations 20
ep results cost results.ep
ep costs log --output costs.jsonl --actual-from results.ep
```


Individual Results include input tokens, output tokens, price information, and calculated cost. Researchers can estimate a repeated-measurement design before running it, conduct a pilot on a smaller


` ScenarioList` , and retain an actual-cost ledger afterward.


## Structured inputs improve provenance


For annotation and extraction research, EDSL scenarios provide a stable mapping between an input observation and its model calls. Rather than interpolating documents into ad hoc strings and saving only the response, a researcher can store document identifiers and content in a ScenarioList. Results then retain the scenario, rendered prompt, model configuration, iteration, and response together.


That makes it much easier to answer:


-


Which input generated this answer?


-


Which exact prompt was sent?


-


Which model configuration handled it?


-


Was this the first or twentieth draw?


-


Was the answer retrieved from cache?


-


What raw response was parsed?


## Important limitations


EDSL helps implement the paper’s standard, but it cannot solve provider-side nondeterminism. In particular:


-


EDSL cannot preserve proprietary model weights or prevent silent updates.


-


A provider’s public model name may not be an immutable checkpoint.


-


Hardware, batching, routing, safety configuration, and provider fingerprints are only available if the provider exposes them.


-


The Results fields I inspected do not establish that query date/time, exact provider-returned model version, fingerprint, or total wall-clock runtime are universally captured.


-


JJH: Good point! We’ll add all this!


-


EDSL does not by itself containerize a local inference stack or archive weights and their cryptographic hashes.


-


It facilitates variability analysis, but researchers still need to calculate and report agreement measures and the sensitivity of their substantive estimates.


-


JJH: We can/should build a package for doing this to sit on top EDSL.


-


So the most accurate claim is: EDSL does not make proprietary LLM inference reproducible. It makes LLM-based research substantially more transparent, archivable, repeatable, and auditable—and gives researchers the machinery to measure irreproducibility instead of concealing it.


That is very close to the paper’s conception of best practice. The strongest opportunity for EDSL would be a dedicated “reproducibility report” export that automatically summarizes model identifiers and parameters, rendered prompts, query dates, cache use, token counts, costs, repetitions, agreement statistics, and missing provider metadata against the paper’s Table 2 checklist.


</🤖>


Want to use EDSL in your own workflows?


[Try Expected Parrot!](https://www.expectedparrot.com/)
