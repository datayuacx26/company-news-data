---
schema_version: "1.0.0"
document_id: "9ca1ee40b2bd10d34aae4ce03cd88bc5c589fceb7e48df30ca612e3078e87c63"
company_key: "yc-let-s-do-this"
company: "Let's Do This"
source_id: "yc-let-s-do-this-news-import-089850d1dce8"
canonical_url: "https://www.letsdothis.com/blog/the-story-of-nova"
published_at: null
first_seen_at: "2026-07-27T03:27:21.649983+00:00"
fetched_at: "2026-07-28T21:16:46.713740+00:00"
content_hash: "sha256:dcaf2c602df3677744d3d4d6d289bdafd07ad47c1bbc3d57c9ae10ce4a4d4fca"
---

# From hackathon to industry leading product - This is the story of Nova

What started as a scrappy hackathon project to explore how AI might help event organizers access data insights faster, has grown into one of the most ambitious product developments we’ve ever worked on. That early spark has evolved into Nova, an AI agent that is transforming how organizers access, understand, and act on their data.


---


### **It began with a healthy dose of Northern Irish sarcasm**


We are proud of the lengths we go to support our organizers - and that has always included our talented team of data scientists helping them get insights from their data. As we’ve grown, it’s been tough to keep up with the volume and variety of questions thrown our way. Often our partners needed insights quickly, but building reports manually could take hours, if not days.


Everyone felt this friction, and our hackathon was the perfect time to step back and reimagine how we could address this challenge.


One of our teams asked a simple but important question:


**What if event organizers could get answers to their data questions instantly - no specialist skillsets or data team requests, no endless spreadsheets, no waiting days for reports?**


Fresh from a Snowflake conference where natural-language analytics was a major theme, the team built an early prototype, a basic tool capable of transforming a plain-English question into a SQL query and executing it directly against our database, housed within a simple Slackbot.


It was called “The Ghost of Simon” - a tongue-in-cheek nod to a data team alumni, trained not just to write flawless SQL - but to respond to you with Simon’s trademark, bone dry Northern Irish sarcasm. This particular trait hasn’t been carried forward to Nova…not yet, at least.


The Ghost of Simon - The AI Slackbot created during the hackathon with Lucy, Data Scientist on the Nova team


The output wasn’t always perfect, and the tool wasn’t polished, but the opportunities were clear:


**AI could bridge the gap between the way organizers naturally ask questions and the technical structure of our data.**


The team won the hackathon, and more importantly, uncovered a direction that would reshape our product strategy.


Lucy, one of our data scientists on the project, recalls the moment it clicked when she had been requested a complex data report from one of our long-standing US partners, Atlanta Track Club:


> “Normally I’d spend several days building a detailed report, but this time I ran it through our prototype, validated it, and had a full analysis within a matter of hours. That’s when we realized we were really onto something.”


– Lucy Miller, Data Scientist


### **Engineering Innovation from the Ground Up**


As we moved from prototype to product, the team quickly realized there was no off-the-shelf solution that could support what we needed. Existing tools on the market weren’t designed for the complexity of event data, so our engineering team built the foundations themselves.


They created an internal service to handle documentation, query execution, and structured data context, enabling Nova to reliably translate natural-language questions into accurate database interactions.


> “Building it ourselves gave us freedom to experiment and move fast. It also forced us to deeply understand how organizers think about their data.” *‍*


– Matteo Hertel, Engineer


Matteo - Engineer, member of the Nova team


Nova evolved into a multi-model system, combining GPT-5 for natural language, Anthropic’s Sonnet for deeper data reasoning, and Haiku for rapid responses.


> “The space moves incredibly quickly. One week a model would be too slow, the next week an update would drop and everything changed overnight. Nova has been shaped by continuous iteration."


– Chris Saunders, Engineer


This foundation enabled the team to think beyond answering questions, and toward guiding organizers more proactively.


### **Design Patterns to Give Organizers Confidence**


As development progressed, it became clear that many organizers didn’t know which questions would give them the insight they actually needed. This shaped a major part of Nova’s evolution, bringing together engineering, data, and design to help organizers understand what to ask in the first place.


The first development was an insights agent, a proactive system that analyzes an event’s performance daily and surfaces meaningful insights and opportunities. Crucially, the agent had to offer relevant context. A small regional marathon should not be benchmarked against a World Major Marathon. Nova needed to understand event type, scale, timing, and behaviour to generate fair comparisons and helpful recommendations.


Nova in action - Event Organizers get proactive insights, analysis and recommendations to help grow their events


At the same time, Stephanie, Product Designer, focused on the experience of working with Nova. She explored dozens of AI interaction patterns, from Claude and ChatGPT to home-built prototypes, to determine what kind of conversational interface made sense for an event organizer’s workflow.


> “One of the biggest challenges was confidence. We needed to help organizers understand what Nova could do without overwhelming them.”


– Stephanie Donald, Product Designer


This led to features such as rotating suggested prompts, contextual nudges, and a carefully integrated sidebar that keeps Nova aligned with the organizer’s workflow. Combined with the insights agent’s contextual logic, these design choices help organizers get started, explore ideas, and build confidence by showing them exactly how Nova can help.


### **Refining Nova’s Tone of Voice**


Early outputs were often accurate but direct, and at times, blunt.


> “Some of the first outputs were unintentionally (and at some points hilariously) harsh. They were factually correct but not particularly constructive.”


– Mark Handley, Product Manager


Refining Nova’s tone became a key consideration. The team shaped its voice around clarity without alarmism, and opportunities rather than criticism. The goal was not just to deliver data, but to support organizers with thoughtful, actionable guidance so they could learn as well as optimize.


### **AI That Learns and Improves Over Time**


As Nova matured, the team built mechanisms to help it learn continuously. Using Claude and internal tooling, the team reviewed Nova’s queries and responses to identify patterns, gaps, and opportunities to improve.


> “We’re using AI to improve AI. Every week, Nova becomes more accurate and more intuitive because it’s learning from real organizer behaviour.”


– Lucy Miller, Data Scientist


This feedback loop has become central to how the team operates.


From planning to problem solving, building Nova has been a true team effort


### ‍ **What Nova Helps Organizers Do Today**


Nova is supporting beta users with tasks that once required deep expertise, and hours of manual work.


From daily registration summaries, to performance comparisons against relevant past events, to drop-off and funnel analysis. It can flag emerging risks and opportunities, offer early marketing insights, and answer ad-hoc analytical questions at speed.


Feedback from the beta group has been really encouraging and is helping shape our roadmap.


> “Nova is surprisingly good at understanding where I'm trying to get to when I ask for its analysis of our data. It provides me with easily consumed analysis and offers suggested marketing action-items based on its analysis.”


– Rich Kenah, CEO @ Atlanta Track Club


By removing technical barriers, Nova enables organizers to work more strategically, focusing on action, not analysis.


Nova in action - Helping Event Organizers get to the insights that matter


### **Constantly evolving how we work**


One of the most important outcomes of building Nova wasn’t the tool itself, but how it reshaped collaboration across the team.


Traditional boundaries between roles began to blur. Designers contributed to prompt logic, engineers influenced UX decisions, product managers immersed themselves in data modelling, and data scientists shaped interface thinking.


> “AI pushed us to rethink how we work. It encouraged experimentation, faster iteration, and a more fluid approach to problem-solving.”


– Stephanie Donald, Product Designer


Arguably the key learning was something Dave, Data Engineer, shared on the difference between getting something working and getting it right.


> “We were amazed by its early responses, but quickly saw its limits with ambiguous questions and its inability to admit when something was out of scope. It showed how easy it is to build something that kind-of works, and how much additional effort it takes to make something truly valuable.”


– Dave Mills, Data Engineer


Ultimately, Nova is not only a product milestone, it’s been a catalyst for team growth, introducing new ways of thinking and building together.


### ‍ **Beta and Beyond**


Nova is currently in beta with partners who are helping shape its next phase of development.


In the near term, we are focused on deepening Nova’s marketing intelligence. Organizers want clearer visibility into which channels truly drive results, so we are building richer functionality to give organizers a far clearer understanding of what is working and where the greatest opportunities lie.


Looking further ahead, we are investing in a stronger data foundation that allows Nova to scale with sophistication. This includes more advanced semantic modelling, improved contextual retrieval, and more meaningful benchmarking across the industry. Collaboration is also an important part of Nova’s future, enabling teams to share insights and make decisions together.


Nova is still early in its journey, but the direction is clear - we are building an AI agent that is improving the way event organizers grow their businesses, with more clarity and actionable intelligence at their fingertips.


**Want to see Nova in action for yourself?**[Join the waitlist](https://www.letsdothis.com/waitlist)
