---
schema_version: "1.0.0"
document_id: "476d705ecb051edd10f7b2ad7bd357d24c2cbdc6bfb185a9ba5cd08f9e0f0386"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/devex-past-present-future"
published_at: "2024-11-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:b78a62e56f8e86d8d256e29aa418957b33c799305a8e7f6b66e20e57b75ee112"
---

# Developer Experience: Past, Present & Future

If it feels like everyone is talking about Developer Experience these days, that’s because *everyone is talking about Developer Experience these days* 😆


Engineering orgs are standing up teams focused on DevEx. Products are trying to bottle it up and sell it. Conferences are hosting sessions on it. Scientists are studying it. Developer tools are crafting their value propositions around it. Career paths are coalescing around it.


Is all this attention to DevEx warranted?


Abso-fuckin-lutely. With countless industries investing heavily in software - and nearly every company now employing software developers - it’s essential to focus on the experience of those driving that development forward.


But listen: The world doesn’t need another “What is DevEx and why is it important?” blog post. These days, every big developer tool platform seems to have an article with that approximate title. So, we thought we’d go a little deeper and instead write about the origins of DevEx, the current state of affairs, and some thoughts on where DevEx might (and should) be headed.


## Where have we been? A brief history of DevEx


DevEx was conceptually defined in 2012 by scientists[Fabian Fagerholm](https://www.aalto.fi/en/people/fabian-fagerholm) and[Jürgen Münch](https://www.researchgate.net/profile/Juergen-Muench) . In their scholarly article[“Developer experience: Concept and definition"](https://arxiv.org/pdf/1312.1452) , they describe *developer experience* as reflecting “how developers think and feel about their activities within their working environments.”


Prior to this, mentions of DevEx are scant in popular and social media. In[this social media post from 2010](https://x.com/mahemoff/status/1947230411948032) , Developer Experience is mentioned explicitly, and a clear analogy is made between it and User Experience.


Not six months later, Jeremiah Lee Cohick wrote about DevEx in March of 2011 in the UX Magazine article[“Effective Developer Experience (DX)”](https://web.archive.org/web/20120331014917/https://uxmag.com/articles/effective-developer-experience) . Cohick wrote that when Apple released the iOS SDK, it moved the iPhone beyond being a mere product, making it a platform. He went on to argue:


> When a product transitions into being a platform, it takes on a new type of user: the third-party developer. When developers build their own products on a platform, they are in effect users of that platform. But they are a special type of user, one that behaves as an intermediary between end users and the platform product.


> An end user’s experience with a platform product, such as the iPhone, includes the experience of using third-party apps. Every app is a use case that should reflect the user experience of the principal product. Platform product owners must be concerned with assisting developers in accomplishing this if end users are to have a good user experience overall. Attention to these details is called developer experience (DX), and enabling app developers to be successful through better DX will create a more successful UX for the platform product.


Outside of these couple mentions, though, search engines don’t point to much industry talk around DevEx prior to the publication of Fagerholm and Münch’s concept-defining piece.


Over the next several years, however, we do start to see “developer experience” pop up in the titles of scientific articles and conference proceedings. Some of these defined and examined DevEx as an analogy to User Experience, considering the experience of developing with certain tools or within particular development contexts:


- [“Design framework enhancing developer experience in collaborative coding environment”](https://dl.acm.org/doi/abs/10.1145/2695664.2695746)
- [“Are software developers just users of development tools? Assessing developer experience of a graphical user interface designer”](https://inria.hal.science/hal-01647699/document)
- [“Investigating factors that influence developers' experience in mobile software ecosystems”](https://www.researchgate.net/profile/Awdren-Fontao/publication/314885924_Investigating_Factors_That_Influence_Developers%27_Experience_in_Mobile_Software_Ecosystems/links/5b02e4354585154aeb06fdbf/Investigating-Factors-That-Influence-Developers-Experience-in-Mobile-Software-Ecosystems.pdf)


Others examined the human factors of DevEx:


- [“Flow, intrinsic motivation, and developer experience in software engineering”](https://link.springer.com/chapter/10.1007/978-3-319-33515-5_9)
- [“Consequences of unhappiness while developing software”](https://ieeexplore.ieee.org/abstract/document/7961892)
- [“Unhappy Developers: Bad for Themselves, Bad for Process, and Bad for Software Product”](https://ieeexplore.ieee.org/document/7965359)


A strong sign that a concept is gaining traction is when researchers begin conducting literature reviews around it. Just this past October (2024), Razzaq et al. published[a literature review focused on DevEx research](https://dl.acm.org/doi/pdf/10.1145/3687299) . Their review covered research from January 2005 to July 2020, aiming to pinpoint practices and methods that impact developer experience and productivity—both positively and negatively. Remarkably, they identified 218 relevant papers focused solely on the productivity aspect of DevEx.


With these papers, we hadn’t yet seen the DevEx explosion, though. The DevEx concept seems to have really taken hold at the industry level in 2022 when – ten years after Fagerholm and Münch’s seminal 2012 paper –[Michaela Greiler](https://scholar.google.com/citations?user=zHYV9RMAAAAJ&hl=en&oi=sra) and[Margaret-Anne Storey](https://scholar.google.com/citations?user=WhjQewkAAAAJ&hl=en&oi=sra) collaborated with entrepreneur Abi Noda to conduct qualitative, exploratory research on DevEx (Noda launched[a product called “DX”](https://getdx.com/about/) in January of 2021). They drew from Fagerholm and Münch’s 2012 work to define DevEx as:


> a broader concept that captures how developers feel about, think about and value their work.


They noted that while Fagerholm and Münch provided “an initial framework for thinking about the different dimensions of developer experience”, it hadn’t explored which of those dimensions impacted DevEx, why some seem to be particularly important, or how teams could overcome obstacles to good DevEx. Their qualitative work aimed to fill that gap, and it led to the development of the DX Framework, which they labeled “an actionable conceptual framework for understanding and improving developer experience” and a “go-to reference for organizations that want to enable more productive and effective work environments for their developers.”


## Where are we now? Contemporary conversation around DevEx


Since the introduction of the DX Framework, there has been a proliferation of both industry and academic activity around the concept.


### DevEx Science


There are now multiple labs that are either explicitly branded as DevEx research labs – those like[Nicole Forsgren](https://scholar.google.com/citations?user=vis0ZxUAAAAJ&hl=en&oi=ao) ’s[Developer Experience Lab](https://www.microsoft.com/en-us/research/group/developer-experience-lab/people/) at Microsoft – or that do research on DevEx topics. For example, Margaret-Anne Storey leads[CHISEL](https://thechiselgroup.org/) at the University of Victoria - Canada, and[Cat Hicks](https://scholar.google.com/citations?hl=en&user=MotKyQcAAAAJ) heads up the Pluralsight[Developer Success Lab](https://www.pluralsight.com/product/flow/developer-success-lab) .


### The Marketization of DevEx


In industry, software engineers, product owners, and startup founders have identified opportunities and business value in tackling DevEx. We’ve seen a rise in DevEx platforms like[Spotify’s Backstage](https://backstage.spotify.com/products/portal/) or[Atlassian Compass](https://www.atlassian.com/software/compass) . These platforms aim to bring all the tools developers need to build, run, and ship software together in one place, with the overall goal of increasing DevEx through standardization and the removal of infrastructure and tooling overhead. In other words, by removing friction.


We’re also increasingly seeing what I’ll call *DevEx as a Product* , with the creation of products devoted to helping engineering teams ascertain, evaluate and improve the DevEx within their teams and organizations.[DX](https://getdx.com/) is the original and predates contemporary academic research, but multiple software engineering metrics platforms have built out DevEx products within their existing suite of products ([Jellyfish](https://jellyfish.co/platform/devex/) ,[Flow](https://www.pluralsight.com/resources/blog/business-and-leadership/developer-experience-metrics) , and[Swarmia](https://www.swarmia.com/product/developer-experience/) , to name just a few). These tools use surveys to ask the software developers themselves about the quality of their experience developing software at that company or on that team.


In addition to companies selling DevEx as a Product, many existing big developer tool companies are publishing content on DevEx, providing their own definitions, and pitching parts of their product offerings as DevEx enhancers. For example:


Company Says that DevEx... DevEx Dimensions


[GitHub](https://github.blog/enterprise-software/collaboration/developer-experience-what-is-it-and-why-should-you-care/) “...examines how the juxtaposition of developers, processes, and tools positively or negatively affects software development.” people, processes, tools


[GitLab](https://about.gitlab.com/topics/devops/what-is-developer-experience/) “...focuses on streamlining processes and developer tools and making it easier for developers to do their jobs.” processes, tools


[Atlassian](https://www.atlassian.com/developer-experience) “...focuses on developers' lived experiences: the points of friction they encounter in their everyday work and how they connect emotionally with their jobs.” lived experience, friction, emotion


### The Viralization of DevEx


When a new corporate concept gains traction and we see a surge of media around it – like podcasts, webinars, articles, blog posts, and conferences – this means there’s a broad interest, curiosity, or at least a perceived need to understand the concept, and this is typically born from a collective, baseline validation of the concept. The concept's popularity suggests a commercial opportunity, prompting creators, consultants, and companies to develop resources, tools, and training around it.


And we’re certainly seeing this with DevEx at the moment. If we look solely at conferences, just this year, we saw Harness host a[Developer Experience Summit](https://www.harness.io/event/developer-experience-summit) . Sentry hosted a[Developer Experience event](https://sentry.io/events/dex/) . GitButler launched[The Merge](https://blog.gitbutler.com/birth-of-the-merge/) , a conference devoted to Developer Experience. And Gradle hosted the[DPE Summit](https://dpe.org/summit2024/) , which their marketing materials claim is “The only event dedicated to Developer Productivity Engineering (DPE) and Developer Experience.”


## Where are we going? Future Directions for DevEx


Looking ahead, we think the future of DevEx will be shaped by emerging technologies, evolving team needs, and the growing importance of seamless workflows, all increasingly guided by empirical research that illuminates what truly empowers developers to do their best work.


Specifically, we hope to see more of the following.


### Group Difference Reporting in Empirical Research


There are a handful of influential DevEx research papers that either don’t report on participant demographics, or that include samples that are much too homogenous to be able to confidently propose frameworks applicable to all software developers. Consider the unique demographic and firmographic profiles of the following developers:


Gender Age Race Software Education Years Coding Role Company Size Industry


Male 45 White Coding Bootcamp 0 Junior Software Developer 8 (startup) MarTech


Female 20 Hispanic Self-taught 12 Software Engineer 2,000 Developer Tooling


Non-binary 36 Black M.A. in Computer Science 18 Principal Machine Learning Engineer 5,000 Language Learning (NLP)


Each of these will likely face different challenges through that unique combination of their demographic and firmographic details. We need more research that explores and reports on these individual differences so that we can understand the experience(s) of developing software for all software engineers, regardless of gender, age, race, education, etc. Only through this expanded understanding can we begin to think about targeted interventions for improving DevEx.


### DevEx Professionals Translating Empirical Research


Finding, reading, thinking about how to apply, and then actually applying findings from empirical research is time-consuming and cognitively demanding. That, and it requires a certain level of research literacy that understandably, many software engineers don’t have. Despite the barrier to entry, I wish I saw more software folks – from the IC-level to high-level leaders – engaging with empirical research. Perhaps what’s needed here are intermediary folks with backgrounds in academia, software industry, and pedagogy (the methods and practice of teaching) . These are unicorn people, of course, but they’re out there. If more research labs employed folks with this skillset – or, if Developer Experience teams included a role requiring these skills – they could translate scholarly research into practical applications, and then help make sure those practical applications (i.e., interventions) are fed back into research for the iterative construction of knowledge and evidence-based practices.


### Developer Tools That Make DevEx Insights Accessible


When a company claims their tool will enhance DevEx, that tool should come with built-in, empirically-grounded features that allow customers to clearly assess its impact. Customers shouldn’t have to undertake the heavy lift of designing and implementing pre- and post-measurements themselves. Instead, tools should offer reliable, valid insights rooted in the latest DevEx research and developed in collaboration with experts in research methodology and measurement design.


## Wrapping Up


DevEx has come a long way since Fagerholm and Münch formally defined the concept back in 2012. At Depot, we strive to make the experience of developing software great for those who do it, and we hope we continue to see this area grow.


Kristen Foster-Marks


Head of Developer Experience at Depot
