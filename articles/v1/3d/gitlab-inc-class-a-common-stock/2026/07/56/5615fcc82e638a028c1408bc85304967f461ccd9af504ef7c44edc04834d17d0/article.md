---
schema_version: "1.0.0"
document_id: "5615fcc82e638a028c1408bc85304967f461ccd9af504ef7c44edc04834d17d0"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/gitlab-transcend-hackathon-orbit/"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-20T13:34:12.049420+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:e7c5b3c94f879bda447006a0b75a99c4340243409b57e1ebd1144a4da68e76e4"
---

# GitLab Transcend Hackathon: What developers built on GitLab Orbit

We gave a few thousand developers GitLab Orbit. Then we got out of the way. The community responded with creative solutions to real production problems slowing down their teams. **The same problems you hit every week: What does this change break, which tests actually matter, and what will this migration really cost.**


[GitLab Orbit](https://about.gitlab.com/gitlab-orbit/) is a live, queryable graph of your code, merge requests, pipelines, deployments, and ownership, with every relationship between them kept current. Agents write code well but are bad at understanding the system around it. Answering the connecting questions (" *what depends on this* ," " *which tests cover it* ," " *who owns the fallout* ") used to mean an agent crawling files or an engineer digging across four tools. Orbit turns that into one query. Agents reach Orbit through Model Context Protocol ([MCP](https://about.gitlab.com/topics/ai/model-context-protocol/) ) ), engineers query it directly, and the answer comes back in seconds instead of hours. The[Orbit home page](https://about.gitlab.com/gitlab-orbit/) has the full story. We wanted to see what people would build once that was theirs.


The hackathon drew 1,576 registered developers, and they shipped 265 eligible Showcase Track projects built on GitLab Orbit: agents, flows, and skills. Separately, 26 contributors merged 61 improvements straight into the Orbit codebase, adding language support, fixing bugs, and sharpening the docs. The community did not just build on Orbit. It made Orbit better.


## The problems people reached for


Before scoring anything, one thing jumped out to us: If we sorted the submissions by what they actually do, the distribution would be lopsided.


**Seventy teams** built a version of the same tool: *Tell me what this change could break before I merge it* . More than 30 built onboarding and comprehension aids: The *help me understand this codebase* problem. After that came incident root-cause, architecture drift, flaky-pipeline diagnosis, and tracing a CVE across repositories.


None of that is random. Engineers ask these questions constantly and answer them poorly because the answers used to be scattered across Git, CI, deploy tooling, and a few dashboards nobody trusts. Put it in one graph, and people go straight for it.


That is orchestration with context, not orchestration alone: speed, with control built in. And when dozens of independent teams reach for the same query without being told to, that is the signal a platform owner should read: The context is where the work actually happens. The bar was high in the crowded categories. Projects stood out by beating 68 other teams, or by going somewhere nobody else did.


## Technological implementation


**Winner:[Sankofa](https://gitlab-transcend.devpost.com/submissions/1054521-sankofa) .** Three agents, each triggered by a different moment in your day, all reading from GitLab Orbit. Open a merge request and Radar hands you the blast radius: downstream callers, affected pipelines, the team that owns the fallout. Get assigned an issue and Guide writes you a brief before you start. A vulnerability surfaces and Shield traces every path to it. The context arrives where you already are, and then gets out of your way.


> "When a security vulnerability gets reported, teams spend days manually tracing how far it reaches, because nothing connects the dots automatically."
>
>
> Lester K, Sankofa


Shield does it in one pass over the graph.


**Runner-up:**[Stayed Shipped](https://gitlab-transcend.devpost.com/submissions/1054106-stayed-shipped) . It asks a question your dashboards cannot answer: Of the changes your AI agents merged last month, how many are still in production? It tracks whether a merged change survives or gets quietly fixed forward by a senior engineer, invisible to every standard metric.


## Design and usability


**Winner:[Carver](https://gitlab-transcend.devpost.com/submissions/1062163-carver-the-migration-quoting-agent) .** Carver prices a legacy migration before you commit to it. You say what you want to move; it reads GitLab Orbit's dependency graph and gives you a quote: how many units, how long, in what order, where the risk hides. What wins this category is the output. Carver gives you one line per unit, sized and risk-flagged, and expands the detail only when you ask. Moving AngularJS to Angular comes back as roughly nine weeks of human effort against ten dollars of generation cost, with the untested load-bearing service marked in red so you know where to look first.


> "It checks Orbit and, if it doesn't find the service, it asks where the real code is."
>
>
> Anes Mulalic, Carver


That is why the agent refuses to invent a number it cannot ground.


**Runner-up:**[Marshal](https://gitlab-transcend.devpost.com/submissions/1062651-marshal-autonomous-migration-assistant) . Same territory, opposite approach: full autonomy. Declare one org-wide goal and it finds every affected repo through GitLab Orbit, sequences the work, and lands merge requests wave by wave, making sure no target drops.


## Potential impact


**Winner:[CrossCut](https://gitlab-transcend.devpost.com/submissions/1061837-crosscut) .** CrossCut runs only the tests a change could actually break. Open a merge request and it pulls the changed symbols, walks GitLab Orbit's call graph to find the true transitive impact, and builds a pipeline that runs those tests and nothing else. No model in the loop, no guessing, just traversal. On a big or cross-repo suite that trims the CI payload by 90% or more, which pays for itself almost immediately.


> "You need the call graph of the whole codebase to know which tests a change can reach. That is exactly what GitLab Orbit builds, so instead of guessing, we just ask the graph."
>
>
> Pritesh Kumar, CrossCut


**Runner-up:**[OrbitWeaver](https://gitlab-transcend.devpost.com/submissions/1061548-orbitweaver) . It does autonomous refactoring using GitLab Orbit's exact blast radius rather than vector similarity. It maps every affected file and edits them in dependency order, the cleanest argument for why a real graph beats fuzzy retrieval when getting it wrong means a red pipeline.


## Quality of the idea


**Winner:[Transcend](https://gitlab-transcend.devpost.com/submissions/1056751-transcend) .** Most teams queried GitLab Orbit directly, which is the right move for most questions. Transcend built a second reasoning engine on top of it using the semantic web stack, OWL, SPARQL, and RDF, to reach the questions the native API cannot express in a single call. Transitive closure. Joins that leave your codebase and pull in the world's structured knowledge. Its demo asks which knowledge-graph-embedding methods a codebase implements, and gets back the class names plus the papers that inspired them, the authors, and the years, joined live to the code. Treating Orbit as a foundation to build on rather than an API to call was genuinely new here, and that is precisely what this category rewards.


**Runner-up:**[Universal Agent OS](https://gitlab-transcend.devpost.com/submissions/1053916-universal-agent-os) . It builds the governance layer around agents rather than the agent itself: Interview first, plan before coding, keep the evidence, and force validation. As agents write a larger share of code, keeping them accountable is about to become the real problem. It is the same problem GitLab Orbit's context makes visible everywhere else on this list: Agents move fast, someone still has to answer for what they did.


## Contribute Track


The Contribute Track ran alongside the Showcase Track, and it was not peripheral work. Twenty-six contributors merged 61 merge requests straight into the GitLab Orbit codebase: language support for C++20 concepts, Go package declarations, Kotlin coroutines, and Ruby lambdas; ontology fixes; a SIGPIPE bug in CI; the first Orbit query tutorial; and documentation cleanups so the next person does not trip over` max_depth` versus` max_hops` . Nineteen earned a cash prize. All 26 earned swag credits.


Cash prizes went to the first 40 merged contributions, so this list is as much about speed as quality. These are the contributors who got a working change reviewed and merged before anyone else. Congratulations to this round's cash-prize winners:


[achalbajpai](https://gitlab.com/achalbajpai) ,[aishahsofea](https://gitlab.com/aishahsofea) ,[AlphaTheGoat27](https://gitlab.com/AlphaTheGoat27) ,[anushkrishnav](https://gitlab.com/anushkrishnav) ,[bartekp854](https://gitlab.com/bartekp854) ,[bhandari.varun04](https://gitlab.com/bhandari.varun04) ,[ChaitanyaManik17](https://gitlab.com/ChaitanyaManik17) ,[fa220](https://gitlab.com/fa220) ,[fongse](https://gitlab.com/fongse) ,[gatlavishweshwarreddy26](https://gitlab.com/gatlavishweshwarreddy26) ,[gkepas](https://gitlab.com/gkepas) ,[JonstonChan](https://gitlab.com/JonstonChan) ,[koves](https://gitlab.com/koves) ,[MattGaiser](https://gitlab.com/MattGaiser) ,[MatthewOscar](https://gitlab.com/MatthewOscar) ,[nexpectArpit](https://gitlab.com/nexpectArpit) ,[priyansh3133](https://gitlab.com/priyansh3133) ,[Vinayreddy765](https://gitlab.com/Vinayreddy765) ,[zidanesalim](https://gitlab.com/zidanesalim) .


Check out the full set of[merged contributions](https://gitlab.com/gitlab-org/orbit/knowledge-graph/-/merge_requests?scope=all&state=merged&label_name%5B%5D=orbit::hackathon) .


## What the community taught us


You learn something about a tool by watching what people build with it, before anyone tells them what it is for.


Nobody built a chatbot. The strong submissions all made the same move underneath. They took a question that used to mean an afternoon of digging across four tools. They answered it in one query.


- What breaks if I merge this?
- Which tests actually matter?
- What does this migration really cost?
- Is last month's change still alive.


**We did not ship those as features.** The community found them the moment the graph was in reach. That tells us more about GitLab Orbit than any benchmark could.


The full field is in the[project gallery](https://gitlab-transcend.devpost.com/project-gallery) if you want to dig around. Plenty of good projects did not fit into eight slots.


## What you can build on GitLab Orbit


Every winner above is one answer to the same question: What can an agent do when it reasons from first-party context across your whole system, instead of guessing from fragments? GitLab Orbit continuously maps your code, work items, merge requests, pipelines, deployments, and ownership into one graph, so agents and engineers draw from a single source of truth. You do not need a hackathon to start. Here are the patterns the community proved out, each grounded in a use case Orbit was built for.


- **Change-aware delivery: See what a change breaks before you push.** Ask GitLab Orbit for every downstream caller of a function, the pipelines it feeds, and the team that owns them, in one query. Sankofa's Radar does this on a merge request; CrossCut takes the same call graph and runs only the tests a change can reach, cutting a CI run by 90%. Catch the hidden dependencies early instead of discovering them at the end of CI.
- **Safer migrations with full system context.** Ask which services depend on the one you want to move, in what order, and where the risk sits. Carver turns that into a priced quote; Marshal drives it repo by repo. Both scope the blast radius from the real dependency graph, so a platform team can commit to a migration date instead of discovering hidden dependents three weeks in.
- **Fast blast-radius analysis for vulnerabilities.** From a vulnerable function, follow the graph to every reachable endpoint, the pipelines that build them, and the teams that own them. Sankofa's Shield does in one pass what used to be a project-by-project correlation exercise across days.


The common thread: The answer was never in the code alone. It is in how code connects to pipelines, deployments, vulnerabilities, and ownership, and GitLab Orbit keeps those connections current so you can ask in a single query. Agents on the[GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) query it natively, external agents connect through MCP, and engineers query the same graph directly through the Data Explorer. One graph, one source of truth, for every agent and every person on the team.


## Try GitLab Orbit


To everyone who entered, thank you. If you want to build your own, start with[GitLab Orbit](https://about.gitlab.com/gitlab-orbit/) on the[GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) : The same context these teams used is already live in your software development lifecycle. We will do this again. Sign up at[contributors.gitlab.com](https://contributors.gitlab.com/) to hear about it first.


Already have access to the GitLab Duo Agent Platform and GitLab Orbit and want to see how far you can take it?[Apply for Co-Create](https://about.gitlab.com/community/co-create/) .


##


Is AI achieving its promise at scale?


[Get your AI maturity score](https://about.gitlab.com/assessments/ai-modernization-assessment/)


Quiz will take 5 minutes or less
