---
schema_version: "1.0.0"
document_id: "e0f1043eca4c0f92cd20568508fb5ad6835384e4b517324239d29f98242823cb"
company_key: "yc-convictional"
company: "Convictional"
source_id: "yc-convictional-atom-2bd8494c6cc9"
canonical_url: "https://get.convictional.com/posts/from-transcripts-to-decisions-a-research-project-in-applied-ai/"
published_at: "2025-07-01T00:00:00+00:00"
first_seen_at: "2026-08-09T21:02:07.552290+00:00"
fetched_at: "2026-08-09T21:02:10.293363+00:00"
content_hash: "sha256:ffd8b4278304c75addb5e27545f19eabfa2b5d5b30302ccac6205c52aa394cc8"
---

# From Transcripts to Decisions: A Research Project in Applied AI

#


What if we could automatically extract not just *what* was decided in meetings, but *how* decisions were reached? Our AI meeting bot already captures meetings as transcripts. Building on that foundation, this post explores a technical applied AI research project at Convictional focusing on extracting structured decisions from meeting transcripts.


To illustrate the problem: With our product launch three weeks away, our engineering team debated in a meeting whether to integrate with Google's email API or build our own email system. We chose Google's proven reliability over building in-house, despite never having used their API before since it would get notifications working in days rather than weeks. Two weeks later though, nobody could remember the exact criteria we used or the specific insights that tipped the decision.


This plays out everywhere. Decisions get made, context disappears, and institutional knowledge walks out the door. Decisions can also happen without meeting participants even realizing they've reached a conclusion in the moment.


Our project to extract structured decisions from those transcripts became one of our first research initiatives to make it all the way to users. Through systematic experimentation and iterative problem-solving we challenged many of our initial assumptions and learned important lessons:


- Start with the simplest approach that could possibly work
- Mirror human cognitive processes by decomposing complex tasks
- Data representation can matter more than model sophistication
- Sometimes less context produces better results
- Test with data you understand deeply
- Applied AI research is about systems, not just models
- Perform research with production in mind from day one


These insights now help guide how we incorporate AI at Convictional to research messy, real-world problems.


## Defining the Problem Space


Before diving into the technical approach, let's be precise about what we mean by "decision extraction." Our model for a decision is informed by our own deep learning into decision science and ongoing collaboration with our academic advisors. In our model a complete decision comprises three main components:


- **Options:** The alternatives that were considered ("Remote work vs. hybrid vs. in-office policies", "OAuth 2.0 vs. custom JWT).
- **Criteria:** The factors used to evaluate options ("Employee satisfaction", "Office costs", "Security posture", "Implementation complexity", "Maintenance overhead").
- **Insights:** Key observations or learnings that could influence the decision ("Most employees prefer flexibility", "Our team lacks OAuth expertise").


This is fundamentally different from meeting summarization or simple Q&A systems. We need to understand implicit reasoning, extract structured data from unstructured conversation, and maintain precise source attribution back to specific parts of the meeting transcript, all while handling the messy reality of how humans actually make decisions in meetings.


This is a hard problem: conversations meander, people change topics mid-sentence, important context gets shared in meeting chat sidebars, and the actual decision moment might be buried in a two-hour discussion about implementation details.


## The Simple Approach (And Why It Failed)


Our first attempt was straightforward and simple: give the LLM a complete meeting transcript and ask it to extract all decisions and their components in a single response. We used the instructor library to ensure structured output validation, defining[Pydantic](https://docs.pydantic.dev/latest/) models for our decision schema:


```text
class Decision(BaseModel):
title: str
description: str
options: List[Option]
criteria: List[Criterion]
insights: List[Insight]
source_lines: List[int]


```


```text
class Option(BaseModel):
title: str
description: str
is_selected: bool = False
source_lines: List[int]


```


The single-shot approach seemed elegant, with one API call, one structured response, and minimal complexity. We tested it on transcripts ranging from 15k to 40k tokens, both chunked and non-chunked.


The results were consistently disappointing:


- **Poor option quality:** Instead of specific alternatives like "Redis vs. Memcached for caching," we got generic options like "implement caching" vs. "don't implement caching".
- **Overlapping criteria:** Instead of recognizing that related concepts should be grouped together, we got separate criteria, like "User experience" and "Ease of use", where the description for "Ease of use" would discuss user experience.
- **Sparse extraction:** Only 1-2 decisions extracted from hour-long technical discussions that clearly contained more decision points.


The root cause of these bad results was that we were asking the model to simultaneously parse 40k tokens of context, identify decision boundaries, extract structured components, and maintain attribution back to the meeting transcript, all at the same time. This is a cognitive load problem, and no human analyst would approach this task in a single pass.


## The Multi-Step Approach


Based on the simple approach, we thought it would be better to think about how a human would actually tackle this problem. We would never ask a human analyst to read an hour long transcript and immediately output perfectly structured decisions all in one go. They'd first skim for decision points, then analyze each one individually for options and trade-offs, then determine which option was selected.


Our multi-step approach mirrors this natural process:


1. **🔎 High-level decision extraction:** Identify decision points (titles and descriptions) with source line attribution using fine-grained transcript data.
2. **➕ Enrichment:** Extract options, criteria, and insights for each decision independently, using the decision summary and relevant transcript excerpts filtered by source line numbers.
3. **🔄 Deduplication:** Handle subtle duplicates in options and criteria (like "user experience" vs. "ease of use"). We don't deduplicate insights since they're typically unique to specific transcript lines and users.
4. **✅ Selection determination:** Combine decision context and transcript excerpts to determine which option was actually chosen.
5. **🏗️ Object construction:** Combine all components into the final decision object and mark the selected option.


Each step uses[Instructor](https://python.useinstructor.com/) for structured outputs of the LLM responses. This modular architecture provides several advantages: each component can be evaluated and improved separately, we can experiment with different decision extraction methods without touching downstream logic, and each step has a single responsibility with well-defined inputs and outputs.


The multi-step approach produced significantly better results than our simple single-shot method. We extracted more nuanced options and criteria, achieved better decision coverage from long transcripts, and maintained higher quality output throughout the extraction process.


## Data Engineering Insights


While iterating on the multi-step approach, we discovered that how we formatted the transcript data was just as important as the extraction logic itself.


Raw meeting transcript data typically looks like this:


```text
John: I think we should go with Redis for caching
Sarah: What about the memory overhead though?
John: Good point, but we can configure memory limits
Sarah: True, and Redis has better monitoring tools


```


We experimented with different representations and chunking schemes. We settled on a pattern of including the actual line numbers for each new speaker block:


```text
Line 47: John: I think we should go with Redis for caching
Line 48: Sarah: What about the memory overhead though?
Line 49: John: Good point, but we can configure memory limits
Line 50: Sarah: True, and Redis has better monitoring tools


```


This simple augmentation of the raw transcript data solved multiple interconnected problems:


- **Precise source citation:** Each extracted object can reference specific line numbers in the transcript as citations.
- **Focused context:** Because we know exact line numbers, downstream steps can work with only the relevant transcript excerpts instead of the entire meeting.
- **UX enablement:** With precise citations, the platform can highlight the exact source text where each decision component was discussed.


## Chunking the Transcript


One problem we observed with our simple approach was sparse decision extraction, with only 1-2 decisions being extracted from an hour-long technical discussion that definitely contained more decisions. We suspected that giving the LLM the entire transcript at once was overwhelming it with too much context to process effectively.


To test this hypothesis, we implemented a chunking scheme where we grouped the augmented transcript by token limits while preserving speaker boundaries (ensuring we don't cut off a speaker's statement mid-sentence when creating chunks). We settled on 5500-token chunks (empirically derived) that typically resulted in 4-7 groups per transcript, with a 120k token maximum for our no-chunking baseline.


For reference, a standard single-spaced page of text contains about 500 words, which corresponds to roughly 665 tokens. This means our 5500-token chunks represent about 8-9 pages of text, while the full transcripts (15k to 40k tokens) correspond to about 23-60 pages of text.


The results were pleasantly surprising:


- 3-5x more decisions extracted with chunking
- Higher quality options and criteria with chunking (more specific, less generic)
- Better transcript line source attribution accuracy with chunking


We believe that this counterintuitive approach worked better because:


**Attention dilution:** Large context windows may lead to less focused processing across the entire transcript. The model attempts to synthesize everything instead of focusing on specific decision moments.


**Response token constraints:** LLMs have limits on output length. When processing 40k tokens of input, models attempt to compress multiple decisions into their response limits, leading to truncated descriptions and missed decision points rather than comprehensive extraction of individual decisions.


**Focus vs. breadth trade-off:** Smaller contexts enable deeper analysis of specific decision moments, producing more nuanced extraction of options and criteria.


Since chunks can be processed asynchronously, this approach doesn't introduce meaningful latency penalties compared to processing the entire transcript at once. There's a risk of duplicate decision objects across chunks, but our deduplication step handles this effectively. This insight now influences how we approach other long-context extraction tasks - sometimes less context produces better results.


## Testing on Real Data


Testing with synthetic benchmarks can be valuable, but it's difficult to maintain the full context and nuance of real-world scenarios. We tested the complete decision extraction pipeline on two types of meetings from our own internal Convictional organization data on the platform:


- **Weekly Demo Sync meetings:** Cases where decisions were made unconsciously.
- **Engineering alignment sessions:** High-complexity technical decisions with multiple stakeholders.


Our validation methodology was manual review and evaluation by domain experts. That is, team members who actually attended the meetings could verify whether the extracted decisions matched their recollection of what was discussed and decided.


The actual review process was very simple. The details of the extracted decisions were simply loaded into a spreadsheet for domain expert feedback, since our sample size of decisions was relatively small. All details of the decision through the entire LLM flow were included to give the human expert an idea of how the LLM arrived at its conclusions.


The results were promising:


- **Decision extraction was accurate:** We extracted 3-7 decisions per meeting, and domain experts confirmed these represented genuine decisions that were actually made during the discussions.
- **Source attribution was reliable:** Extracted components correctly referenced the transcript segments where they were discussed.


These results gave us confidence that the system was production-ready, validating our systematic approach to breaking down the complex problem into manageable, testable components.


## From Research to Production


One of the most important lessons from this project was learning how to perform research that could actually ship to users. From the beginning, we approached the work with production constraints in mind - using data models and expectations that translate to the actual app environment. Rather than researching in isolation, we coded our decision extraction process to work with existing platform data models and follow established naming conventions, making any handoff to engineering much smoother.


Our modular approach was appreciated by engineering, since each component of the pipeline could be tested, improved, and integrated independently. This meant research findings could be incorporated incrementally rather than requiring a massive all-or-nothing implementation or a messy "go find the relevant parts of the code to pull out into production" process.


Although the human evaluation process did not directly affect how engineering implemented the decision extraction code, the process itself has been incorporated into production engineering flows. What started as a bespoke spreadsheet for every research project evolved into our current human feedback infrastructure built by the research team. Within this new process researchers use a custom App Script and Google Sheets to populate A/B comparisons and let humans decide which is better. This adds statistical rigour to our evaluation process, and engineering now uses it for production-related research, like prompt tuning.


Communication proved just as important as technical design. We learned to frame research findings around what engineering teams needed to make implementation decisions, rather than getting lost in academic details. This meant providing clear recommendations with supporting evidence, but always with an eye toward practical constraints and trade-offs that would matter in production.


This project became our first research initiative to successfully make it all the way to users, establishing patterns that now accelerate how we hand off subsequent projects. The systematic approach we developed has become part of our template for ensuring research actually creates user value rather than remaining in the lab.


## What This Says About AI Engineering


The decision extraction project revealed several meta-lessons about conducting applied AI research and experimentation.


We learned that our intuitions about what would work were often wrong. We assumed that giving the LLM more context at once would lead to better extraction, but only when we actually tested chunked versus non-chunked approaches did we discover that we were incorrect.


Breaking our system into separate, modular pieces made our lives so much easier when we needed to fix or improve something. Instead of having to rebuild everything from scratch and worry about regressions in other parts of the logic, we could focus on just the piece that needed work. For example, if we discover better prompt patterns for extracting options, we can swap that piece out without touching anything else.


It turns out that how we formatted our transcript data, something as simple as adding line numbers, had way more impact on output quality than all the prompt engineering we did. We spent a lot of time perfecting our prompts when the real breakthrough was in the data preparation.


When we stopped trying to force the LLM to do everything at once and started thinking about how humans would actually tackle this problem we saw much better results. The multi-step approach feels natural because it mirrors how humans naturally break down complex tasks, and we believe it works with LLMs for the same fundamental reasons.


These patterns generalize beyond decision extraction. Any complex structured extraction task - whether it's parsing legal documents, analyzing customer feedback, or extracting insights from research papers - benefits from this systematic, modular approach.


This project reinforced an important lesson about research methodology: keeping production constraints in mind while doing research makes the eventual handoff to engineering much smoother. It's not about limiting creativity or cutting corners, it's about building research that can easily be implemented rather than having to rebuild everything from scratch when it's time to ship.


## Looking Forward


The decision extraction process became one of our first applied AI research projects to ship to production, validating our approach to turning applied AI research into practical business value for customers. But more importantly, it established patterns and methodologies that now influence how we tackle other complex AI problems.


The techniques we developed - modular pipeline architecture, systematic chunking strategies, human-inspired task decomposition - are already being applied to other AI-powered features across the platform. Each project builds on the lessons learned from the previous one, creating a cumulative advantage in our ability to integrate AI reliably into our product.


There's something deeply satisfying about taking research work that feels abstract and academic and watching real users actually benefit from it in their daily work. This project reminded us why we love applied research - not just the intellectual challenge of the problem, but the gratification of seeing our systematic methodology translate into something people find genuinely useful.
