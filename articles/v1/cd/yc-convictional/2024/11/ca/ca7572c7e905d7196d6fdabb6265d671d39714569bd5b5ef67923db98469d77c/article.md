---
schema_version: "1.0.0"
document_id: "ca7572c7e905d7196d6fdabb6265d671d39714569bd5b5ef67923db98469d77c"
company_key: "yc-convictional"
company: "Convictional"
source_id: "yc-convictional-atom-2bd8494c6cc9"
canonical_url: "https://get.convictional.com/posts/context-is-a-fluid-permissions-in-an-ai-world/"
published_at: "2024-11-26T00:00:00+00:00"
first_seen_at: "2026-08-09T21:02:07.552290+00:00"
fetched_at: "2026-08-09T21:02:10.293363+00:00"
content_hash: "sha256:dad2d0fd0897a6fab4a91d3965b9ed5ee2f5147974197a3ab5384ccf92118f90"
---

# Context is a Fluid: Permissions in an AI World

#


The old rules of document security won't work in the age of business AI. When LLM tools like ChatGPT synthesize and share answers that span hundreds of underlying sources from the public internet, users benefit from its ability to make use of context in answering our question. But if a business focused chat product were to do that using your corporate documents, it would be viewed as an IT security disaster and shut down.


Unlike the public internet which is intended for consumption by anyone and everyone, most documents in the context of a business are intended for specific and limited audiences and use fine grained permissions to restrict access. LLMs strength in synthesizing information across many sources can become a problem when applied to work.


Unlike documents, which are like glaciers of knowledge, *context* is a liquid, with meaning emerging from the integration of and relationship between discrete pieces of knowledge. It flows between and among explicit knowledge like documents and databases, and tacit knowledge like meeting conversations and chat messages. *Context* , not documents, is the true input into core CEO tasks like goal setting and decision making. To be able to make use of context tools that bring together knowledge from all of these sources is both powerful and risky.


The liquid nature of context means that attempting to freeze knowledge into artifacts that can be permissioned individually will slow down your ability to apply the context to your work. At the same time, you have to make sure you're appropriately handling situations where sensitive access is happening, and those decisions about access should be made by people.


#### The Problem with Traditional Permissions


Permissions in the past are modelled after filing cabinets. You might get access to an entire cabinet, drawer, folder or file depending on the need. For sensitive data you'd likely be given fine grained permission by individual file, whereas for less sensitive information you might need to get in through course grained permissions like a cabinet or even an entire room. This worked because the magnitude of context being stored in physical files was limited by people's ability to organize it.


The challenge with files and your ability to discover what you don't know continued as information became digital, and accelerated as the amount of information that companies produce started to exceed our ability to file it. Most companies now access their knowledge through search tools rather than filing tools, and that is continuing with LLMs.


Information also has network effects which is why companies end up in a tug of war between sharing things widely and holding them close. Companies rarely have time to consider the direct effects of sharing new information. And some initiatives like M&A mean that companies operate according to multiple truth frameworks even within the leadership team. This pattern of tension between what is optimal for performance and what is optimal for security is constantly acting on work.


#### Understanding Context's Fluidity


Knowledge in companies is organized according to permissions rather than pattern of use. There's a lot of innovation going on right now around patterns of use for context that result from LLMs. Because LLMs are capable of understanding and 'working with' unstructured and structured context, the way that context is retrieved and combined prior to being used in producing a result for a user is where active research and development is ongoing. When we return results at Convictional, we are combining context about the industry, company, user, documents, meetings and data. The results get meaningfully better when you can make use of and combine far reaching context, rather than being limited to individual documents.


#### Example Problem #1: Cross functional planning


Say you're working on a planning project with a group that spans multiple functions. You might not know what they have access to, and they might not know what you have access to, while a shared context system can know both of those things. By having the human users collaborate with each other through the shared context workspace, it becomes possible to cover much more ground, but it also creates issues with permissions on the underlying documents. One approach is that you could have the LLM summarize the documents but not grant underlying access, but in 'all or nothing' permission schemes that's the worst case scenario, because then you don't have auditable knowledge of who accessed which documents.


Interpreting multiple sources of context and synthesizing it historically is the job of people who could lay out the related artifacts and develop their understanding in their head before writing it down again. In the future, machines will be providing much of that synthesis work. LLMs will be able to be deployed against the problem, where they are capable of processing thousands of pages of content in seconds. People are limited by their ability to consume information and make resulting decisions.


#### Example Problem #2: M&A decisions


Another illustrative example is M&A decisions on both sides of the transaction, where discussions are limited to the top 2-3 people in each company. Which means some executives are operating according to 'Truth framework A' where there's an active M&A discussion going on, and other executives are operating according to 'Truth framework B' where they are unaware. So while nominally the goals for the group are the same - hit profit targets - the actual goals drift even among peers. Great care is taken that discussions are limited to the group who has been read into the discussion and not others.


This can limit the effectiveness and alignment of the group. In the future, we will be able to use LLM enabled tools to answer almost any diligence question instantly, but the trade off we're making is that the LLM needs to know who is and is not read into the M&A discussion, and then be able to provide the user with meaningful risk analysis when sharing. The effect of LLMs on deal diligence will be profound, but it will require a corresponding shift from deterministic file-level permissions to something more akin to security clearances.


#### The Permissions Paradox: Security vs. Intelligence


The tension between what is best for the task at hand, and what is best for ensuring security of sensitive information, are often directly in conflict with one another as a result of LLMs. The cost of being overly restrictive with permissions lowers the return of investment of knowledge work in your company, and to not be able to benefit from bringing AI capabilities to bear on your goals. Being able to deploy AI at knowledge work gives human workers leverage to better serve customers, faster.


We believe that enterprise software is going to need to evolve to support *context-aware permissions* . The permissions scheme will be non deterministic, meaning that we will move away from deterministic access control and towards a risk-based model similar to the way the government does security clearances. LLMs themselves will be used to keep track of who is able to access which information at a topical level. When a user requests access to something, the LLM will perform a risk analysis and request the appropriate human user review access. In some cases where sensitive data is involved, this might mean dual approval by the CEO and the CIO. New permissions actions become possible as well, such as 'Temporary access by topic'.


Rather than completely hand over responsibility for permissions and walking the line between security and intelligence to machines, we believe that there will be a significant element of humans in the loop for verification steps. For example, managers could sign off on granting access to a user on their team for a specific new and sensitive topic. Over time this will lead to more organizational confidence and trust than we currently have and higher functioning knowledge work.


This shift means going from binary to probabilistic access, where a probability that access is appropriate is assigned. Ultimately, companies will be able to see even more rich audit logging, such as who has access to which knowledge by topic and not just by individual document. This likely means that all knowledge accessed at work can be recalled during audits. We are going to have to think about the privacy implications to systems that are completely aware of who has accessed which context.


#### The Future of Intelligent Permissions


It's going to take time for systems to evolve from binary to probabilistic permissions schemes. Once we do get there, many new forms of intelligent permissions and security methods become possible. We believe that security analysis of who has access to which context will be performed continuously by AI agents. A form of moderation agent could be added at the edge (e.g. on people's devices) and used to analyze *all* context that they are accessing in the course of their work. This could flag real time alerts to IT about access that may have been unintended, such as in emails, chat messages and on video calls.


These AI agents will also be able to learn across companies and prevent many types of insider threats that are commonly affecting companies today. Because AI is being used to perform true-to-life phishing attacks and other forms of malicious action, companies will inevitably be investing in AI tools for protecting data both in centralized storage and on the edge on user's devices. With powerful tools for search and synthesis across company data, comes responsibility for ensuring safe use.


#### Next Steps for Organizations


Many organizations are still struggling through the first AI chatbot deployment and learning from their mistakes as they go. We believe it's better to experiment and learn than it is to avoid experimentation and end up falling behind. AI is capable of making people materially more effective in their roles which accelerates every aim and intention companies have. Any time you accelerate something, you end up also increasing the number and magnitude of problems. It might feel like there are constant security issues and be tempting to slow it down, but that is directly born of the acceleration of the underlying work.


The main thing to do is audit how your team is using these AI tools today and identify opportunities to raise people's understanding of security. Start to experiment with giving access to tools that can synthesize data across multiple sources, and start with data sources that are publicly available like team handbooks and non sensitive customer contexts. As time goes on and the organizational readiness increases you can start to implement it in more sensitive areas like legal, finance, HR and IT.


At Convictional, we believe context aware permissions are inevitable and are designing our software in the way that we believe balances people's readiness for non deterministic permissions with powerful capabilities. If you're thinking about this kind of hurdle to implementing AI within your organization, reach out and we can share more of our learning.


#### Key Takeaways


1. Context is inherently fluid and crosses traditional document boundaries
2. AI amplifies the need for more sophisticated permission models
3. The future belongs to organizations that can balance security with contextual intelligence
4. Traditional permission models are increasingly becoming a liability
5. The transition to intelligent permissions is both necessary and inevitable


Interested in trying Convictional? Email us atdecide@convictional.com .
