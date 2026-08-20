---
schema_version: "1.0.0"
document_id: "d2e288371160cf4a4f9f78a10145a1f36914a1966ec9c35b16b047515651ef6e"
company_key: "yc-butter"
company: "Butter"
source_id: "yc-butter-rss-cfe99054af6a"
canonical_url: "https://blog.butter.dev/on-automatic-template-induction-for-response-caching"
published_at: "2026-01-07T04:51:07+00:00"
first_seen_at: "2026-07-24T22:18:01.434032+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:ebebdb3e30a3788bae317e14a74fa5c6905ded4f37e757d3ee8c545a7d86ef17"
---

# On Automatic Template Induction for Response Caching

As of last week, Butter’s proxy now offers automatic template induction for its response cache! We’ve prepared the following blog post to help explain its significance and potential to help you serve more LLM responses from cache. You can also read through our[documentation on template-induction](https://docs.butter.dev/concepts/template-induction) .


---


[Butter](https://butter.dev/) is a cache for LLM responses, sitting as an HTTP proxy between clients and LLM inference endpoints.


One of Butter’s central goals is to develop a system of serving LLM responses from cache in a way that is:


-


*Fast* at the time of request,


-


*Accurate* enough to avoid both false positives and false negatives, and


-


*Powerful* enough to achieve a high cache hit rate.


Our main strategy for doing so is via *template-aware response caching* , something we discussed in[an earlier post](https://blog.butter.dev/template-aware-caching) . We’ll cover it again here, as well as talk about the challenges involved in automating it.


Rather than storing user-agent messages verbatim, a template-aware response cache stores templated messages, or *templates* . This allows messages in the cache to generalize, thanks to the introduction of variable placeholders.


A message is considered an instance of a template if that template could be populated (that is, *hydrated* ) according to some *bindings* which specify the hard values to substitute for each template variable.


See the figure below for a simple example of inducing a template and bindings from a query.


Templates lend themselves to expanding the reach of Butter’s cache. Currently, when Butter receives a new query, it attempts to match that query to all the available templates by direct, syntactic comparison (i.e., by comparing against an appropriate regex pattern).


Below, we illustrate how this syntactic comparison works on a followup query which matches the template we induced in the previous figure.


Importantly, this kind of templated matching algorithm employed by Butter is both deterministic and syntactic, avoiding calling extra language models during request time. We continue to create powerful templating and data transformation tools which ensure users’ request-time hotpaths to be served truly deterministically and LLM-free.


## Butter’s Cache is a Tree


We should take a moment to clarify how Butter organizes its response cache (and hence, how it serves from cache). For simplicity, we’ll treat all messages in a user-agent interaction as if they either come from the user or the agent; ignoring tool calls and system prompts.


Butter’s caching is tailored to the turn-based structure of user-agent interactions: user queries are met with assistant responses, constituting a single “turn” of the interaction. Models like GPT-4o or Sonnet 4.5 are technically *stateless* between requests. So, in order for second, third, or later turns to operate under the context of prior turns, the user must make sure to pass along this prior context along with their latest request, all appended together in chronological order. We’ll call this style of managing context: *append-only* .


Append-only context management is the de-facto default for facilitating messages between users, agents, and tools, and is expected in the now-standardized Open AI Chat Completions format. Note that newer APIs such as Responses handles appending for you.


Under the append-only context management style, Butter’s cache may be thought of as a tree: each node in the tree is a new message in the thread; and distinct branches may spawn from the same node whenever Butter encounters distinct ways of continuing on from a shared context.


This helps us understand what happens when Butter “compares to cache:” It begins at the top-level of the tree and looks for a template matching with the first message in the context. It then seeks a child template of that node matching the second message, etc. Eventually, this process ends whenever a matching child cannot be found, or once it’s exhausted the context. If Butter indeed has the full context stored in the tree, it is ready to serve the corresponding response from cache.


Below is an example of how Butter compares an incoming query with context to its own cache. In this case, we observe a full cache hit.


Beyond the placeholders, the contents of a template essentially capture the structural content of a query: the tokens that inform the model *how* to respond to the query. Under template-aware response caching, differences in structural content are exactly what trigger branching in Butter’s tree. And so, diverging paths can really be interpreted as distinct workflows followed by agents. Extracting the structural content of a query is thus central to Butter’s caching process. Let us now see what stands in the way of doing this.


## Noise


LLM-based agents operate in messy environments. Their context may get cluttered with extraneous information or artifacts which do not serve in performing tasks. We’ll call these artifacts: *noise* .


**Noisy Query:**


```text
<?xml version="1.0"?> <div></div>


<!-- output -->


[2024-10-17 14:32:01] User logged in


====================


[AD] Special offer! <div></div>


```


Noisy environments pose a real obstacle to cache-based responding. If Butter caches a noisy query, there is little chance that a later instance of that query will contain exactly the same noise as the former, impeding Butter’s ability to recognize it as a cache hit.


Ideally, Butter’s cache stores the “ideal” (i.e., not noisy) version of the query, and any incoming query gets “de-noised” before getting compared to Butter’s cache.


**Possible De-Noised Query:**


```text
[2024-10-17 14:32:01] User logged in


Special offer!


```


One could identify certain punctuation, whitespace, or HTML tags as noisy, and manage to detect them by purely syntactic means. E.g., construct an appropriate regex pattern and filter all instances of these tokens from the query. We could use such noise detectors to *syntactically* filter out any noise present in queries.


```text
# Example: filter out any instance of the HTML tag <!DOCTYPE ...>


noisy_text =  "<!DOCTYPE html><html><body>Hello</body></html>"


regex_pattern =  r'<!DOCTYPE[^>]*>'
denoised_text = re.sub(regex_pattern,  ""  , noisy_text)


assert   denoised_text ==  "<html><body>Hello</body></html>"


```


Together, a de-noised query would ultimately be the form in which Butter prepares queries for comparison with (and storage into) its cache.


We can now show exactly how Butter serves from cache: once we find a template which matches the (de-noised) query, Butter syntactically deduces the bindings that would make this template hydrate to the query. Then, it may use those deduced bindings to hydrate the cached response template, yielding a full response.


Understanding noise to be any information which is not relevant to completing a task, we should point out that noise may be context-dependent. Exhibit A: **timestamps** .


Suppose a computer-use agent includes timestamps with every browser event or interaction it witnesses. Many of the workflows developed by the agent will not be time-dependent: the same “Submit” button that was clickable on` 2025-11-10T11:34:00` should still be present on` 2025-11-10T11:34:01` . So, these timestamps included in the response only muddy the context and prevent the corresponding cache entry from applying to future instances of the same workflow.


However, some other timestamps *could* indeed be relevant to the agent’s task. Any workflow that distinguishes between a weekday and the weekend, or between morning and night, will require some type of timestamp in order to proceed in its flow logic. Therefore, it is not appropriate to syntactically filter out timestamps indiscriminately.


Context-dependent noise (otherwise called *semantic noise* ) requires more sophisticated methods to detect and discern. So, for now at Butter, we choose *not* to apply any syntactic de-noising, and pass off the job of semantic de-noising until[Variable Induction](https://www.notion.so/On-Automatic-Template-Induction-for-Response-Caching-29a96babc1b28013b9ead23a461b960a?pvs=21) , below.


## Template Induction


We imagine every user query as begging for an LLM response. A proper response should make use of any relevant information found in that query (as well as in any previous context). Surely, not *all* content in the query is guaranteed to be relevant to responding. Templates should be robust to any of this irrelevant content. Moreover, some content in the query might indeed be relevant to *answering* the query, but not relevant towards deciding *how* to produce an answer to the query.


With this in mind, before Butter may add a message to the cache, we ask that it split the message into structural content (the template) and dynamic content (the bound variables). We sometimes describe this as *separating data from code* , or more concretely as performing *template induction* . \[See here for[Why “Induction?”](https://bramble-recess-9ca.notion.site/why-induction?source=copy_link) \]


Any information which may be abstracted out and bound to a variable (without affecting the workflow’s logic) acts like *data* in the message, whereas the rest comprises the *code* of the message.


-


**Data (dynamic content)** : tokens which are essential towards building a response but not essential towards deciding an algorithm for generating the response.


-


**Code (structural content)** : tokens which are essential for fixing a method for responding to the query.


Entirely structural messages might look like:


```text
Find the topmost element on the page and interact with it.


```


Templating parts of the above query wouldn’t make sense, since any changes would likely impact the workflow chosen by the agent for completing the task. For example, with a few swaps, the above command could have instead looked like:` Anthropomorphize the largest icon on the page and argue with it.`


Whereas highly dynamic messages might look like:


```text
Send Erik at erik@butter.dev the message: "Hey, nice to see you!"


```


Where portions like` Erik` and` erik@butter.dev` and` ”Hey, nice to see you!”` are mostly safe to templatize: most replacements maintain the same response method: to try to send an email containing some contents to some address and named recipient.


Structural messages require no extra templating before getting stored into Butter’s cache. It is the dynamic content which needs to be detected and get bound to variables.


Thus, template induction boils down to variable induction.


### Variable Induction


We’ve just mentioned how variables are useful as placeholders for dynamic content in a message.


But variables also serve a second purpose: as placeholders for semantic noise. This way, only some of the variables specified in the bindings may be useful for generating responses. The rest “mask out” any irrelevant information that can’t be screened syntactically from the message.


It is also possible that what registers as semantic noise at one turn may become relevant for responding in subsequent turns. So, it’s good that we keep semantic noise around in the bindings even if it isn’t useful presently.


Luckily, unlike in the case of syntactic de-noising, we can afford to employ some (slower) semantic analyses when inferring the variables of a message. This is because the caching process happens asynchronously from request time:


Variable induction asks “What to include in the bindings?” We’ll see below how our desire to more generally respond from cache—as well as our need to hydrate templates into full responses—restrict how we may do this.


### Bindings


The bindings carry all of the variable assignments. Naïvely, we could hope that each unit of data be assigned to a variable via the form:` {{var}} ↦ literal` . But, consider the following example query:


```text
Erik Dunteman works at Butter. Erik loves to code and to cook with butter.


```


The naïve approach might produce separate bindings for each piece of dynamic content:


` full_name` Erik Dunteman


` company` Butter


` first_name` Erik


` activity` code


` ingredient` butter


But we know` full_name` and` first_name` are not independent. We could even *derive* one’s first name from their full name:


` full_name` Erik Dunteman


` company` Butter


` first_name`` full_name.strip()\[0\]`


` activity` code


` ingredient` butter


That is, variables can be (syntactically and/or semantically) related. (But it’s not always obvious! Consider that the company “Butter” and the ingredient “butter” are nearly identical strings but semantically independent).


It is vital that we track these interdependencies, especially when a user or agent quietly applies a transformation to existing data in their message. For example:


```text
**User**: On what day of the week did the 1900s start?
****
**Agent**: The first day of the 20th century was a Monday.


```


While the user asked about “the 1900s,” the agent distinctly referenced the “20th century.” If we wished to treat the century as dynamic content in this exchange, we would have to explain how “20th” derives from “1900s.” Otherwise, the workflow would fail to properly generalize to other time periods.


So, our bindings might not only be storing literal assignments to some named variables. Instead, they may contain code specifying how to derive its value from other variables’ values. In practice, we make use of a coding agent to generate the code for all such derivations, and verify/sandbox that code appropriately. Importantly, a derivation must return a literal value given literal assignments for all its arguments.


For example, consider the following prompt, from which we have inferred some dynamic content.


We might propose a few inter-variable derivations where appropriate. For simplicity, we only provided the function signatures of each of the proposed derivations below:


In order for this system of interdependent derivations to always be resolvable, the bindings must satisfy a *DAG* (directed, acyclic graph) structure, since no variable should have a derivation implicitly depending on itself! The nodes of the bindings DAG would consist of all the bound variables. And a node` x` connects to another node` y` in this DAG if the variable corresponding to` x` is used in the derivation of the variable corresponding to` y` . In our example:


Any finite DAG will possess at least one root node (i.e., a node having no “incoming” arrows), meaning our bindings should always have at least one variable which does not depend on any other variables in order to be hydrated. So-called *independent variables* are necessarily bound to literals:` {{independent var}} ↦ literal` . The remaining *dependent variables* depend on other variables in order to be hydrated, and are thus bound to derivations treating those free variables as arguments.


Given a bindings DAG, it is straightforward to hydrate a template making use of variables from those bindings: we use topological sort to fix a *hydration order* for the variables in the bindings, starting with the independent variables, then any variables depending only on those independent ones, and so on. We hydrate all the variables to literal values, and then populate those values into the template.


## Automating Template Induction


Recall that whenever Butter observes novel contexts, it forwards the request along to the provider, and then asynchronously attempts to add the query-response pair as templates into the cache:


Our job at Butter is to automate the template induction process so we may respond to a variety of requests from cache. What might this take?


Separating data from code appears infeasible by purely deterministic means. In general, queries specified in natural language require some level of intelligence or high-level reasoning in order to be templated. \[See my[previous post](https://blog.butter.dev/template-aware-caching) for some examples.\]


That is, we expect automatic template induction to be a harder task than simply implementing a syntactic pattern matcher, grammar constructor, or text-embedding.


In particular, we should have a robust system for extracting variable data from messages.


### Possible Induction Algorithm


For instance, one could break this task down into the following algorithm:


**Algorithm for Inducing Variables** :


1.


**Set up Bindings** \[ *deterministic* \]: Inherit any bindings that may have been inferred from messages earlier in the query’s context. Otherwise, start with empty bindings.


2.


**Identify Dynamic Content** \[ *classification task* \]: Identify any substrings of this message which should qualify as dynamic content (either as data or as semantic noise).


3.


**Label** **Dynamic Content** \[ *naming task* \]: Propose semantically-relevant variable names for these substrings (consistent with any inherited variable names).


4.


**Arrange all Variables** \[ *reasoning task* \]: Fixing all inherited bindings as independent/literally bound, incorporate any newly-induced variables to arrange all variables into a DAG structure.


5.


**Derive each Variable** \[ *code-gen task* \]: For each dependent variable in the DAG, propose the code relevant to deriving that variable from its arguments.


Each of the above steps involving intelligence (i.e., Identify, Label, Arrange, and Derive) will require slightly different skills, and hence could be handled by distinct models and approaches. We might set up specialized agents called the **Identifier** , **Namer** , **Arranger** , and **Deriver** , to accomplish each step, respectively.


One of our prerogatives is ensuring that such a pipeline for performing variable induction is not prohibitively expensive: e.g., minimizing the cost and compute required for any LLM call we make.


### Worked Examples


In the following demos, I show off some cute examples of Butter’s automatic template induction at work. It cleanly performs one-shot generalization for messages involving arithmetic, string manipulation, and form parsing.


[https://youtu.be/bMppsIgOc8U](https://youtu.be/bMppsIgOc8U)


[https://www.youtube.com/watch?v=ORDfPnk9rCA](https://www.youtube.com/watch?v=ORDfPnk9rCA)


### Possible Tweaks


The version of automatic template induction we’ve developed in this blog post insists on doing so *for every novel query-response pair* . Should this prove to be too expensive, we might still deploy a modified, few-shot version of template induction more cheaply. That is, Butter may begin by directly caching exact messages throughout its cache. Then, there might come a time that Butter decides it is worthwhile to attempt to merge many children under a common node into one or more templates. Merges will attempt to induce generic templates from several examples of query-response pairs. And merges might be triggered by reaching a critical number of children under a single node, or by judging the similarity between the existing examples by way of evidence generated by lightweight language models or other inexpensive methods.


We may further save on costs by taking advantage of[Prompt Caching](https://platform.openai.com/docs/guides/prompt-caching) when designing the prompts used by the various semantic agents described above. That would involve front-loading their contexts with all the instructions that appear consistently across runs, and leaving the rest until the end of their prompts.


---


We continue to shed weight from and calibrate our template induction pipeline. Expect to see more from us as we observe how much it benefits our users’ cache hit rates.
