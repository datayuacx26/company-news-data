---
schema_version: "1.0.0"
document_id: "112bf00cb0a439ce4de7c27e54e3e31fad6fefc2fe6855cde65efcaedd4de65c"
company_key: "draftkings-inc-class-a-common-stock"
company: "DraftKings Inc."
source_id: "draftkings-inc-class-a-common-stock-rss-016c40719db2"
canonical_url: "https://medium.com/draftkings-engineering/the-evolving-role-of-the-software-architect-b63c3d3104b3"
published_at: "2025-06-03T11:30:46+00:00"
first_seen_at: "2026-07-20T04:35:13.112015+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:fc461980f8e7c343fd23ba4f544ace0c48af8ba580a0b83f923262a4605786d4"
---

# The Evolving Role of the Software Architect

Software Architecture


Principal Engineer


Ai Engineering


Tech Leadership


# The Evolving Role of the Software Architect


[Alexei Krasner](https://medium.com/@skalik?source=post_page---byline--b63c3d3104b3---------------------------------------)


22 min read


·


Jun 3, 2025


--


In today’s fast-moving tech organizations, the role of the software architect is both misunderstood and more vital than ever. Over the past decade, many companies have moved away from traditional “Software Architect” titles. They now favor Principal Engineers and Staff Engineers as the senior individual contributors guiding technology. (Interestingly, not all companies agree on the seniority of those roles, but I’ll use them as common examples for simplicity.) On the surface, this seems like a semantic change — a simple rebranding. In reality, it reflects deeper cultural shifts in how we approach system design and technical leadership. Yet despite title changes, the need for architectural thinking has not diminished. The business challenges (and product requirements) in the last 10–15 years brought the usage of many emerging technologies/patterns/frameworks/methodologies like Microservices and Event-Driven Architectures, Cloud Computing (with Serverless and Edge), Containerization Orchestration, DevOps/DevSecOps/MLOps (all with large portions of IaC), Big Data and Data Engineering, ML, AI, and many more. This article (or manifesto if you wish) explores the evolving role of software architects in modern organizations, merging theory with hard-won practical lessons. It advocates for an empowered architectural function that addresses complexity, aligns with business strategy, and charts a future-ready path in an age of constant change.


Press enter or click to view image in full size


## From “Architect” to “Principal Engineer”: A Shift in Title, Not Purpose


It wasn’t long ago that large technology organizations commonly had designated Software Architects — individuals or teams tasked with defining the system’s architecture and guiding the engineering teams during implementation. However, many companies became wary of this traditional architect role, which was often negatively portrayed as the “ivory tower” architect — someone who created diagrams in isolation and then simply passed them on to the development teams to build.


This skepticism was, in some ways, justified. There were many cases where architects became too detached from the everyday realities of coding, leading to designs that were not really practical. In response, companies started to move away from formal architect titles, preferring senior engineers who remained hands-on with the code. The specific title of ‘Architect’ became less prevalent, even though the essential architectural responsibilities continued to be very important and were being fulfilled by engineers at the Staff/Principal levels, expected to balance their coding work with broader system design responsibilities.


Having observed this evolution firsthand as a leader, executive, or in prior architecture roles, my viewpoint is one of nuanced appreciation coupled with a degree of caution. On one hand, the renewed emphasis on the engineering discipline within architectural responsibilities has undeniably helped mitigate the pitfalls of overly naive and impractical planning tendencies. This shift has also facilitated a valuable career path for senior technical contributors to assume leadership without becoming detached from the realities of implementation. Conversely, simply altering job titles does not inherently eliminate the fundamental need for dedicated architectural focus. Designating an individual as a Principal Engineer does not automatically confer the requisite time allocation, cross-functional authority, or specialized architectural acumen necessary to effectively address strategic, big-picture architectural concerns. In some organizations, I’ve noted a tendency for the shift to swing excessively in this direction, with “no architects, only engineers” becoming an almost dogmatic principle. This can inadvertently lead to a situation of architecture by committee — or worse, by happenstance — where no single entity is explicitly responsible for considering the broader system design beyond their team or microservice. In most cases, decisions are driven by immediate needs like solution delivery, feature implementation, or specific problem-solving. The potential ramifications of this approach include the development of incoherent systems, the inefficient duplication of efforts across teams, and the eventual necessity for costly and disruptive refactoring initiatives. While I don’t want to overemphasize refactoring as the primary theme here, it serves as a critical downstream impact of a poorly understood architectural function. Engineering teams often struggle to fully connect their past decisions and implemented solutions to the accumulation of debt, making it challenging to track, monitor, and quantify this relationship, especially given the potentially prolonged timeframe between cause and effect. Furthermore, the ability to clearly differentiate between general technical debt and debt that is fundamentally architectural in nature is often lacking, further obscuring the true cost of neglecting dedicated architectural oversight.


The core tenet remains that software architecture is fundamentally a function, a continuous set of essential activities, rather than simply a specific rank or title within the organization. Whether we formally designate individuals as ‘Architects,’ ‘Principal Engineers,’ or ‘Staff Developers’ is ultimately a matter of nomenclature. The critical aspect is ensuring that seasoned technical leaders are actively engaged in the process of architectural thinking on a daily basis: considering systems from a comprehensive, holistic viewpoint, making strategic design decisions that have an enduring impact, and providing a clear and compelling technical vision for the future direction of our platforms. In many organizations, the Principal Engineer effectively embodies the role of the software architect for a particular domain, bearing ultimate responsibility for its success and its technical evolution.


Senior engineers, including Principal and Staff Engineers, are undoubtedly capable of handling architectural duties and often do so with great proficiency. However, their primary day-to-day responsibilities usually focus on other critical matters, such as leading product initiatives, guiding team execution, and resolving immediate technical challenges. This inherent focus on more immediate concerns can often conflict with the continuity and dedicated focus required for effective and consistent architectural work. Architects, on the other hand, often dedicate a significant portion of their time specifically to these broader architectural considerations, ensuring a continuous and proactive (continuous and proactive are key here) approach rather than addressing them solely as momentary challenges arise. This dedicated focus allows for more consistent systemic thinking, proactive future planning, and effective alignment across disparate teams — a level of sustained attention that can be difficult for even the most capable senior engineers to maintain alongside their other crucial responsibilities.


## Navigating the Complexity of Modern Distributed Architectures


Over the last decade (and a half), software architecture has profoundly changed — not just because new technologies emerged, but because business and product requirements have become more innovative, ambitious, and demanding. Architectures must now inherently support scalability, agility, real-time interactions, personalized user experiences, global distribution, and robust compliance. Architects no longer deal merely with selecting frameworks or organizing code modules; their role is now to strategically manage unprecedented complexity, stitching together diverse technologies and methodologies into coherent systems capable of solving intricate business challenges.


To effectively discuss these new aspects, let’s look at three core areas of complexity that modern software architects must handle in large technology-driven companies, taking inspiration from my experience in building platforms for the dynamic world of Sports Betting and iGaming:


### Distributed Systems and Event-Driven Complexity


As products evolve from monolithic applications to highly distributed ecosystems, architects face new challenges around data consistency, latency, event management, and transactional integrity. Patterns like Microservices and Event-Driven Architectures, originally introduced to achieve scalability and autonomy, require architects to adopt entirely new ways of thinking about systems.


At DraftKings, for instance, the real-time nature of a Sportsbook involves streaming millions of odds updates per second, triggering events based on user interactions (such as bet placement), and synchronizing state across multiple platforms (mobile apps, web, backend services). Designing such systems isn’t simply about choosing technology — it’s about defining clear boundaries, effective data contracts, and resilient patterns to cope with volatility and unpredictability. Architects must strategically determine how to partition logic, manage eventual consistency, handle fault-tolerance scenarios, and ensure real-time responsiveness. This complexity didn’t originate from technology — it was driven by the innovative business requirement to deliver dynamic, engaging, and real-time user experiences.


### Infrastructure as Software and Operational Excellence


A second layer of complexity arises from how modern systems are deployed, operated, and managed. With practices like DevOps, DevSecOps, MLOps, and Infrastructure as Code (IaC), the responsibility of architects has expanded into what was traditionally considered “operations.” Containerization (Docker) and orchestration platforms (Kubernetes), combined with Cloud Computing and serverless platforms, by far not an exhaustive list, now allow rapid development and deployment but also introduce greater intricacy in configuration, security, and observability.


Consider an example: launching an operation in a newly regulated market at DraftKings involves not just building the product functionality but also architecting the entire infrastructure stack to comply with jurisdictional requirements, often demanding data sovereignty, isolation, auditability, and robust disaster recovery scenarios. Architecture must include reusable deployment blueprints, compliance automation frameworks, observability standards, and secure environments that balance rapid expansion with stringent regulatory demands. Here, again, architecture complexity emerges directly from business goals, enabling rapid expansion into new territories while maintaining regulatory compliance and operational excellence.


### Data-Intensive Architectures and Personalized Experiences


The shift toward personalization, recommendations, fraud detection, and other intelligent user-centric features has significantly raised the bar for handling data and analytics within modern systems. Concepts from Big Data, Data Engineering, and Machine Learning have become integral parts of system design rather than separate analytics functions.


At DraftKings, personalized user experiences in the Sportsbook, Fantasy Sports, or iGaming platforms aren’t just marketing gimmicks — they directly translate into customer engagement and revenue growth. Architecture must include seamless integration of data pipelines for real-time analytics, design data lakes and warehouses that facilitate both rapid ingestion and complex querying and incorporate ML models for recommendation or fraud prevention without sacrificing performance or introducing unnecessary complexity. The need for real-time personalization — such as offering tailored betting odds based on past behavior — drives us to design systems that can handle massive data streams, continuous training and inference loops, and strong data governance. This is driven not by the availability of technology, but by the critical business need to create compelling, tailored user experiences.


### Architecting with Business Complexity in Mind


It’s vital to underscore that architects aren’t chasing trends; rather, they leverage emerging technologies strategically to address evolving business complexity. At DraftKings, adopting microservices or event-driven designs wasn’t simply following an industry pattern — it was driven by the core need to build flexible products capable of handling massive spikes in traffic during key sports events. Similarly, embracing Kubernetes or IaC frameworks wasn’t about technological enthusiasm only; it was about rapidly deploying stable environments to support business growth and regulatory compliance. AI and ML integrations weren’t just tech showcases — they were (and will be in the future) deeply tied to delivering unique customer value, engagement, and operational excellence.


Thus, the real architectural shift isn’t about any specific technology — it’s about the expanded responsibility architecture now carries: managing complexity at scale, adapting rapidly to shifting business landscapes, and ensuring that each new architectural decision directly enables business innovation rather than merely reflects technological fashion.


## The Architect’s Evolving Responsibilities (and Why They Aren’t Going Away)


If we accept that architecture is still essential, how exactly does the architect contribute to a modern agile organization? What do they do day-to-day that a “regular” senior engineer might not? This section looks at the key responsibilities of software architects as they stand today — blending traditional expectations with new twists — and why these responsibilities are more important than ever.


### Strategic Technical Decision-Making and Alignment


One of the architect’s foremost roles is serving as a strategic decision-maker at the intersection of technology and business. Architects possess the unique ability to see both the detailed technical landscape and the broader business goals, enabling them to make informed decisions that balance immediate product needs with long-term organizational vision.


Architects often play a critical role in providing high-level estimates and making technical complexity visible and understandable to product and business stakeholders. Rather than hiding engineering complexity, they actively communicate the potential impacts of technical decisions on timelines, budgets, and resource allocation. This transparency facilitates better-informed decisions, aligning business expectations with realistic technical outcomes and mitigating misunderstandings early in the process.


When initiating entirely new product lines or strategic initiatives, architects collaborate closely with product management and business leaders from the outset. Their role here is pivotal: architects help define what is technically feasible, outline potential risks, identify critical technology decisions, and ensure alignment with existing systems or broader organizational strategies. This close collaboration between architecture, product, and business teams ensures foundational decisions are sound, reducing risk and increasing confidence as new projects launch.


A clear example of strategic architectural involvement is geographic or market expansion. Entering new markets or regions, especially heavily regulated ones, involves significant architectural considerations — from compliance and data sovereignty to latency management and disaster recovery strategies. Architects provide early input into these decisions, defining how systems should adapt or scale, which significantly influences timelines, cost estimations, and even the viability of expansion initiatives.


Architects also play a vital role in mergers and acquisitions (M&A). Experienced senior architects often participate directly in technical due diligence processes, evaluating potential acquisitions or partnerships. They assess architectural compatibility, integration complexity, compliance concerns, and identify risks that could undermine expected synergies. Their assessments provide critical insights to business stakeholders, directly influencing deal valuation, negotiation strategy, and post-acquisition integration planning.


Similarly, integrating new third-party providers, strategic partners, or suppliers involves architects from initial evaluation to final implementation. Architects evaluate the technical capabilities of partners, assess compatibility with existing systems, and define integration strategies, considering security, scalability, compliance, and maintainability. Their involvement ensures that partnerships add strategic value without introducing unforeseen technical debt or operational risks.


Importantly, modern architects influence without always having direct authority. They often operate by persuasion, education, and community leadership. By clearly communicating technical vision, facilitating productive discussions, and guiding collective decision-making, architects cultivate an inclusive environment. This collaborative strategic role ensures that architecture is not a dictatorship of one brilliant mind but rather a shared vision continually refined by many, with the architect acting as its steward and champion.


### Embedding Quality Attributes through Strategic Collaboration


A key advantage architects bring to the engineering organization is their unique ability to address quality attributes, such as security, scalability, performance, reliability, and resilience, not merely as isolated concerns but from an integrated, strategic perspective. Their vantage point, spanning multiple teams and product lines, allows them to proactively recognize shared challenges and facilitate solutions that provide lasting, reusable value.


Rather than tackling quality attributes individually, architects regularly engage with Infrastructure and Platform Engineering teams to collaboratively design foundational solutions that initially address specific, urgent domain needs. These solutions, however, are strategically designed for broader applicability across multiple domains or teams in the future. Architects, therefore, bridge immediate business requirements with long-term organizational goals, ensuring efficiency and reducing redundant engineering efforts.


For instance, an architect might help Infrastructure and Platform teams build a scalable, elastic infrastructure tailored initially for high-volume, real-time interactions typical in online sports betting scenarios. By ensuring the solution is flexible and generic enough to support other potential use cases, the architect maximizes organizational value beyond the immediate project. This approach reduces the future overhead of engineering teams facing similar challenges elsewhere in the organization.


Moreover, architects actively cultivate a culture of knowledge sharing and continuous learning across the organization. When a successful architectural solution is developed, architects communicate and distribute these new concepts across teams. This collaboration often occurs through architecture forums, guild meetings, or documentation of best practices, enabling rapid dissemination of architectural knowledge and promoting consistency in addressing common engineering challenges.


Architects’ unique organizational position — spanning multiple product teams, engineering groups, and business units — provides them with the perspective and influence required to align cross-team initiatives. By working closely with Infrastructure, Platform Engineering, and domain-specific teams, architects ensure that strategic initiatives addressing essential quality attributes remain aligned with overall organizational goals and technological direction.


The key point is that architects think about risk mitigation from day one. Every system has risks: the risk of being hacked, the risk of crashing under load, the risk of data inconsistency, etc. Architects identify the major risks for the system being built and ensure the design addresses them proactively. This doesn’t mean overengineering for hypothetical scale or gold-plating everything — it’s a balance. Good architects are pragmatic;​ they gauge what level of robustness is needed for the context.


This proactive, collaborative, and strategic focus on quality attributes distinguishes the architecture role from other senior engineering positions. Architects not only identify and solve technical issues as they emerge but also strategically enable the broader engineering organization to anticipate, manage, and consistently deliver high-quality, resilient, and secure systems aligned to long-term business objectives.


### Enabling Agility through Continuous Architectural Improvement


Software architecture is often misunderstood as a static discipline, focused on planning upfront and delivering comprehensive blueprints to implementation teams. This misperception sometimes leads to the idea that architectural work conflicts with agile methods, which prioritize adaptability, iterative delivery, and rapid response to change. In reality, modern architectural practice not only aligns closely with agile methodologies — it embodies them.


Architectural work is inherently an agile discipline because it is structured around systematic evaluation, continuous improvement, rapid feedback loops, and adaptability to changing requirements. Architects don’t merely respond to immediate product or engineering demands; instead, they proactively anticipate evolving business needs, evaluate potential impacts, and guide engineering teams through incremental implementation steps. This systematic yet iterative planning process ensures that the architecture remains flexible enough to accommodate future changes, thus enhancing the organization’s overall agility.


Unlike the outdated perception of architecture as a detached function (the earlier mentioned “Ivory Tower”) from practical realities, contemporary architects maintain close relationships not just with engineering, but also with product management, operations, marketing, customer experience, and compliance teams. Their broad and cross-functional collaboration enables architects to stay aligned with business strategies, emerging market trends, and shifting customer expectations. This continuous dialogue across departments allows architects to integrate new insights rapidly into architectural planning, ensuring that systems remain robust, relevant, and adaptable.


This doesn’t mean architectural work is spontaneous or unstructured — quite the opposite. Architecture is fundamentally an engineering discipline built on meticulous planning, systematic research, careful evaluation of trade-offs, and data-driven decision-making. Architects continuously research and evaluate new methodologies, frameworks, and technologies in advance of actual needs, enabling informed choices rather than impulsive reactions. This careful preparation means that when business priorities shift or new market opportunities emerge, architects are equipped with practical knowledge and viable options already evaluated and tested.


Another critical aspect is that architects maintain an organizational view, continuously identifying patterns, reuse opportunities, and efficiency gains across multiple product domains and teams. Rather than making isolated decisions optimized for individual projects or immediate challenges, architects identify broader patterns, architectural solutions, and best practices that can be leveraged repeatedly. This structured, engineering-based approach allows the organization to scale effectively, reduce redundant efforts, and incrementally improve the overall quality of systems.


Architects enable continuous improvement by incorporating agile engineering principles directly into architectural guidelines, including modular designs, loose coupling, incremental deployment strategies, experimentation, and automated testing frameworks. This ensures that architectural solutions remain practical, sustainable, and straightforward to refine and evolve. Architects consistently communicate these patterns to engineering teams, reinforcing the organization’s commitment to agility and continuous learning.


In many agile frameworks, architects are explicitly recognized for their strategic value, just like product, delivery, and engineering. SAFe, for example, clearly identifies architects at multiple organizational levels (solution, system, and enterprise), and not only SAFe. Architects in these roles continuously translate high-level business objectives into actionable technical strategies and clearly articulated engineering plans. They balance strategic direction and tactical implementation, systematically steering the organization toward long-term architectural objectives while enabling agile teams to deliver incremental, measurable value in shorter cycles.


## The Architect–Principal Partnership: A Strategic Collaboration


While this article has outlined the distinctions between Architects and Principal Engineers, it’s important to emphasize that these roles are not in competition — they are complementary and, in fact, mutually reinforcing. A future-ready engineering organization must rely on both to operate effectively, especially when navigating complexity, scale, and continuous change.


At DraftKings, we embrace this duality as a core part of how we structure our architecture function. Architects and Principal Engineers collaborate tightly, forming a continuous feedback loop between high-level vision and domain-specific execution. In most cases, the architect works directly with the Principal Engineer in a given domain or vertical. When no Principal is present, that collaboration naturally shifts to the next senior engineering role — in our case, typically a Senior Lead Software Engineer.


This collaborative model ensures that any architectural solution — be it for a new feature, an infrastructure upgrade, a response to technical debt, or an entirely new product line — is shaped by both a broad architectural perspective and deep local expertise. The result is not an isolated decision but a shared design process where accountability and ownership adapt to the complexity at hand.


Sometimes, the Principal Engineer leads the architectural effort for their domain, with the architect serving as a consultative peer and strategic reviewer. Other times, the architect is responsible for the solution, with the Principal deeply involved throughout the process. In yet other cases, the architect might only participate in key reviews and validations, leaving the implementation details and ownership to the engineering leadership. The degree of involvement is always calibrated to the complexity and impact of what we’re trying to achieve.


What makes this relationship work is not rigid definitions, but a shared understanding of purpose. Architects bring the context of the broader system, strategic alignment, and patterns that have worked elsewhere in the organization. Principals bring hands-on technical awareness, domain depth, and proximity to product and team dynamics. Both roles are essential. And it’s the dynamic interplay between them — the iterative refinement of solutions from both a strategic and tactical lens — that allows engineering organizations to scale without sacrificing coherence, velocity, or architectural quality.


The architect–principal relationship is the connective tissue that holds together the layers of system design, product development, and long-term technology strategy. In a world that increasingly rewards speed, adaptability, and intelligent system composition, this relationship is no longer optional — it is foundational.


Press enter or click to view image in full size


## From Code to Capability: The Architectural Shift Driven by AI and Abstraction


We stand at an extraordinary turning point in software architecture — one catalyzed by the rise of artificial intelligence, but whose roots extend far deeper into the very essence of human-technological evolution. Throughout history, advancements in software development have consistently been marked by abstraction: each new paradigm has moved us progressively away from hardware-specific instructions and toward human-centric expression. Today, AI accelerates this evolution dramatically, pushing architects to re-envision the very definition of what constitutes a “system.”


Philosophically, AI embodies a profound shift toward systems designed around human intentions rather than lines of code. AI-driven technologies — ranging from generative coding assistants to autonomous AI agents capable of decision-making, planning, and direct interaction with users — represent an unprecedented elevation in our level of technological abstraction. This abstraction fundamentally transforms our architectural mindset: we are no longer constrained by the granular details of implementations, but instead think in terms of orchestrating intelligent capabilities that closely mirror human reasoning and intent. In this new paradigm, architects serve as the critical link between abstract human objectives and the increasingly sophisticated technological landscape, carefully aligning powerful yet opaque intelligent capabilities with strategic business goals and human values.


This evolution provokes a deep reconsideration of the architect’s role. Traditionally, architects bridged human needs with technical implementations, turning business requirements into actionable system designs. Today, as AI and intelligent automation blur distinctions between human cognitive processes and technological execution, architects face an exciting but daunting responsibility: shaping software systems composed not merely of code or algorithms but of intelligent capabilities and behaviors themselves. Architects must grapple philosophically with unprecedented questions: How do we govern the ethics of intelligent systems? How do we ensure explainability and accountability when parts of the system are no longer entirely under human comprehension or control? How do we preserve human agency in an AI-enabled system landscape? These questions define the new frontier for software architecture, requiring architects to become not just technical leaders, but thought leaders — active participants in shaping the future relationship between humans and intelligent technologies.


Yet this philosophical evolution brings equally profound practical implications. As architects begin thinking at the level of intelligent capabilities, their approach shifts concretely toward solution-centric, composable architectures. Architects increasingly operate as curators and integrators, assembling solutions from pre-existing modules, intelligent services, AI components, third-party APIs, and internally developed systems. Rather than building from scratch, architects now define interfaces, establish contracts, and create modular structures that allow components to be combined and recombined flexibly. This practical shift profoundly influences existing architectural methodologies and patterns: loose coupling, event-driven designs, modularity, and clear contracts become much more essential, ensuring that the overall architecture remains stable and adaptable as individual capabilities evolve.


In the composable enterprise, architecture practice shifts from specifying detailed technical implementations to ensuring the coherence, interoperability, and resilience of solutions built from diverse components. Architects establish strategic guardrails and standards, such as common API structures, unified integration patterns, and consistent observability mechanisms, to allow rapid yet controlled evolution. The role of architecture extends to curating internal marketplaces or catalogs of reusable capabilities, which teams leverage to quickly and reliably assemble new solutions. Architects themselves become active participants in the continuous evaluation and improvement of this catalog, guiding strategic investment into reusable components or capabilities aligned with long-term organizational goals.


Moreover, architects will (if not already) face an expanded set of responsibilities that extend into ethical and sustainable considerations. Intelligent technologies pose ethical questions related to data usage, privacy, transparency, and accountability. Architects — uniquely positioned at the intersection of technology, business, and user experience — must proactively establish architectural frameworks that address these concerns. This broader ethical and social perspective is yet to be unpacked, but it will elevate architecture from purely technical considerations to genuine stewardship of organizational values and societal impact.


This evolution reflects well on the true purpose of the architectural role: shaping the relationship between humanity and technology. As the level of abstraction rises, architects, ever pragmatic yet visionary, are transforming from code-centric toward capability-centric thinking, driven by AI and abstraction. Philosophically, architects must grapple with AI’s ethical and human implications, practically, they must adopt composable, modular patterns to rapidly assemble reliable solutions aligned to strategic business goals.


Press enter or click to view image in full size


## Architecting the Future Organization: Toward Continuous Architectural Excellence at DraftKings


As software architecture continues its evolution toward higher abstraction, intelligent capabilities, and solution-centric design, traditional ways of structuring architectural practices must be reconsidered. At DraftKings, architecture is no longer purely a technical function, isolated into a single team, or tied solely to specific domains. Instead, we’ve consciously structured architecture as a strategic capability embedded throughout the organization, designed deliberately to support agility, abstraction, and continuous improvement.


Future-ready architectural structures rely less on rigid hierarchical lines and more on carefully orchestrated interactions among different types of architectural expertise. Within DraftKings, we’ve intentionally chosen clear yet flexible terminology — Embedded Architects and Global Architects — to avoid restrictive definitions like “Enterprise Architect,” “Systems Architect,” or specific frameworks such as SAFe’s predefined architectural roles. This deliberate simplicity aligns perfectly with our philosophy: maintaining flexibility and adaptability within the architectural function itself.


### Domain-Embedded Architects at DraftKings


Our embedded architects maintain deep, ongoing relationships within defined business or technical domains. Their role balances stability and responsiveness, focused on translating evolving product and business needs into clear architectural pathways. Rather than designing solutions in isolation, embedded architects engage daily with product owners, engineers, operational teams, and stakeholders from various functions. Their continuous involvement allows them to proactively shape the architectural “runway” ahead of immediate product demands, ensuring architectural changes happen just in time rather than reactively. Embedded architects serve as trusted experts and mentors within their domains, embedding strategic architectural thinking directly into the teams responsible for delivering software.


### Global Cross-Domain Architects at DraftKings


Complementing embedded architects are our global architects, strategically positioned to operate at a higher level of abstraction and across multiple domains. Their role isn’t bound to specific product lines but spans horizontally across the entire organization. Global architects lead strategic, cross-functional initiatives such as mergers and acquisitions, major product integrations, long-term technology evaluations, and partnerships, guiding DraftKings through complex decisions involving multiple business units, technological ecosystems, and external providers. These architects maintain a future-oriented perspective, continuously scanning the technology landscape for emerging trends and proactively integrating them into our broader architectural vision.


Together, embedded and global architects create a dynamic interplay that effectively bridges immediate tactical needs and long-term strategic goals. Global architects leverage deep domain insights provided by embedded architects to inform high-level strategic direction, while embedded architects draw on global architects’ broader perspectives to anticipate shifts and proactively adopt innovative architectural solutions.


### The Capability Marketplace: Architecting Through Composable Capabilities


In this forward-looking architectural organization, the internal capability marketplace concept emerges as a central, defining innovation. Rather than limiting architecture to the specification of technical details and prescriptive designs, architects curate and maintain an internal marketplace of reusable and composable software capabilities. Each capability represents a cohesive, abstracted solution or intelligent functionality that teams can integrate seamlessly into their products, ranging from intelligent decision-making components powered by AI to standardized modules for security, compliance, or personalization.


The capability marketplace embodies the architectural shift from code-centric implementation to capability-oriented abstraction. It allows architects to manage complexity, maintain consistency, and enable rapid innovation simultaneously. Architects oversee the marketplace, setting strategic guidelines for interoperability, quality standards, security considerations, and ethical usage policies. Teams across the organization benefit from easy access to trusted, reusable capabilities that accelerate delivery and ensure architectural coherence without sacrificing creativity or autonomy.


Architectural governance in this model moves away from top-down enforcement and toward collaborative stewardship. Architects no longer solely control the architectural landscape but instead guide and empower product teams, helping them navigate strategic trade-offs, integrate complex capabilities, and adopt emerging technologies responsibly. This form of governance ensures that as the technology ecosystem continuously evolves, the organization remains capable of integrating new capabilities rapidly and sustainably.


To be candid, DraftKings is very far from implementing such a capability marketplace. However, this type of theoretical idealism and forward-thinking is precisely what drives our architectural vision. It’s this ambition and clarity of direction that gives me confidence that we will indeed reach this future.


## Conclusion: Architects as Visionaries in an Age of Abstraction


At the heart of this article lies a fundamental truth: the essence of software architecture has always been about connecting human intent to technical realities. Historically, this connection was tangible, bound tightly to lines of code and physical infrastructures. Today, it is abstracted by layering concepts, API-Led architectures, and other patterns. Tomorrow, it will become more fluid and increasingly mediated by intelligent systems, forcing us to rethink what it means to architect at all.


The redefinition of titles from “Architect” to roles like Principal or Staff Engineer does not diminish the essential need for architectural thinking — if anything, it intensifies it. In a technology landscape now defined by unprecedented intelligent abstraction and a constantly growing business appetite, organizations neglecting dedicated, strategic architectural oversight risk fragmentation, inefficiency, and chaos. Titles aside, it is the explicit investment of time, energy, and thought into a holistic, strategic system design that distinguishes enduring companies from those perpetually caught in cycles of technical debt and reactive firefighting.


The advent of AI, intelligent automation, and composable capabilities is not merely another technological shift — it signals a profound philosophical transition. Architects of today and tomorrow must navigate increasingly abstract environments, orchestrating systems composed of intelligent, autonomous capabilities rather than code alone. The challenge lies in understanding and guiding this new relationship between product requirements and technological potential, maintaining clarity amid the rising complexity and abstraction. Architecting systems that gracefully integrate intelligent agents and dynamic capabilities is no longer simply a technical skill — it’s a visionary practice, reflecting our understanding of how technology should seamlessly empower human intent.


This understanding shapes the way I approach building and leading architectural organizations today. I actively pursue flexibility, resisting rigid categorizations, because I believe future-ready architecture demands adaptability above all else. Architecting capabilities (vs. implementing a feature) is the guiding star that continuously motivates me to innovate not only within the products I deliver but also fundamentally in how I conceive and structure engineering itself.


Let this article serve not as a conclusion, but as an opening statement in an ongoing conversation. My hope is to ignite curiosity, inspire deeper inquiry, and encourage architects and technology leaders alike to rethink and refine the way we practice architecture. The road ahead is indeed uncertain, but filled with extraordinary potential for those willing to reimagine the role of architects as strategic visionaries, capable of thoughtfully guiding our collective journey into a future shaped by continuous change and boundless innovation.
