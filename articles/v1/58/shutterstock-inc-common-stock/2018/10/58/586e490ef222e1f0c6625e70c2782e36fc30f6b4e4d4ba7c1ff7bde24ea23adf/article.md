---
schema_version: "1.0.0"
document_id: "586e490ef222e1f0c6625e70c2782e36fc30f6b4e4d4ba7c1ff7bde24ea23adf"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2018/10/16/engineering-career-tracks-at-shutterstock"
published_at: "2018-10-16T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:39e706d5e25773bfccda3c030bb5f8aac938c9cb4a15ec11660d78d7b739e4bc"
---

# Engineering Career Tracks at Shutterstock

Clear definition of levels and active enablement of individual career development are cornerstones of healthy, high-performing, highly-engaged engineering organizations. At any given point in time, it is important for engineers and managers of engineers to comprehend where they stand, the possible paths before them, and the specific behaviors and impact expected of them at each level from both performance and career progression perspectives.


Although tech companies generally share many traits, what expectations are deemed important at a given company are highly dependent on their own unique engineering cultures. As such, there are no drop-in, one-size-fits-all career leveling/development frameworks that can be leveraged between companies without (potentially significant) loss of fidelity to the characteristics and culture that companies choose to define themselves.


As such, this post describes how we’ve implemented a structured framework that is aligned to the unique traits and culture of Shutterstock but does not claim to define a generalized solution for all engineering organizations.


## Prior (Ground)work


One of the most notable, relatively recent enlightenments to engineering tracks is the bifurcation of management and individual contributor careers within software engineering as parallel, and not sequential, tracks. Before this shift, when an engineer reached a certain level of seniority and was ready for promotion, upward mobility usually necessitated a move into management, or to otherwise see their careers stall. Given that many engineers are not interested in or suited for the far different demands of people management (vs. individual contributor (“IC”)), this change was a momentous (though with the benefit of hindsight, painfully obvious) advance. Fast forward to present day—though everyone begins their careers as individual contributors, after a certain level, a fork in the path appears where an engineer, should they be suited for the role and identified as such by leadership, may embark upon the management track or remain in the IC path which continues before them.


Today, most large tech companies now define IC and management (M) tracks as parallel.


### Individual Contributor (IC) Track


When we look at the traditional IC track, progress is generally linear along a single dimension. For example, in an organization with 7 career steps for engineers (e.g. L1…L7), an individual will progress incrementally from L1 through L5 over the course of their career at the company, with L7 being the highest level (both L6 and L7 are reserved for “exceptionally exceptional” individuals and are not considered part of “normal” career progression for the vast majority of ICs). In many places, as one progresses in their career, this not only means that they’re getting progressively better at the specific competencies and expectations of them as a software engineer, but also means, especially after the Senior Engineer level (usually L3), that they may need to acquire/demonstrate skills that may not be in their interests or wheelhouse.


For example, in many traditional career tracks, expectations for L4+ (i.e. Staff Engineer+) come with the implication of broader focus on more cross-cutting concerns across the engineering organization, as opposed to diving deeper into the product engineering stack for their teams; this almost always means less coding coupled with i) taking on broader, organizational initiatives (e.g. architecture reviews, process design, general “thought leadership”), ii) mentoring, iii) cross-team collaboration/communication, iv) public speaking and, again (and importantly), v) **less coding** .


Not everyone is suited for or interested in leading organizational initiatives and, as anachronistic as it was in forcing engineers to become managers in the past, it is equally disjointed to take the engineers who are best at what they do (i.e. coding) to make them do less of that, just because of arbitrary career definitions which fail to account for more nuanced differentiation in interests between individuals. At smaller companies (i.e. startups), you typically have no choice but to be a generalist and work on everything that comes your way, but at larger companies, specialization becomes an imperative to achieve broad technical efficiencies and, as far as Shutterstock goes, existing career models did not satisfy the specific needs of the organization.


### Management (M) Track


The entrypoint to the engineering management track typically comes after the Senior Engineer level (i.e. L3), as a promotion up and out of the IC track into leadership, which implies that an entry-level manager is on par with L4 engineers in terms of experience/leveling. The nature and roles of Engineering Managers vary widely across the industry, however, the fundamental, defining characteristic of the role is that of being responsible for managing people. The extent of their day-to-day technical involvement is dependent on the specific needs of companies, however, the explicit mandate of leadership positions generally broadens further along the management track. As with the IC track, we believe there’s a clear difference between types of technical leaders that we’ll get into below.


## Building for Purpose: Overall Structure


As referenced in Jon’s[introductory post](https://tech.shutterstock.com/2018/10/10/tech-blog-relaunch) , Shutterstock’s priorities in the early stages were about building product and moving quickly to support the company’s growth, resulting in tacit, undocumented understandings of career development. Like virtually all startups, thinking about how to structure the organization to support career development for what started off as a handful of engineers isn’t a priority until that cohort has “suddenly” grown to several hundred in number. With the benefit of hindsight, it becomes clear that a lack of clearly-defined titles, levels and expectations is detrimental to long-term retention of talent and therefore poses a challenge to scale the organization past a certain point. These are the non-trivial challenges which this framework is intended to counteract.


### What We’ve Done


Shutterstock launched a new career track framework across the Engineering organization in early February 2018. In our case, we have a need for both deep, product-level and broad, cross-team/organizational specialists. In order to support growth in what we see as clearly differentiated specialties, we created two distinct and parallel paths within the IC and management tracks, corresponding with focus at the team or organizational levels. This is best illustrated with a diagram:


### IC Track


After IC3 (Senior SE), the career track splits into two lanes, corresponding to team or organizational scope. We define Staff Engineers to be deep product-level experts who are tied to specific, product teams and who report into the (Sr.) SDMs for those areas. On the other hand, we define Principal Engineers to have broader, organizational focus on cross-cutting initiatives that generally impact more than one team if not the entire organization. Principal’s, therefore, report into organizational-level leadership at Shutterstock (i.e. Directors and above).


This does not mean that an IC in either lane is 100% focused in one area at the expense of the other; it’s more about the proportional allocation of focus (e.g. 70% organizational/30% team for Principal SEs and the opposite for Staff SEs) which may shift over time. Being a Staff Engineer at Shutterstock, however, doesn’t restrict their involvement in broader initiatives outside their immediate teams. By the same token, a Principal Engineer is not myopically focused on organizational initiatives alone; they still contribute to discrete team initiatives, but that is not the overarching focus of their attention.


This structure does not preclude the ability to switch between Staff and Principal paths once an IC has chosen a lane.


### M Track


We’ve adopted a similar bifurcation with the leadership track, where we’ve identified team- and organizational-scopes for career development therein. Entry-level engineering managers (M1—a level which is equivalent in overall experience as IC4) and more experienced engineering managers (M2) are classified as team-scope leaders. We expect our leaders to have attained a senior level of technical expertise before taking on the challenges of leading engineers.


Though we expect our Engineering Managers to be technical and available as technical advisors/coaches, we don’t generally expect them to write code or make day-to-day technical decisions for their teams; their responsibilities are primarily concerned with maintaining, cultivating and growing healthy engineering teams along the dimensions of individual skills growth, project execution, job satisfaction, and career development. At the M3 (Director) level and above, the mandate of leadership responsibilities shifts towards a more organizational scope (i.e. across more than one team) and typically involves managing managers. This lane goes to M5 (VP).


### Breakpoints


We’ve also introduced the concept of “breakpoints” in our career framework (this is not unique to Shutterstock). These are levels at, and after, which a given role is terminal. In other words, once reaching a breakpoint, an individual can conceivably remain there for the remainder of their career. However, up to a breakpoint, all individuals are expected to continue growing professionally through their respective tracks. This concept supports individuals that, for any number of reasons (e.g. family obligations), are not willing/able to make the contributions necessary to advance to the levels above a given breakpoint, yet are fully-productive and valued employees.


## Putting Meat on the Bones


Visual diagrams of frameworks are nice and tidy, but without well-defined supporting documentation and processes, they are toothless and wholly ineffective. How we define the behavioral characteristics and expectations of personal impact at every level along both tracks is largely informed by what we value as an engineering organization, but, more importantly, is informed by the clear, aspirational goals we want to set for our employees. In other words, *“What sort of organization are we now and what sort of organization do we aspire to be?”* This is the core essence which we codify in a career track framework.


### Baselines


Most experienced engineers and engineering leaders have strong, internalized views on what it means to be an IC1 vs an IC3 for example. Though these views may intersect to some degree across individuals, they are personal opinions and not universal, empirical truths; they are based on relative, not absolute, measures that may or may not be objectively important from a given organization’s perspective: relative experience, relative skill, relative seniority, relative communication skills, etc. Since an individual’s perceived value of a given measure is pegged to internalized benchmarks derived from their own unique experiences, it is important to define externalized anchors that create objective alignment and shared understanding across individuals, emphasizing the specific behaviors and impact that are important to succeed within the context of the organization.


We use the concepts of “behaviors” and “impact” to define leveling which allows us to scale the framework across the diverse engineering organization (product engineering, services, data science, devops, infra, etc.). These are “stable” characteristics which we believe measure what’s important for individuals within the organization. We deliberately avoid articulating *specific* technologies as requirements for any level. Instead, technical skills, if applicable for a given individual, are framed as behavioral expressions or magnitudes of impact. These are detailed for every level and each track. Though we don’t drill into the specific behaviors and impact for levels and tracks in this post, we may go into some level of detail in a future post.


### Supporting Processes


Like many large companies, at the company level, we set and measure against individual goals/objectives annually. However, career development and human processes are too fluid to hew to such an infrequent cadence of reviews. As such, in parallel to annual objectives, each manager works with their direct reports to create career development plans that are reviewed throughout the year to track progress and (re)set expectations as needed, understand challenges, clarify progression, etc. The career development plan sits apart from and does not directly impact the annual review; it may help inform and measure achievement of annual objectives, but there is a loose coupling between these disparate, but related, processes. Moreover, the contents of such career development plans are confidential between each manager and their direct reports.


## Conclusion


The key objectives of why we implemented a comprehensive career track framework are to:


- create a shared, objective understanding of what we expect from each level
- provide a framework within which an individual can understand their own performance
- set realistic expectations on career development and timelines
- calibrate levels and expectations across the organization and across the industry at large


We view what we’ve created thus far the start and not the end. The organization will change, as will the people, and the framework must adapt to new realities and changing expectations; like organizations themselves, these frameworks must evolve as necessary to remain relevant over time.


\[Originally published on[jasonoh.org](https://jasonoh.org/2018/08/31/Designing-Engineering-Culture-Shutterstock.html) \]
