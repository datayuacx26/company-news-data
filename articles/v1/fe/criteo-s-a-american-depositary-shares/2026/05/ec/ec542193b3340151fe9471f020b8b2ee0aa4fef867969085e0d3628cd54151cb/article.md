---
schema_version: "1.0.0"
document_id: "ec542193b3340151fe9471f020b8b2ee0aa4fef867969085e0d3628cd54151cb"
company_key: "criteo-s-a-american-depositary-shares"
company: "Criteo S.A."
source_id: "criteo-s-a-american-depositary-shares-rss-02db2411825d"
canonical_url: "https://medium.com/criteo-engineering/our-experience-at-devoxx-france-2026-8aaf3044f28f"
published_at: "2026-05-21T07:21:00+00:00"
first_seen_at: "2026-07-20T23:17:33.645392+00:00"
fetched_at: "2026-08-20T03:05:58.983824+00:00"
content_hash: "sha256:585652d3049d25befd34ea5c591b23395ca2d71a4981e42d7aaecb7ed6a89b60"
---

# Our Experience at Devoxx France 2026

**Authors:**[Toufik Ider](https://www.linkedin.com/in/toufik-ider/) ,[Djoble Koutouan](https://www.linkedin.com/in/djobl%C3%A9-k-a9b668a5/?locale=en) ,[Aymen DELLY](https://www.linkedin.com/in/aymendelly/) ,[Mathieu Gilgenkrantz](https://www.linkedin.com/in/gilgenkrantz/) ,[Thanh Phuc LE](https://www.linkedin.com/in/thanh-phuc-le-339357b7/) and[Dat Phan](https://www.linkedin.com/in/phan-tien-dat/) .


From April 22 to 24, we were back on site at[Devoxx France](https://www.devoxx.fr/) with a booth and two talks from our Tech Ambassadors, Estelle Thou and William Montaz. This year’s edition had a great atmosphere, strong turnout, and one very clear trend: AI was everywhere, with sessions on LLMs and agentic systems filling up fast and driving conversations across the venue.


Between talks, our booth gave us plenty of chances to talk about what makes Criteo unique, share some of the AI initiatives happening in our tech teams, and connect with other engineers and curious attendees from very different backgrounds.


### Criteo Talks


#### Maternité dans la tech: Du témoignage à l’action


[Estelle Thou](https://www.linkedin.com/in/estelle-thou-5356b4132/) , one of our Software Engineers, talked about concrete strategies to better recruit and retain women in tech, drawing on the experience of maternity leave: safe spaces, HR support, mentoring, inclusive communication, adaptation of the return to work, and levers for improvement, aimed at managers, HR, and developers for greater inclusion.


#### Production Troubleshooting: boostez vos skills, une étude de cas


[William Montaz](https://www.linkedin.com/in/williammontaz/) , Senior Staff SRE, walked attendees through a real-world incident involving Spark jobs corrupting data. The session explored a rigorous technical investigation method: formulating and testing hypotheses, avoiding bias, and distinguishing between correlation and root cause. The goal was to learn how to effectively diagnose any complex incident, even in uncertain environments.


### Talks that stood out to our team


#### Stop à la dette documentaire: Industrialisez vos ADRs et READMEs avec Spec-kit


*Speakers:*[Alexandre Guillemot](https://www.linkedin.com/in/alexandre-guillemot-4032a3172/) *, Tech Lead at WeScale, and*[Axel Chauvin](https://www.linkedin.com/in/axel-chauvin/) *, Senior Engineer at WeScale.*


This session looked at documentation debt through a very practical lens: ADRs and project documentation should be part of the regular development workflow, not something left for the end. The spec-driven approach presented with Spec-kit was especially interesting, from specification and planning to tasks and implementation, with AI helping generate artifacts while developers remain responsible for reviewing and validating them. Another strong idea was the use of CI agents to check architecture rules, required ADRs, and consistency between code and documentation, with a scoring mechanism that can even reject a pull request when the ADR is missing or unrelated to the change.


#### Understanding Prompt Injection: Techniques, Challenges, and Advanced Escalation


*Speaker:*[Brian Vermeer](https://www.linkedin.com/in/brianvermeer/) *, Staff Developer Advocate at Snyk.*


Brian framed prompt injection less as traditional hacking and more as social engineering for LLMs. Through live demos, he walked through prompt leakage, structured output abuse, role-playing, multi-turn manipulation, payload splitting, obfuscation, delimiter confusion, and link smuggling, showing that newer models are not automatically safer and that combining simple techniques can still bypass guardrails or expose sensitive data. One of the clearest takeaways was the need to be careful when AI agents have access to broad tools, chat history, or sensitive context.


#### Shall we play a Game? LLM Security in Practice


*Speakers:*[Joseph Katsioloudes](https://www.linkedin.com/in/jkcso/) *and*[Xavier René-Corail](https://www.linkedin.com/in/xavier-ren%C3%A9-corail-2428431/) *, GitHub Security Lab.*


This workshop explored how to trick LLMs into revealing information they should not disclose, starting with a simple chatbot and ending with a more agentic setup based on GitHub Security Lab’s Secure Code Game. What made it memorable was how concrete it felt: some classic tricks are now blocked by major providers, but other manipulations still work, even on the latest models. The conclusion was both simple and important: prompt injection cannot be fully prevented, so the rest of the application has to be designed with that reality in mind.


*⚠️ This is the workshop from*[Devcon Spring 2026](https://tessl.io/devcon/)


#### L’ampoule qui en savait trop: voyage au cœur de la sécurité des objets connectés


*Speaker:*[Paul Pinault,](https://www.linkedin.com/in/paulpinault/) *Rtone.*


What started with a simple ESP32 lightbulb demo quickly became a deep dive into the hidden complexity of modern connected devices. The talk showed how compliance, secure updates, certificate chains, real-time clocks, and long-term support requirements turn a seemingly simple IoT product into a much larger engineering challenge, especially under European regulations such as the Radio Equipment Directive and the Cyber Resilience Act. It was a great reminder that while talking to the device may take only a few lines of code, building a secure and compliant connected product is something else entirely.


#### 7 ans au pays de Kafka


*Speakers:*[Fabien Alberi](https://www.linkedin.com/in/fabien-alberi/) *and*[Valérie Servaire](https://www.linkedin.com/in/valerie-servaire/) *, Michelin.*


This was an impressive look at how Michelin built and scaled its Kafka infrastructure over seven years, from factory floors to web and client-facing services. The session walked through the architecture journey from initial requirements to real-world design and showed how they achieved smooth, reliable streaming at scale.


#### Les design patterns agentiques dont vous êtes le héros


*Speaker:*[Guillaume Laforge](https://www.linkedin.com/in/glaforge/) *, Google.*


Guillaume offered a hands-on exploration of multi-agent systems that went beyond basic demos. The session focused on coordination strategies, routing mechanisms, and swarm-style collaboration, using LangChain4j and ADK for Java to show how scalable and resilient agent-based architectures can be designed and integrated in practice.


#### Docker Compose votre Dev Toolkit pour AI & Cloud


*Speakers:*[Nicolas De Loof](https://www.linkedin.com/in/ndeloof/) *and*[Guillaume Lours](https://www.linkedin.com/in/guillaumelours/?locale=en) *, Docker.*


This talk focused on the latest Docker Compose updates for LLM-based applications. The demos showed how to define models, connect to external resources through providers, and offload GPU workloads with Docker Offload, all within a more streamlined workflow that brings code, models, and execution closer together for modern AI projects.


#### Apprendre à notre IA à apprendre


*Speaker:*[Stéphane Philippart](https://www.linkedin.com/in/philippartstephane/) *, OVHcloud*


Stéphane Philippart brought useful clarity to the vocabulary around AI agents, covering concepts such as RAG, MCP, tooling, and agents. The key idea was to separate the reasoning engine from the runtime: the LLM acts as the brain, while the agent runtime handles state, memory, and tooling so the brain can interact with the real world. It was a strong reminder that most of the real implementation work for developers still lives in the runtime layer.


#### Standards et agents IA: un tour d’horizon de MCP, A2A, ADK et plus encore


*Speakers:*[Guillaume Laforge](https://www.linkedin.com/in/glaforge/?skipRedirect=true) *and*[Emmanuel Bernard](https://www.linkedin.com/in/emmanuelbernard/) *.*


Several attendees highlighted this session, and the common thread in their notes was clear: AI agents are moving from standalone tools to connected systems built on shared protocols. MCP was presented as the standard way to connect agents to tools and external systems, A2A as the communication layer between agents, and ACP as a structured messaging protocol, all pointing toward a more interoperable ecosystem. Frameworks such as LangChain4j and Quarkus were also highlighted as the scaffolding that helps teams implement these ideas without getting lost in protocol boilerplate.


#### L’Agentic Coding, nouveau territoire du Platform Engineering


*Speakers:*[Yankı Sesyılmaz](https://www.linkedin.com/in/sesyilmaz/) *and*[Julien Tanay](https://www.linkedin.com/in/julientanay/) *, Doctolib.*


This was another talk that came back in several recaps, and for good reason. Through Doctolib’s experience, it showed what happens when agentic coding becomes a real internal platform used by hundreds of engineers: spec-driven development, shared skills and plugins, telemetry, governance, and strong onboarding all matter as much as the models themselves. One takeaway stood out especially well: in such a fast-moving ecosystem, success is not just about building features, but about knowing what to standardize, what to govern, and sometimes what not to build at all.


#### PostgreSQL Analytics: It’s a Trap!


*Speaker:*[Lætitia Avrot](https://www.linkedin.com/in/l%C3%A6titia-avrot/) *, Docker.*


Lætitia challenged the familiar idea that PostgreSQL is only for transactional workloads and should be abandoned for analytics. A big part of the talk was about benchmark bias, especially vendor-created benchmarks that are designed to favor the engine they promote while ignoring general-purpose scenarios where PostgreSQL performs very well. Her conclusion was refreshingly pragmatic: the best engine for your data is often the one you already know how to run well.


#### Sous le capot d’une base de données analytique


*Speaker:*[Sylvain Wallez](https://www.linkedin.com/in/swallez/) *, Elastic.*


This session offered a deep dive into what really happens inside a query engine, using Elasticsearch and ES|QL as reference points. The talk walked through the full pipeline from parsing to logical planning, optimization, physical planning, and distributed execution, with a strong focus on predicate pushdown, vectorized execution, and static rule-based optimizations that reduce compute and data movement. One particularly interesting point was that the engine does not score multiple query plans against each other, but instead relies on deterministic rules to produce a single optimized plan.


#### Another World, une belle leçon d’architecture logicielle


*Speaker:*[Olivier Poncet](https://www.linkedin.com/in/olivier-poncet-33369010/) *.*


This talk revisited the architecture of *Another World* , the 1991 game created by Éric Chahi, and showed just how modern some of its design choices still feel today. The most striking example was the use of a custom virtual machine and instruction set, a concept that still echoes in today’s software world through technologies such as the JVM, .NET, and Docker. The session also covered compression, rendering, and sound, while reminding the audience that all of this came from one person working far outside the usual norms of the time.


#### De zéro à des milliards de traces, le tracing distribué chez Winamax


*Speakers:*[Anthony Maffert](https://www.linkedin.com/in/anthonymaffert/?locale=en) *and*[Nicolas Fidel](https://www.linkedin.com/in/nicolas-fidel/) *, Winamax.*


This session explored Winamax’s observability journey, from trace production to indexing, storage, and search, with open-source technologies such as OpenTelemetry, Quickwit, and Jaeger. The talk was also notable for its scale: 100% of the system is instrumented, an internal tracing SDK helps standardize implementation across hundreds of repositories, and daily trace volume reaches around 50 terabytes. For teams working on large, interconnected systems, it felt very relevant and easy to map to challenges we know well ourselves.


Devoxx France 2026 reminded us once again why this conference matters so much to our team. It gave us space to share what we do at Criteo, reconnect with the wider tech community, and come back with ideas that feel immediately relevant to the way we build, scale, and think about software today. From agentic platforms and AI security to documentation, streaming, observability, and database architecture, the event was full of strong conversations and practical lessons.


Most of all, we are leaving this edition grateful for the people we met, the talks we attended, and the energy around the booth throughout the event. We are already looking forward to the next one.


Check all the recordings from the conference here 👇


[Devoxx France videos](https://www.youtube.com/@DevoxxFRvideos)


---


[Our Experience at Devoxx France 2026](https://medium.com/criteo-engineering/our-experience-at-devoxx-france-2026-8aaf3044f28f) was originally published in[Criteo Tech Blog](https://medium.com/criteo-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
