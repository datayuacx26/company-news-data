---
schema_version: "1.0.0"
document_id: "691559bfd43630a00db74712b63163d4fd1dc3664499a639de0d55330398c9a9"
company_key: "yc-letterdrop"
company: "Letterdrop"
source_id: "yc-letterdrop-news-import-e2cb75c1c09f"
canonical_url: "https://letterdrop.com/blog/chatgpt-prompts-for-outbound-sales"
published_at: "2026-01-05T00:00:00+00:00"
first_seen_at: "2026-07-23T16:23:30.256512+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:37920a267fedbd025d025c50294d52db6c9706349e6546415e106c2e8c13387d"
---

# ChatGPT Prompts for Outbound Sales

Let’s start with what’s real:


- Poorly researched outbound doesn’t work
- Good outbound requires:


- Strong segmentation
- Clear observations
- Correct persona → message matching
- A differentiated product
- A compelling reason to reply


- AI can help with all of the above **if you tell it exactly what to do**


Here’s the problem:


- Most BDRs and AEs don’t know how to prompt AI
- Most RevOps, GTM engineers, and marketers who know how to prompt don’t understand outbound


This guide exists to bridge that gap.


##
Understanding Good Outbound Copy (Before Prompting)


Before you touch prompting, you must understand what[good outbound copy](https://letterdrop.com/blog/how-to-write-outbound-copy) actually looks like.


Prompting does not fix bad fundamentals.


It only scales them.


If you skip this step, you’ll get AI-written spam faster.


## [✍️ How to Write Great Outbound Copy](https://letterdrop.com/blog/how-to-write-outbound-copy)


What ChatGPT (or Any LLM) Needs to Write Good Outbound Messages


If you ask ChatGPT to “write an outbound message,” the output will be bad by default.


LLMs only perform well when you give them **clear context and tight constraints** .


At a minimum, they need five things.


###
1. Full Context on Your Company


- What your product or service does
- Who you sell to
- The problems you solve
- How you uniquely solve them
- Existing customers or case studies it’s allowed to reference


###
2. Full Context on the Lead


- Name, job title, company
- Company website
- LinkedIn profile
- A specific observation that explains *why* you’re reaching out


###
3. Web Access (If You Expect Research)


If you want the model to find new information, it needs access to:


- LinkedIn
- Company websites
- Blogs, case studies, news
- Podcasts, interviews, 10-Ks, etc.


###
4. Clear Instructions


Tell the model:


- Which signals to research and how to prioritize them
- Which pain points matter for each persona
- How to pitch your product
- Which CTAs are acceptable


###
5. High-Quality Samples


Provide fully formed examples of what “good” looks like.


This dramatically improves output quality.


## How to Structure a Strong Outbound Prompt (Plain English)


Whether you’re prompting ChatGPT manually or using a tool built on top of LLMs, the structure should be the same.


From top to bottom, a strong prompt tells the AI:


- Who you’re reaching out to
- What your company does
- What signals to research
- Which customers it can reference
- How to pitch your product
- Which pain areas to focus on
- Which org function the lead belongs to (for referrals)
- What good outbound messaging looks like
- Full examples to follow


Anything in **ALL CAPS** is meant for you to customize.


##


### Example: ChatGPT Prompt for Outbound Sales


```text


I'm a BDR at {{your_company_name}}. I want to reach out to:


Name: {{lead_full_name}}
Company: {{lead_company_name}} ({{lead_company_website}})
Title: {{lead_job_title}}
LinkedIn: {{lead_linkedin_full_profile}}


Here is some information about our company:
<about_us>
{{your_company_description}}
</about_us>


Research some relevant signals:
<signals>
(LIST YOUR SIGNALS HERE)
</signals>


These are existing company customers for references:
<existing_customers>
(LIST YOUR CASE STUDY CUSTOMERS HERE)
</existing_customers>


<product_pitch>
(PUT YOUR PRODUCT PITCH HERE. SEGMENT BY PERSONA IF NEEDED)
</product_pitch>


Possible pain areas:
<pain_areas>
(LIST THE PAIN AREAS YOU SOLVE HERE. SEGMENT BY PERSONA IF NEEDED)
</pain_areas>


Relevant functions for routing:
<relevant_functions>
(NAME THE ORG FUNCTION YOU SELL INTO)
</relevant_functions>
```


##
How to Control the Final Outbound Message Output


Once your prompt gives the model the right inputs, you still need to control how the message is written.


This is where most AI-generated outbound falls apart: the structure is loose, the tone drifts, and the message turns into a pitch.


The sections below work together to prevent that.


###
1. Style Rules for a Casual Outbound DM


These rules constrain the model so the message sounds human, not AI-written.


- Max 85 words
- No sign-off
- Don’t mention job titles
- Allowed punctuation only:` . - ? ,`
- Ban fluff words: innovative, cutting-edge, thrilled, delighted, synergy, leverage
- No em dashes


These constraints matter more than people think.


They force clarity and prevent the model from “selling.”


###
2. OPPS Format for Outbound Messaging


Use a simple structure so the model doesn’t jump straight into a pitch.


```text


Hi {{lead_first_name}}!
{Observation tied to signals and inferred pain}
{How you solve that pain}
{Learning-focused CTA}
{Optional referral line if C-suite}
```


This format mirrors how a good human outbound message flows.


###
3. How the Final Message Is Assembled


Anything inside` {}` tells the AI **how to think** , not what to copy verbatim.


The structure should always resolve to:


1. A specific observation
2. A likely priority or blocker
3. How you solve it (with proof)
4. A learning-focused CTA


This sequencing is what keeps the message educational instead of salesy.


###
4. Using Tags to Keep Prompts Organized


When giving the model lists (customers, signals, pain areas), always wrap them in tags.


Example:


```text


<customers>
- Acme: Manufacturing
- Northwell: Healthcare
- PetroChina: Energy
</customers>
```


Tags reduce ambiguity and help the model reason over grouped information consistently.


## Choosing the Right Model (Cost vs Quality)


Use the cheapest model that produces acceptable results.


If your prompt requires new research, it must have search access.


A practical approach:


1. Test with a high-quality model (e.g. GPT-5.1)
2. Run the same prompt on cheaper models
3. Compare outputs on the same leads
4. Keep downgrading until quality drops


Stop at the lowest-cost model you’re happy with.


##
How Letterdrop Applies This Prompting Framework


Letterdrop applies this same framework, but automates the slow parts.


It pulls lead context from the web, including whether that contact is actively evaluating a competitor, keeps your prompt variables structured, and runs the same constraints consistently across large lead lists.


When a competitor signal is involved, it also attaches a confidence score and a recommended play, so the prompt isn't just personalized, it's timed to something real.


This might come in handy for scale.


#### Automatic outbound sequences for intercepting in-cycle deals


Funnel competitor signals + relevant outreach directly into your stack


[See How Letterdrop Does This](https://letterdrop.com/demo)
