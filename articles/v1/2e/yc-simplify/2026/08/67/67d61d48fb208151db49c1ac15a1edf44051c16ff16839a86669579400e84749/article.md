---
schema_version: "1.0.0"
document_id: "67d61d48fb208151db49c1ac15a1edf44051c16ff16839a86669579400e84749"
company_key: "yc-simplify"
company: "Simplify"
source_id: "yc-simplify-news-import-f976f9fcdcd3"
canonical_url: "https://simplify.jobs/blog/can-recruiters-tell-if-ai-wrote-your-resume"
published_at: "2026-08-03T02:29:56+00:00"
first_seen_at: "2026-08-03T14:06:44.817334+00:00"
fetched_at: "2026-08-03T14:10:52.670502+00:00"
content_hash: "sha256:6afcfe0bb808949f3195c2333e8c7c303bf167630afb380ff5532ded5afa77dc"
---

# Can Recruiters Tell If AI Wrote Your Resume? Usually, Yes. Here's What Matters More

Usually, yes. I'm a technical recruiter at Simplify, and unedited ChatGPT gives itself away within about two bullets. But that matters less than you'd expect, because no recruiter I know rejects a resume for being AI-written. We reject resumes for being generic. Raw, unedited AI output just happens to be the most reliable generic-resume generator ever built.


Here's what that looks like from my side of the screen.


## The tells recruiters learn fast


Nobody trained me to spot AI. Read enough resumes and certain patterns start jumping off the page.


**Em dashes.** Language models love this punctuation mark. Almost nobody types one on purpose, because on most keyboards you have to go hunting for it. When I see two or three scattered through a resume, I have a confident guess about who did the writing. (You will not find a single one in this post. Given the topic, it seemed important.)


**The same five verbs.** Spearheaded, leveraged, orchestrated, honed, streamlined. Each is a fine word on its own. When four consecutive bullets open with them, that's a model's habit, not a person's.


**Buzzword adjectives.** Dynamic, results-driven, detail-oriented, self-starting. These plagued resumes long before ChatGPT (we keep a whole list of[resume buzzwords to cut](https://simplify.jobs/blog/resume-buzzwords-and-cliches?ref=blog.simplify.jobs) ), but AI stacks them thicker than any human does.


**Suspicious uniformity.** Every bullet lands between 14 and 18 words, on the same grammatical skeleton. Real people are messier, and the mess reads as human.


**Polish without specifics.** Grammatically immaculate claims that never name a tool or a number I could ask about in a phone screen. This is the big one, the only tell that actually costs you.


Here's a made-up pair. I wrote both; no candidate's resume was harmed.


- *Ungrounded AI:* "Spearheaded cross-functional initiatives to optimize operational workflows and deliver impactful, data-driven results."
- *Grounded:* "Cut the weekly inventory report from six hours to 45 minutes with a Python script that pulls directly from the Shopify API."


The first could sit on any resume in any industry, which means it tells me nothing. The second belongs to exactly one person, and it hands me a screening question I actually want to ask.


## We measured the shift


My tells are anecdotes, so we checked them against Simplify's data. We measured first-role descriptions on resumes created on Simplify, grouped by the year the resume was created: 857 resumes from 2023, 26,674 from 2024, 59,412 from 2025, and 23,445 from 2026 so far.


The em dash tells the cleanest story. On resumes created in 2023, an em dash in the first role description was a one-in-a-thousand event (0.0% after rounding). In 2024 it was 0.1%. In 2025, 1.6%. On 2026 resumes it's 2.7%, roughly one in 37. That's a rise of about 27x in three years, and it tracks exactly with AI drafting going mainstream. Job seekers did not suddenly develop a taste for a punctuation mark most people can't type from memory. The authorship changed.


The verb habit moved the same way. At least one of the five AI-favorite verbs (spearheaded, leveraged, orchestrated, honed, streamlined) appeared in 32.2% of first-role descriptions on resumes created in 2023. By 2025 it was 52.0%. When it feels like every other resume opens with "Spearheaded," that's because more than half of them now do.


Two disclosures, since this is our own data. Simplify's resume builder offers AI drafting, so the drift includes output from every AI tool, ours included; the language shift is real and visible whichever model wrote it. And these are text-pattern measurements on first-role descriptions, so read them as directional rather than a census.


## What actually gets a resume rejected


I want to be precise here, because "recruiters can tell" scares people in the wrong direction. I have never rejected a candidate because I suspected AI involvement, and I don't know a recruiter who has. There is no detector in our stack and no checkbox for it.


What gets a resume skimmed past is content that could belong to anyone. That's exactly what a vague prompt produces. Ask a model to "write resume bullets for a software engineering intern" and it writes filler, because it doesn't know what you did. It papers over the gaps with plausible-sounding generalities, and those collapse under a single interview question.


That's my test: read a bullet aloud and ask a follow-up. "Tell me about the operational workflow you streamlined. What was it before? What did you change?" A grounded bullet turns into a five-minute story. An ungrounded one turns into a long pause, and the pause costs you far more than the em dash ever did.


Detection was never the real risk. Submitting a resume with nothing of you in it is.


## How to use AI without inheriting its accent


The fix is grounding, not abstinence. AI drafting from your real experience reads like you on a good writing day. AI drafting from a blank prompt reads like everyone at once.


Grounding means handing the model your raw material before it writes a word: the actual project, the tool names, the number you improved, the thing that broke and how you fixed it. "Write me a resume bullet for a software intern" produces filler. "I built a Flask dashboard that cut our on-call triage time from 20 minutes to 5, write me a bullet" produces a resume line. That's also how Simplify's[resume builder](https://simplify.jobs/resume-builder?utm_source=blog&utm_medium=cta&utm_campaign=ai-resume-detection) approaches it: drafts start from the roles, dates, projects, and skills already in your profile instead of from a blank prompt.


Then do a human pass. Mine would look like this:


1. Delete every em dash. A period or a comma always works.
2. If two bullets open with the same verb, change one. Swap the famous five for plainer verbs: built, cut, ran, shipped, fixed.
3. Add the number only you know. Models can't invent your real metrics, which is exactly why they convince.
4. Check your tenses. Past for previous roles, consistent throughout; our guide to[past vs. present tense](https://simplify.jobs/blog/resume-past-or-present-tense?ref=blog.simplify.jobs) covers the edge cases.
5. Read each bullet aloud. If you wouldn't say it in a phone screen, rewrite it until you would.


Ten minutes of this and the resume stops reading like a model and starts reading like a person who writes well.


## Common questions


**Do companies run AI detectors on resumes?**
Rarely, and the detectors that exist are unreliable, with documented false positives on fully human writing. Applicant tracking systems parse your resume and match it against the job description; they don't score how AI-ish the prose sounds. If you're worried about software, worry about[formatting and keywords](https://simplify.jobs/blog/how-to-write-an-ats-optimized-resume?ref=blog.simplify.jobs) , not detection.


**Will I get rejected for using ChatGPT to write my resume?**
Not for using it. You can absolutely get rejected because of what unedited ChatGPT wrote: vague, interchangeable bullets that give a recruiter nothing to grab onto. The tool is fine. Its default output is what fails you.


**Is it cheating to use AI on your resume?**
No. People have always gotten help with resumes, whether from a career coach or a friend's red pen, and nobody calls that cheating. The line is truth. Describing your real work with AI's help is fair game. Inventing work you never did is a lie no matter who typed it.


**How do I make AI writing sound like me?**
Feed it your specifics before it drafts, then edit what comes back: cut the em dashes, vary the opening verbs, put back the details only you could know, and read it aloud. If a bullet could appear on a stranger's resume unchanged, it isn't yours yet.


## Start from your real experience


The cheapest way to get grounded drafting is to never start from a blank prompt. Simplify's[resume builder](https://simplify.jobs/resume-builder?utm_source=blog&utm_medium=cta&utm_campaign=ai-resume-detection) writes bullets and summaries from the experience already in your profile, so the first draft names your actual projects instead of guessing at them. It's free, and it leaves you with the one job AI can't do: adding the number only you know.
