---
schema_version: "1.0.0"
document_id: "d405a016111ea16d1c83634caa9dd110536ddc5e3ed16f5664801477f4d3b5b4"
company_key: "yc-scale-ai"
company: "Scale AI"
source_id: "yc-scale-ai-news-import-179249eb8ad6"
canonical_url: "https://scale.com/blog/untangling-the-myth-and-realities-of-sovereign-ai"
published_at: "2026-08-06T05:00:00+00:00"
first_seen_at: "2026-08-06T12:35:34.646674+00:00"
fetched_at: "2026-08-06T12:35:35.742975+00:00"
content_hash: "sha256:67b8db084d06a87c4032a1e4aa4e9106bc79dff121dcb9a283b9708ce5e8d00d"
---

# The Cost of Control: Untangling the Myth and Realities of Sovereign AI

## Introduction


Last month’s abrupt shutdown of Anthropic’s Fable 5 model — ordered offline by a late Friday afternoon U.S. export control directive — was a Rorschach test in the debate over sovereign AI. To some, it confirmed the most visceral fears of leaders like French President Emmanuel Macron, who previously warned that the EU risks[becoming a “vassal” state](https://www.france24.com/en/live-news/20251118-merz-macron-to-push-for-european-digital-sovereignty) if it did not develop its own frontier AI capabilities. To others, the reality looked like a narrower, if ad-hoc, attempt to balance public safety and national security with the incredible pace of AI innovation.


Regardless of how the debate evolves, the temptation to respond with the bluntest tool available — onshoring the entire AI technology stack in the name of sovereignty — rests on a flawed premise: location is not control. When compute and model selection drive all the headlines, harder-to-see elements like integration, diffusion, talent, governance, trust, and partnerships get lost.


In hundreds of touchpoints over the past two years — spanning government, defense, and enterprise AI deployments — Scale has seen time and again that no country can fully sever itself from the interdependencies that come with building production-grade AI systems. These dependencies can, however, be successfully managed by making purposeful tradeoffs along a continuum of risk, cost, opportunity, and dependency. The Fable 5 crisis makes this more true, not less.


What follows in this paper is not a single prescription for governments looking to design for sovereignty. Rather, we offer a decision framework: first, whether a given use case genuinely requires sovereign control at all; second, if it does, which specific levers — compute, data residency, talent, governance rights — actually deliver that control, and at what cost.


## The Sovereign AI Dilemma


Building a strong sovereign AI strategy means understanding the tradeoffs that governments are already negotiating and implementing as they build out their AI stack. **It is rare that a decision affecting one element of the tech stack does not carry with it both benefits and costs.** The challenge is that while the benefits are often publicized, the costs — the risks of failure, the new dependencies created, the technological drawbacks — are far more rarely enumerated.


***“It is rare that a decision affecting one element of the tech stack does not carry with it both benefits and costs.”***


Today, the countries that Scale believes are leading the world with sovereign AI buildouts are not those focused on “owning” layers of the AI tech stack as if they were one-time exercises. Instead, they're purchasing capabilities to solve current problems, managing dependencies within and across the stack, and treating the process as a cycle of routine reevaluation rather than a single decision. This forces governments and companies to get specific about where they want to add value, what they need to protect, and what they need to deprioritize. Ultimately, sovereignty is really just one axis of the decision: the other three are capability (leading-edge models, chips, and applications), cost (the drain on limited taxpayer funds, relative to the benefit derived), and deployability (taking advantage of technology available now, not cycles down the road).


In the age of AI, managing these tradeoffs is one of the strongest plays that a country can make in a rapidly changing environment. Making those decisions on a country’s own schedule — rather than on news cycle timelines or in response to a crisis — is what will separate solid national strategy from reactive gambles.


## Chasing Full Stack Sovereignty


The problem today is not that governments are failing to understand the imperative of sovereign AI: they plainly do. But the “how” is less well-developed. Many large tech companies offer a sovereign AI prescription that focuses on what they sell: infrastructure companies center[on national compute](https://blogs.nvidia.com/blog/what-is-sovereign-ai/) , cloud companies say that[control is in local datacenters](https://www.microsoft.com/en-us/sovereignty) , model builders sell national models as the path out of dependencies, and consultants tell governments that[“full control” of the stack](https://www.mckinsey.com/featured-insights/mckinsey-explainers/what-is-sovereign-ai) is the way forward. In this confusion about what is most important, governments sometimes reach for language that frames AI as a way to avoid “vassal” status, rather than to secure economic benefits or ensure privacy for their citizens. Defining AI sovereignty, quipped Stanford’s Institute for Human-Centered Artificial Intelligence earlier this year, is like “[trying to nail jelly to the wall](https://hai.stanford.edu/news/ai-sovereigntys-definitional-dilemma) .” This is because sovereignty is not one choice but many. Core motivations range from questions of data privacy, model access, and frontier performance to infrastructure efficiency, economic power, national security controls, human safety, national autonomy, cultural and linguistic competency, accountability and traceability of decisions, and even national pride. These are all meaningful aims, but they trade off against each other more than they reinforce each other, which is why no single lever can deliver “sovereignty” on its own. Each one buys control of elements of some layer of the AI stack, and each comes with a structural ceiling that no amount of investment can fully resolve.


To see why, it helps to walk through the stack layer by layer — because the tradeoffs, and the ceilings, look different depending on where a government chooses to intervene.


` THE TECH STACK`


**Energy** is the obvious resource constraint, as AI's power demands are growing so fast that grid capacity, not chips, may become the binding constraint on who can build frontier systems and run production-grade systems. Here, countries with abundant domestic energy — namely hydropower, nuclear, and natural gas — have a real sovereignty advantage. But that advantage takes time to convert into capacity: gigawatt-scale datacenters require significant water for cooling and years of permitting, transmission buildout, and local engagement to stand up successfully.


**Compute** receives the most attention because it is the most visible: chips, datacenters, and networking infrastructure built on domestic soil. But physical presence in-country does not mean local ownership or control. A government that spends hundreds of millions on a local datacenter may call the result a “sovereign” stack, but at that scale it's a small slice of domestic infrastructure — and often at higher cost per unit than leasing compute abroad, at a moment in which compute’s potential to trade as a commodity is increasingly of interest. These efforts also still depend on foreign IP: many advanced chips, for instance, require ongoing driver updates, security patches, and license servers, so even a “sovereign” buildout retains foreign chokepoints. The case for this kind of investment is more as a hedge for a country’s most sensitive workloads rather than a substitute for the broader AI stack.


**Model** access is another lever, and it offers roughly three choices: use foreign frontier models via API, adapt open-weight models like Llama locally, or train new models from scratch on local data. The first option is where governments most fear loss of control. As the Fable 5 shutdown showed, API access can be switched off by export controls overnight — the use of closed models requires trusting an external party’s servers, jurisdiction, and data-handling practices. Open-weight models appeal because they avoid this: a government can run them entirely on domestic infrastructure, without routing sensitive data through a foreign company’s cloud. Homegrown models, meanwhile, have mostly disappointed: with a few exceptions such as Singapore’s SEALION, they've been dismissed as failures. In fact, some research suggests they[often trail frontier models](https://arxiv.org/pdf/2510.14565) even on the cultural and linguistic competence they were built for. The result is that users are increasingly turning to efficient Chinese open-weight models instead — solving the current performance problem, but reintroducing the same bias and control issues that motivated homegrown models in the first place. Even so, this hedge isn’t as clean as it appears, as Beijing is reported weighing export curbs on overseas access to its top models,[including open-weight models](https://www.reuters.com/world/beijing-is-looking-curbing-overseas-access-chinas-top-ai-models-sources-say-2026-07-07/) .


**Data** is objectively one of the most controllable assets a country has: unlike chips or talent, it is generated domestically by default. Data is the layer of the tech stack that enables a country to be deliberate about its own values, norms, and cultural identity — either embedded foundationally, if the model is trained from scratch, or correctively, if local datasets are post-trained. Datasets are national assets that, when leveraged, can have one of the most significant effects on sovereignty. But “keeping data in-country” and “keeping data resilient” are not the same goal. Conflating them can undermine the privacy and security gains that motivate data localization in the first place. Mandating strict in-country data processing can concentrate risk instead of eliminating it — as customers of[OHVCloud in France learned in 2021](https://www.pcmag.com/news/ovhcloud-data-center-devastated-by-fire-entire-building-destroyed) , when a pivotal datacenter was destroyed by fire, or as[Ukraine learned in 2022](https://medium.com/@heinancabouly/europes-cloud-sovereignty-paradox-how-ukraine-s-survival-exposed-the-continent-s-digital-f31469182170) , when Russian missiles began targeting its datacenters. Data sovereignty strategies that rely on single-country localization and processing can be more fragile that architectures that distribute data and backups across a broader region.


**Applications** deserve more attention than they currently receive. Though they are often seen as downstream of the “real” infrastructure decisions like compute, models, and data, the process of building solutions often reveals the nuances and necessary decisions around sovereign tradeoffs that would otherwise remain theoretical. In fact, applications are one of the most accessible ways for a country to capture value without needing to control the lower layers of the stack, because control can be secured through governance and contract terms: audit rights, defined data handling, and guaranteed protections. The challenge is ensuring that these applications are designed, built, and maintained by experienced experts in ways that are not only compliant with local regulations but actively ensure local control, graceful degradation in the event of a failure, and security from the start. Thoughtful decisionmaking[separates success stories](https://scale.com/six-percent) from failures.


### ` THE HUMAN COMPONENT`


Beyond the technology stack itself, there are equally important considerations to be made about the ecosystem in which AI is developed and deployed.


**Governance** is where legislation must move at the speed of the technology it is meant to govern, while balancing diverse and often competing priorities. Strict procurement rules may protect against foreign dependence, but risk raising costs or cutting access to the best tools. Take cloud services: U.S. firms currently supply roughly[80% of the EU cloud market](https://www.cnbc.com/2026/02/13/four-charts-europes-reliance-us-digital-infrastructure.html) , a dependency European policymakers are grappling with right now. This pits national security against price and performance — clamp down too hard, and the EU risks losing ground on capability while costs climb; do nothing, and it risks deepening foreign dependence and forfeiting the upside of a homegrown cloud industry.


**Talent** investments photograph well: a new AI institute, a university partnership, or recruiting researchers across borders. But building a domestic talent base takes years. Furthermore, “AI talent” is not just one thing — an expert in model architecture doesn't necessarily scale into someone who can manage data pipelines, security, or deployment. In the meantime, most countries still need to rely on foreign expertise — whether through visas, consulting relationships, or outsourced technical teams — to fill these gaps and build and maintain frontier systems. Ultimately, tech talent is a cycle: attracting top technology companies helps retain top talent, which in turn fosters a demand signal to the workforce.


Of course, tradeoffs are not just a challenge for middle powers. The United States and China, for all their dominance in AI, have their own decisions to make. American enterprises have to toggle between efficiency and frontier performance: Microsoft[recently drew heat](https://www.axios.com/2026/06/16/microsoft-copilot-cowork-tokenmaxxing-cowork) for contemplating running Copilot on Chinese model DeepSeek. The Chinese government has apparently banned top-of-the-line NVIDIA chips — allegedly in[an effort to force domestic production](https://finance.yahoo.com/sectors/technology/articles/china-banned-nvidias-rtx-5090d-181645536.html) .


## Scale’s Framework


If the definition of sovereignty is inherently contradictory, and no single solution offers a full fix, how do government leaders achieve their desired control over AI? With a framework that forces considered answers to tough tradeoffs, their implications, and fail-safe options in the case of failures.


The following framework is based upon our work with AI-adopting governments around the world, and is designed to turn the daunting strategic question of AI sovereignty into a series of tractable decisions.


### ` STEP 1: ASSESS AND DEFINE THE PROBLEM THAT AI IS SOLVING FOR`


Putting AI to work means driving real return on investment instead of evaluating laboratory potential. This only happens when AI is deployed, integrated into workflows, and usable by the people inside an institution — a process far more labor-intensive than most consumers are conditioned to expect, having primarily experienced on-demand access to frontier LLMs via API.


*Key Questions:*


- *What is the impact of adding AI?*
- *Is this actually an AI question — or is it a broader question about geopolitics, technology, or economic effects? If the latter, what is AI’s specific role?*
- *What is in-scope and what is not? In other words, what are the fixed constraints that organizations need to work around, and what can they control and shape?*


Above all, governments need to be clear about what they are trying to achieve. Are they seeking to replicate the U.S. AI ecosystem? Wall off their military and national security functions? Provide public services? Lay the regulatory groundwork to facilitate large-scale uptake? Protect the private information that citizens enter into LLMs in exchange for “free” services? Jumpstart a new ecosystem? Some of these are feasible — but they still come with tradeoffs. Clarifying the primary objective helps prevent decisionmakers from trying to make the case for “all of the above, just in case.”


### ` STEP 2: DON’T STOP AT ASSETS — REVIEW AND MAP DEPENDENCIES`


Each objective from step one should be mapped, understanding not only how it implicates the layers of the tech stack and broader ecosystem — energy, compute, data, models, applications, talent, and governance — but how the technical, social, and financial tradeoffs play out.


*Key Questions:*


- *Where are the benefits, the risks, and the biggest single points of failure?*
- *Have governments mapped across the entirety of the stack, including talent, energy, data, and applications, or are they fixated on compute, governance, and models only?*
- *What are the available resources and fallbacks if something goes wrong?*
- *What happens if goals are delayed — and what risks develop before a project is completed?*
- *What are the second and third-order effects of a decision?*


***A foreign dependency that can be swapped easily is not a vulnerability, while a domestic dependency with no alternative is — even if it is owned locally.***


Not everything can be owned, nor should it: ownership and control are not synonymous. Here, governments need to be honest not only about their aspirations but their timelines, resources, and capabilities. A foreign dependency that can be swapped easily is not a vulnerability, while a domestic dependency with no alternative is — even if it is owned locally. The thing governments should most closely examine is the chokepoint: the core services or processes where leverage is concentrated and switching is costly or not possible. It may or may not be a full layer of the tech stack — for example, at the model layer, a country might default to a foreign model’s API for many use cases, but rely on several different providers.


### ` STEP 3: DECIDE AND NAME THE TRADEOFFS THE USE CASE IS ACCEPTING`


Here, naturally, decisions about human safety and national security will almost always receive a higher weight than economic or political wins. Hard safety and security goals are not fungible. A use case where a cutoff means citizens go untreated in national hospitals or the state cannot function is located in a different tier than one where a cutoff means a contract renegotiation or switching service providers.


*Key Questions:*


- *Of the risks, which are likely, which are catastrophic, and which are neither?*
- *Where can single points of failure be avoided from the start?*
- *Where is it necessary to hedge bets, and what is reliable enough for the use-case?*


There are diminishing returns on exponential capital investment, and each government has the responsibility to draw the line on where their need for control is non-negotiable and net-beneficial, and where the gains are marginal and fungible.


### ` STEP 4: PLAN AND DESIGN THE FALLBACK BEFORE IT IS NEEDED`


Every tradeoff acknowledged in step two and accepted in step three needs a mitigation strategy. This means that three things need to be set in advance: an alternative strategy, a known buffer period where switching takes time (compute reserves, the portability of data, existing local talent), and a trigger — the circumstances that require a country or organization to activate its contingency plans.


*Key Questions:*


- *What are the set of back-up options available, and how feasible are they in the short, medium, and long term?*
- *What is the cost of each fallback, at the time of design and in the moment of crisis?*
- *How is technology evolving — and what are the implications for each?*


Capabilities across the layers of the stack evolve at different rates. Of these, talent is the slowest buffer to build. Compute can be reserved and data made portable inside the window a crisis allows, but the capacity to run an alternative has to exist before an emergency triggers it.


### ` STEP 5: REVISIT THE PLAN — BECAUSE DEPENDENCIES DRIFT`


AI technologies are evolving rapidly. The frontier model that is a chokepoint today may be commoditized in a year. Demand for compute may drift to the edge. Dependencies that seem survivable now may worsen as use cases scale and underpin critical infrastructure.


*Key Questions:*


- *Where does the trajectory of technology development mean that dependencies will increase or decrease?*
- *What are the longer-term infrastructure gaps or capabilities that solving for use cases highlights?*


This framework will not resolve every disagreement about risks, opportunities, or outcomes. Instead, it is intended to show that sovereignty should be understood as a set of decisions governments retain the ability to make, not a point-in-time, yes-or-no status.


The governments building from this understanding are not trying to copy over, wholesale, the choices the United States or China made to build their stacks. They aren’t privileging sovereignty over the practical realities of AI delivery. Instead, they are managing their dependencies, building toward their competitive advantage, and planning for a rainy day — preparing to defend against unplanned coercion, undue bias, or an infrastructure cutoff by an external actor.


## **Conclusion**


*The countries that fare best will not be the ones that move fastest, but the ones that move deliberately: naming their no-fail use cases, mapping the dependencies underneath them, and building fallbacks before a crisis hits.*


The urgency of the current AI moment, compounded by fast-breaking geopolitical events, is increasing the pressure on governments to publicly prove that they are acting quickly and decisively.


In this environment, investment plays that replicate foreign infrastructure on domestic shores or that label models as homegrown may feel like the right move. But in the absence of a considered strategy, they risk failures and unacknowledged tradeoffs that, over time, can deepen the very dependencies they were meant to escape.


The countries that fare best will not be the ones that move fastest, but the ones that move deliberately: naming their no-fail use cases, mapping the dependencies underneath them, and building fallbacks before a crisis hits.


Handled well, then, the breakneck developments in how AI is made available and who controls inputs to levels of the technical stack might turn out to be a moment of opportunity: a chance for global leaders to pressure-test their sovereign AI plans against a real shock and on their own timeline. Because what comes next may not be nearly as low-impact as the current crisis — which is why the work of naming tradeoffs, not just owning infrastructure, has to start now.
