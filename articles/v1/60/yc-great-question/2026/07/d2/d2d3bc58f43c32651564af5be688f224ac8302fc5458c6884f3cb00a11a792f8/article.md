---
schema_version: "1.0.0"
document_id: "d2d3bc58f43c32651564af5be688f224ac8302fc5458c6884f3cb00a11a792f8"
company_key: "yc-great-question"
company: "Great Question"
source_id: "yc-great-question-news-import-f3444748def0"
canonical_url: "https://greatquestion.co/blog/claude-skills-ux-research-assistant"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-23T10:44:58.935973+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:0c3f736c5317d3ce29d3d468888e1f3019385ed24b8c06c2c7945072a3b80824"
---

# 17 Claude skills that turn AI into your UX research assistant

*Updated July 2026: This guide now covers 17 Claude skills (originally 8). The six newest (#12–17) are a different breed: MCP-powered skills that take real actions inside Great Question, from setting up studies to recruiting participants to drafting readouts. Great Question also has an MCP, with research power built-in.*[Request access here](https://greatquestion.co/features/mcp-integration) *.*


‍


You don't need another AI tutorial. You need tools that work the moment you install them.


Claude skills are reusable instruction sets that give Claude deep expertise in specific tasks. Think of them like hiring a specialist contractor: you install the skill once, and every time you hand Claude a relevant task, it knows exactly how to handle it: the methodology, the edge cases, the output format.


For UX researchers, this changes the math on what's worth automating. Not the thinking parts (that's still yours), but the parts that eat your afternoons: cleaning transcripts, tagging highlights, drafting screeners, building readout decks.


Here are 17 skills we've built and tested on real research projects. Each one targets a specific bottleneck in the research workflow, and each one is ready to install.


## How Claude skills work (30-second version)


A skill is a markdown file (SKILL.md) that contains detailed instructions for Claude. When you install one, Claude reads it before handling related tasks — like giving a new team member a thorough briefing before they start work.


You can install skills in Claude Code, Claude Desktop, or any MCP-compatible client. Drop the file in your skills folder, and you're done.


The skills below work independently, but they're designed to chain together. Clean a transcript, then synthesize across 10 cleaned transcripts, then generate a stakeholder readout from the synthesis. Each step feeds the next.


## 1. Transcript cleaner


**The problem it solves:** Raw transcripts from Otter, Fireflies, Zoom, Rev, or Grain are a mess. Speaker labels are inconsistent ("Speaker 1" vs. "John" vs. "J. Smith"), filler words clutter every sentence, timestamps break up the flow, and the formatting makes analysis painful.


**What this skill does:** Takes a raw transcript and produces a clean, analysis-ready document. It normalizes speaker labels to consistent names, strips filler words (um, uh, like, you know) while preserving meaning, removes or reformats timestamps, fixes obvious transcription errors from context, and structures the output with clear speaker turns.


**When to use it:** Right after your recording tool exports a transcript and before any analysis begins. This is always step one.


**What makes it different from just asking Claude to "clean this up":** The skill includes specific rules for handling overlapping speech, preserving emotional cues (laughter, sighs, pauses) that matter for analysis, and maintaining verbatim accuracy on key quotes even while cleaning surrounding text. Without the skill, Claude tends to over-edit — smoothing out the rough edges that researchers actually need.


**Pro tip:** If you're using[Great Question](https://greatquestion.co/) for your studies, the platform handles transcript cleanup automatically. This skill is for transcripts from other sources you want to bring into your analysis workflow.


## 2. Research synthesizer


**The problem it solves:** You've run 12 interviews. You have 12 cleaned transcripts. Now you need to find the patterns, contradictions, and key themes across all of them — and that's a full week of work.


**What this skill does:** Processes multiple transcripts (or sets of notes) and generates a structured synthesis. It identifies recurring themes with supporting evidence from specific participants, flags contradictions between participants, highlights surprising or unexpected findings, and maps the strength of each theme based on how many participants mentioned it.


**When to use it:** After you've cleaned your transcripts and before you start building a readout or making recommendations. Works best with 5–15 transcripts. For larger sets (15+), run it in batches of 10–12 and then synthesize the batch outputs.


**The output format:** You get a structured document with themes ranked by prevalence, direct quotes mapped to each theme, a contradictions section (this is where the gold is), and a "signals worth investigating" section for things that only 1–2 participants mentioned but that seem significant.


**Important caveat:** This skill accelerates pattern recognition, but it doesn't replace interpretation. The AI identifies that 8 of 12 participants mentioned onboarding friction — you decide what that means for the product roadmap. Use it to organize the data, not to draw conclusions.


If you're running research at scale,[Great Question's AI analysis](https://greatquestion.co/features/ai) handles cross-study synthesis natively across your entire research repository.


## 3. Discussion guide builder


**The problem it solves:** Writing a good discussion guide takes 2–3 hours. Not because it's hard, but because you're balancing research objectives against conversation flow, making sure questions are open-ended enough, building in probes for each topic, and sequencing warm-up through core through wind-down.


**What this skill does:** Takes your research objectives and target participant profile, then generates a semi-structured discussion guide. It includes a warm-up section (non-threatening questions to build rapport), core topic blocks with primary questions and follow-up probes, transition language between topics, a closing protocol (anything else, referral requests, next steps), and estimated timing for each section.


**When to use it:** During research planning, after you've defined your research questions but before you start scheduling sessions.


**What it doesn't do:** It won't tell you what to research. If your research objectives are vague ("understand the user experience"), the guide will be vague. Garbage in, garbage out. Start with specific research questions: "How do enterprise teams currently manage participant recruitment across multiple researchers?"


**The skill's secret weapon:** It flags questions that are likely to produce yes/no answers or leading responses and suggests alternatives. This is the thing most discussion guides get wrong — asking "Do you find onboarding confusing?" instead of "Walk me through what happened when you first set up your account."


## 4. JTBD interview guide builder


**The problem it solves:** Running a Jobs-to-Be-Done interview isn't the same as a generic discovery chat. The Bob Moesta timeline method has a specific arc — first thought, passive looking, active looking, decision, first use — and skipping a stage or asking the wrong probing question wastes the session. Most teams either run JTBD interviews loosely (and miss the rich forces data) or skip the method entirely because building a proper guide takes hours.


**What this skill does:** Generates a complete JTBD moderation guide based on the Competing Against Luck methodology. It builds out the six-stage timeline walkthrough with opening questions and 3–4 probes per stage, includes a four forces deep-dive section (push, pull, anxiety, habits), adds desired outcomes questions, and writes facilitation notes telling the moderator what to listen for at each stage — frustration language, aspiration language, anxiety language, habit language.


**When to use it:** Whenever you need to understand why someone switched to (or away from) a product — adoption research, churn analysis, competitive intel. Especially useful for PMs and designers running interviews without a dedicated researcher in the room.


**The detail that matters:** The skill adapts the guide based on participant type. Churned users get an exit-interview framing. Prospects who haven't switched yet get hypothetical probing. Existing users get the full timeline walkthrough. That's the difference between a guide that produces rich switching stories and one where the participant doesn't know what story you're trying to walk them through.


## 5. Insight tagger


**The problem it solves:** You've pulled 200 highlights from your research. Now you need to categorize them: what theme does each one belong to? What's the sentiment? Which ones connect to insights from previous studies?


Doing this manually across a large study takes days. Doing it consistently across multiple studies takes a system most teams don't have.


**What this skill does:** Takes a set of research highlights (quotes, observations, notes) and applies a structured taxonomy. It assigns theme tags to each highlight, tracks sentiment (positive, negative, neutral, mixed), suggests connections to other highlights in the set, and builds a category hierarchy that emerges from the data rather than being imposed on it.


**When to use it:** After synthesis, when you're organizing insights for your research repository. Also useful for ongoing repository maintenance — run it on new highlights as they come in to keep your taxonomy consistent.


**Integration with Great Question:** If you're using[Great Question's repository](https://greatquestion.co/features/research-repository) , this skill complements the platform's built-in tagging. Use it for batch processing highlights from external sources before importing them, or to audit and clean up existing tags.


**A word on taxonomy design:** The skill generates tags from the data, which is great for discovery. But over time, you'll want to align these with your team's existing taxonomy. Run the tagger, review what it generates, then merge similar categories and promote the ones your team actually uses.


## 6. Stakeholder readout generator


**The problem it solves:** You've done the research. You have the findings. Now you need to present them to three different audiences: the PM who wants "just tell me what to build," the design lead who wants the user journey details, and the VP who wants business impact.


Same research, three different framings. That's three decks — or at least three significantly different sections.


**What this skill does:** Takes your synthesis or findings document and generates stakeholder-ready readout content. You specify the audience (PM, design, leadership, cross-functional), and it reframes accordingly. For PMs: leads with actionable recommendations, maps findings to product decisions. For designers: centers on user behavior, pain points, and journey moments. For leadership: frames everything around business impact, risk, and opportunity.


**When to use it:** After synthesis, before your readout presentation. This doesn't replace your thinking about what matters — it reformats your thinking for the audience.


**What it actually outputs:** Not a slide deck (use a presentation tool for that). It gives you structured content blocks with headers, key points, supporting evidence, and recommended actions — organized in the narrative flow that works for each audience. Copy these into your preferred presentation format.


## 7. Screener builder


**The problem it solves:** Recruiting the right participants is half the battle. A weak screener wastes everyone's time: you end up interviewing people who don't match your criteria, or you disqualify good participants with poorly worded questions.


**What this skill does:** Takes a target persona description and research objectives, then generates a screening survey. It includes 8–12 screening questions with qualification logic, red-flag disqualifiers (professional survey takers, competitor employees, etc.), a scoring rubric so you can rank-order candidates rather than just pass/fail, and response validation patterns.


**When to use it:** During study setup, after you've defined who you want to talk to. The output is formatted to paste directly into[Great Question's recruitment tools](https://greatquestion.co/features/research-recruitment) or your screener platform of choice.


**The detail that matters:** The skill differentiates between hard disqualifiers (must-have criteria) and soft preferences (nice-to-have characteristics). This prevents the common mistake of building a screener so strict that nobody qualifies. It also randomizes response options and includes attention-check questions to filter out people who aren't reading carefully.


## 8. Survey designer


**The problem it solves:** Survey questions look easy to write and are easy to get wrong. Leading questions, double-barreled prompts, hypothetical traps, and seven-point scales nobody can interpret consistently — these are the failure modes that quietly kill survey data quality. Most teams either write surveys quickly (and get useless data) or over-engineer them (and never ship).


**What this skill does:** Builds a research-quality survey from a clearly stated goal. It structures the survey with a context-setter, behavioral anchors, core questions, and an open-ended closer. It picks response formats that actually match the research question (frequency scales for behaviors, Likert for opinions, NPS for advocacy, max-diff for ranking). It quality-checks every question for leading language, jargon, and double-barreled framing before output.


**When to use it:** Three common scenarios. Feature validation (when you have a proposed solution and need to pressure-test it against actual user behavior). Concept testing (when you're choosing between directions). Post-interview follow-up (when you've found themes qualitatively and want to test their prevalence at scale).


**The thing it gets right:** The skill includes rationale for every question — a one-to-two sentence "why this is here" that helps you defend the survey to stakeholders and trust the data when results come in. That's the difference between a survey people fill out and a survey you can actually act on.


If you're running surveys natively,[Great Question's survey tool](https://greatquestion.co/features/surveys) handles the logic branching, distribution, and analysis end-to-end.


## 9. Affinity mapper


**The problem it solves:** Affinity mapping is one of the most powerful synthesis methods in research — and one of the most tedious. Sorting hundreds of observations into meaningful clusters, naming those clusters, then looking for patterns across clusters. On a physical wall, it takes half a day. Digitally, it's often done in FigJam or Miro, but you're still doing all the grouping manually.


**What this skill does:** Takes a set of observations, quotes, or notes and runs a digital affinity mapping process. It groups related observations into clusters, suggests descriptive names for each cluster, identifies outlier observations that don't fit neatly (these are often the most interesting), and shows the relationships between clusters.


**When to use it:** During analysis, especially when you have a large volume of unstructured observations. Works well with interview notes, usability testing observations, survey open-ends, or support ticket themes.


**Important:** this is a starting point, not the finished analysis. The skill's groupings are based on semantic similarity — words and concepts that appear together. But research insight often comes from unexpected connections that aren't semantically obvious. Use the AI-generated map as a draft. Move things around. Challenge the categories. The value is in the 80% that's already sorted correctly, freeing you to spend your time on the 20% that requires human judgment.


## 10. JTBD analyzer


**The problem it solves:** You've run your JTBD interviews. Now you need to extract the four forces from each transcript, find consensus and divergence across participants, and turn it all into a job statement you can actually share with the product team. Doing this manually for a single transcript takes 60–90 minutes. Across eight interviews, that's most of a week.


**What this skill does:** Reads transcripts (or notes) and produces a structured JTBD analysis. It writes a core job statement in Moesta's "when… I want to… so I can…" format, maps push/pull/anxiety/habit forces with evidence and quotes from the transcript, ranks the magnitude of each force, lists desired outcomes with supporting evidence, identifies competing solutions the participant was firing, and flags unmet jobs as product opportunities.


**When to use it:** After your JTBD interviews are cleaned, when you're ready to synthesize. Works on single transcripts (deep individual portrait) or batches (cross-study synthesis with consensus, divergence, and segment hypotheses).


**Why this one is paired with the synthesizer:** The general research synthesizer (#2) finds themes. The JTBD analyzer applies a specific theoretical lens — Bob Moesta's four forces framework — which is more useful when you're trying to understand a switching decision than to map a general experience. Use the synthesizer for "what did we learn." Use this for "why did they hire it."


## 11. Research brief writer


**The problem it solves:** Every research project needs a brief, and every brief needs the same things: background, objectives, methodology, participant criteria, timeline, success metrics. Writing one from scratch takes 1–2 hours. Adapting a template still takes 45 minutes.


**What this skill does:** Takes a business question or product decision and generates a complete research brief. It recommends a methodology with justification (why interviews over surveys for this question, why moderated over unmoderated), defines success criteria, scopes participant requirements, and estimates a realistic timeline.


**When to use it:** At project kickoff, when a PM or stakeholder brings you a question and you need to translate it into a research plan. Also useful for training purposes: show junior researchers what a solid brief looks like for different study types.


**The methodology recommendation is the standout feature.** Instead of just filling in a template, the skill evaluates the research question against method fit. Exploring a problem space? It recommends generative interviews. Comparing two design options? It suggests unmoderated usability testing. Need to validate with numbers? Survey with the right sample size calculation.


## NEW: skills that act inside Great Question


The 11 skills above turn Claude into a research analyst working on files you hand it. This next set turns Claude into an operator inside your research platform.


Each one is a skill that works with Great Question through its[MCP integration](https://greatquestion.co/features/mcp-integration) . Instead of cleaning a transcript you exported, these skills spin up the study, shortlist the candidates, draft the invitation, and pull the findings, taking actions in your account, then handing you back the small handful of decisions that actually need a human.


You install them the same way: copy the template into a SKILL.md file, edit the placeholders that describe your team's rules (naming conventions, incentive policy, tone, disclosures), and leave the platform mechanics alone. They're built to be safe by default, nothing gets sent or recruited without your explicit confirmation.


## 12. Interview study builder


**The problem it solves:** Standing up a moderated interview study is a lot of clicking. You create the study, write the screener, add moderators, configure scheduling and the livestream, set the incentive.


**What this skill does:** Takes a research brief and builds the entire interview study inside Great Question in one orchestrated pass: study creation, screener, moderators, scheduling configuration, livestream, and incentive. It stops at the point where a human should take over, you activate the study in the GQ UI when you're ready to recruit.


**When to use it:** At study kickoff, the moment you have a brief or enough context to describe one. This replaces the manual setup grind so you go from "I have a brief" to "ready to activate" without leaving your AI tool.


**What makes it different:** It's not drafting a screener for you to paste somewhere, it's building the actual study in your account, wired to your team's defaults. Edit the "Your rules" block once (title format, default duration, incentive method, what's always on your screener) and every study it builds follows your conventions automatically.


**Requires the Great Question MCP.** This skill acts inside your Great Question account, so it only works with the[Great Question MCP integration](https://greatquestion.co/features/mcp-integration) connected.


## 13. Unmoderated test builder


**The problem it solves:** Prototype tests, card sorts, tree tests, and survey-style task collections all need the same fiddly setup — the study, the blocks that make up the test itself, a screener, an incentive, before a single participant sees them.


**What this skill does:** Takes a brief and builds a complete unmoderated test in Great Question: study creation, block authoring (the body of the test), screener, and incentive. Like its moderated sibling, it takes you to "ready to activate" and hands the activation back to you.


**When to use it:** Whenever you're launching an unmoderated study, prototype testing, website testing, card sorting, tree testing, or a set of survey tasks — and want the scaffolding built for you so you can focus on the test design.


**The detail that matters:** It authors the blocks, not just the shell. You describe the test you want and it sequences the actual test content to match, then applies your team's participation limits, screener rules, and tone to everything participant-facing.


## 14. Study email writer


**The problem it solves:** Every study needs invitations, reminders, and follow-ups, and every one of them has to sound like your team and clear your compliance bar. Written fresh each time, they drift in voice and quietly miss required disclosures.


**What this skill does:** Drafts, revises, and reviews all the participant-facing emails on a study — covering every message kind Great Question supports, from the first invitation to the post-session thank-you — with your voice, mandatory disclosures, and incentive-mention rules applied consistently every time.


**When to use it:** Any time you're writing or improving the emails participants receive. Useful as part of study setup, or as a cleanup pass on messaging that's gotten inconsistent across studies.


**What makes it safe:** It drafts and revises; it never sends without your confirmation. You bake your voice, required disclosures, and incentive rules into the template once, and the skill stops guessing on the things that carry legal or brand risk.


**Requires the Great Question MCP.** This skill acts inside your GQ account, so it only works with the[Great Question MCP integration](https://greatquestion.co/features/mcp-integration) connected.


## 15. Recruiter


**The problem it solves:** Recruiting is a chain of small steps — find the right people, check them for fit, shortlist them onto the study, send screener invitations — and doing it consistently across a team means remembering rules (cooldown periods, batch sizes) that live in someone's head.


**What this skill does:** Takes you from "I want to talk to this kind of person" to "the invitations are out." It handles candidate discovery from a saved segment, a search, or a hand-curated list, filters for fit against your rules, shortlists candidates onto the study, and sends screener invitations.


**When to use it:** Whenever you're recruiting for a study — whether you name the people, point it at a segment, or just describe who you're looking for.


**The guardrail:** Sending requires explicit approval. The skill assembles the shortlist and shows you who it's about to contact, applying your org's cooldown periods and batch sizes, and waits for your go-ahead before anything leaves the building.


## 16. Stakeholder readout generator (MCP version)


**The problem it solves:** The findings live in your research repository; the readout lives in a deck or an email. Bridging the two — pulling the right highlights, finding supporting quotes, framing it for the audience — is manual every time.


**What this skill does:** Pulls recent findings, highlights, and insights from one or more studies directly out of Great Question, then drafts a stakeholder readout in your team's preferred format, complete with supporting quotes lifted from the actual transcripts.


**When to use it:** When you need to brief leadership, a stakeholder, or a cross-functional audience on one or more studies and want a first draft you can drop into a deck or send as an email.


**How it differs from the file-based readout skill (#6):** That one reformats a synthesis document you already have. This one goes and gets the material — insights, highlights, and quotes — from your repository itself, so you're not hunting for evidence before you can write.


## 17. Panel hygiene auditor


**The problem it solves:** Panels degrade quietly. The same willing participants get over-recruited, some segments run thin, and people get contacted inside their cooldown window — and you usually only notice when response quality drops or someone complains.


**What this skill does:** Runs a structured read of your candidate panel: who's been over-recruited, which segments are running low, which candidates are inside their cooldown window, and where it would recommend recruiting from next. It's a read-only review — it audits and reports, and changes nothing in Great Question by itself.


**When to use it:** Before a recruitment-heavy study, or on a regular cadence to keep your panel healthy. Pair it with the recruiter (#15): this one tells you where the panel is strained, that one acts on it.


**Why it's read-only on purpose:** Auditing and acting are deliberately separated. This skill surfaces the problems and proposes remediation; it won't shortlist, invite, or modify candidates. That keeps the health check honest and puts the decision to act back in your hands.


**Requires the Great Question MCP.** This skill reads from your GQ account, so it only works with the[Great Question MCP integration](https://greatquestion.co/features/mcp-integration) connected.


## How to chain these skills together


These skills are useful individually, but they're designed to work as a pipeline. Here's the typical flow for a qualitative research project:


**Planning phase:** Research brief writer (defines the project) → Discussion guide builder or JTBD interview guide builder (creates interview protocol) → Screener builder (sets up recruitment criteria). For survey-based studies: Research brief writer → Survey designer → Screener builder.


**Analysis phase:** Transcript cleaner (processes raw recordings) → Insight tagger (categorizes highlights) → Affinity mapper (clusters observations) → Research synthesizer or JTBD analyzer (finds patterns and forces across everything)


**Communication phase:** Stakeholder readout generator (formats for each audience)


## Getting started


1. Pick one skill that targets your biggest time sink. For most researchers, that's the transcript cleaner or synthesizer.
2. Install it in your Claude environment (Claude Code, Claude Desktop, or Cowork).
3. Run it on a real project
4. Add more skills as you get comfortable. The chaining is where the real efficiency gains happen.


If you're using[Great Question](https://greatquestion.co/) , many of these capabilities are already built into the platform, from AI-powered analysis to automated tagging in your research repository. These skills extend that functionality into your broader AI workflow, especially when you're working with data from multiple sources.


Want all 17 skills ready to install?[Download the AI Research Toolkit](https://greatquestion.co/resources/ai-research-toolkit) — includes the skills, a prompt library, and workflow templates.


## FAQ


**What exactly is a Claude skill?**


A Claude skill is a markdown file containing detailed instructions that Claude reads before handling related tasks. When you install a skill, Claude gains expert-level knowledge about a specific workflow — the methodology, edge cases, and output format. It's like giving a new team member a thorough briefing that they reference every time they do that type of work.


**Do I need Claude Code to use these skills?**


No. Skills work with Claude Code, Claude Desktop (via the Settings → Capabilities → Skills upload), and Cowork. The installation method varies slightly by client, but the skill files themselves are the same.


**Can I customize these skills for my team's specific workflow?**


Yes — that's actually encouraged. A skill is just a markdown file. Open it, edit the instructions, save. If your team uses a specific taxonomy for tagging, edit the insight tagger skill to use those categories. If you have a standard discussion guide format, update the builder to match.


**How do these skills compare to Great Question's built-in AI features?**


[Great Question's AI](https://greatquestion.co/features/ai) handles analysis, tagging, and synthesis natively within the platform — no setup required. These Claude skills extend similar capabilities to data that lives outside Great Question: transcripts from other recording tools, notes from field studies, or insights from external sources. They work together, not as replacements.


**Will AI skills replace the need for human researchers?**


No. These skills automate the mechanical parts of research — cleaning, sorting, formatting, pattern-matching. The intellectual work (choosing what to research, interpreting what findings mean, deciding what to recommend) stays with the researcher. The goal is to spend more time on thinking and less on processing.


**How long does it take to see results?**


Most researchers report time savings on their first project. The transcript cleaner saves 1–2 hours per interview immediately. The synthesizer typically cuts analysis time by 40–60%. The compound effect across a full project (using 3–4 skills together) is where the numbers get significant.
