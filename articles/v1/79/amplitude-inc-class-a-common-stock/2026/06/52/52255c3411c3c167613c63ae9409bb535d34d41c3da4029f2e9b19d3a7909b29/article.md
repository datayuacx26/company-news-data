---
schema_version: "1.0.0"
document_id: "52255c3411c3c167613c63ae9409bb535d34d41c3da4029f2e9b19d3a7909b29"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/putting-a-number-on-ai-quality"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-22T23:44:16.700638+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:ff2bbc6ed4730e99c5935988394bb6fe483d66519e20494956dfc8730cb8c8fe"
---

# Putting a Number on AI Quality

The Economist Group is best known for its newspaper that has existed since 1843. In 1946, the Economist Intelligence Unit was set up to answer questions Economist readers were asking. Today EIU helps businesses, financial firms and governments to understand how the world is changing and how that creates opportunities to be seized and risks to be managed. EIU’s team of analysts cover nearly every country in the world with analysis, forecasts and indicators to back decisions. They create a lot of content. And data. It can be hard to navigate.


Enter Lens. In March, we added the multi-turn AI research assistant Lens to Viewpoint, EIU’s website. It allows our analyst, strategist and risk customers to ask about the fiscal outlook or compare political risk across the five biggest economies in South America and get an answer sourced entirely from EIU content. Getting Lens to a good enough quality was hard work. We worked with our analysts on evaluations. We went back and forth until we were happy. By the time we launched we knew Lens was good. But a feature in the wild is a different beast and pinpointing the areas to improve was the next challenge.


## From sessions to signal


Product analytics showed us what users did. It couldn't tell us whether the AI answer was any good. A user who asked three questions in a session might have been getting excellent answers and going deeper. Or they might have been rephrasing the same question because the first two answers were wrong. The data looked the same either way. We opened sessions and read them. But not fast enough to act on it. First impressions matter.


Lens sessions landed into Amplitude with signals attached out of the box: intent classification, outcome tracking, quality scores. Getting there took one engineer and some back and forth, but once it was running, reading a trace stopped being archaeology. I could open a session, see each turn, see what the agent retrieved and how it responded, and see how it scored. Before that, our data insights lead, our platform engineers and I were drawing conclusions from handfuls of sessions. A few examples, a hunch. Now we're looking at the same evidence across all of them.


## Filtering by behavior


Traces


9,240 sessions


**312** of 9,240 · Power users


Power users ×


Filter


Pull every support ticket tagged billing


Power user


Draft a release note for v4.12


New user


Compare YoY active-user growth


Dormant


Build a cohort of accounts that activated


Power user


Generate a weekly retention email


New user


Why did session length drop 18%


Dormant


Summarize churn risk by segment


Dormant


Explain the drop in trial conversions


New user


Find accounts stuck in onboarding


Dormant


Agent name


Cohort ›


Has tool errors


Select cohort


Power users


New users


Dormant


Cohort definition


Power users


Include users who —


did **Ran an analysis** ≥ 5 times


in the last **14 days**


&


active **3 of the last 4 weeks**


= **8,412** users · 3.4% of active


Apply


##### The capability I lean on most is addictive. Pull every session matching a behavioral signal and drill in.


Early on, the request complexity classification surfaced a concern about how Lens handled ambiguous questions. I filtered to those sessions, found two areas that worried me, and we now track both programmatically. That whole loop, from hunch to confirmed pattern to standing metric, took an afternoon.


It also catches things that look like problems and are not. Around half of our session outcomes were labelled "Clarification Requested," which initially read as a defect. Drilling in showed the agent was ending answers with a follow-up question, a deliberate design choice. But when we looked at what users did next, the pattern was clear enough. They weren't going deeper, they were going elsewhere. We tweaked the prompt. Without the ability to interrogate the label, we might have spent a sprint fixing behavior that was working as intended.


## A scorecard instead of a vibe


We all know AI outputs are inherently non-deterministic, which is a long way of saying that viewing a handful of sessions and calling it quality assurance doesn't scale. We wanted a number.


Amplitude's standard signals gave us a floor on day one. Custom evals let us go further, scoring sessions against our own definition of good, including rubrics built from ground-truth Q&A pairs our analysts wrote.


By Amplitude's measure, Lens now holds a 96.9% task success rate, and weekly task failures fell 84% as we worked through the issues the data surfaced. Those gains came from ordinary engineering. The measurement is what tells us where to point it.


Evals alone tell you whether an answer was correct. Quality signals connected to real product usage tell you whether the product is getting better at its job.


## What comes next


Agent interactions in Amplitude arrive as decomposed events, the same shape as any other product event. That opens the door we care most about, connecting Lens quality to engagement across Viewpoint as a whole. Does a strong Lens experience deepen how teams use the platform? As we add structured data and chart visualisation to Lens, and as other parts of The Economist Group explore whether the architecture fits their own products, that connected view is how we will judge the work.


None of this is about watching individual users. It is about holding our AI products to the same standard of evidence we hold our analysis to. Our clients pay us for rigor. The tools we use to improve their experience should have some too.
