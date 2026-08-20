---
schema_version: "1.0.0"
document_id: "aeba6afc244bddb47042e033b1092408a57a0030dc817f53fc977b3dd8168aff"
company_key: "yc-prolific"
company: "Prolific"
source_id: "yc-prolific-news-import-3a1392a67315"
canonical_url: "https://www.prolific.com/resources/m-turk-s-closure-marks-the-end-of-an-era-but-a-bigger-data-quality-challenge-remains"
published_at: null
first_seen_at: "2026-07-31T19:13:13.781646+00:00"
fetched_at: "2026-07-31T19:13:15.127161+00:00"
content_hash: "sha256:cb812ac55f7f61b8d79dc009b558fe0ecb1a89d17df0ee3e7edb340712a557ca"
---

# MTurk's closure marks the end of an era. But a bigger data quality challenge remains.

As of July 30, Amazon Mechanical Turk is[closing its doors to new customers](https://www.prolific.com/prolific-vs-mturk) . As someone who used MTurk in the early years of my research, the news is bittersweet in many ways. It solved a genuine problem, providing access to participants at a scale and speed academia had never achieved before.


But to be brutally honest, I’m surprised it’s taken this long. In recent years, I’ve witnessed a pretty seismic drop in the quality of samples MTurk has offered. And while MTurk's closure to new customers is the dominant headline right now, it’s symptomatic of a wider issue of data integrity in online research that we need to talk about.


## MTurk's data quality has been in decline for a long time


At some point, MTurk’s design stopped keeping pace with what it was being asked to do. It was built as a labor marketplace first, and participant vetting was left almost entirely to individual researchers to sort out for themselves. As the same pool of workers was recruited into study after study and the incentives rewarded speed over attention, sample quality dropped.


This observation isn’t new. Margaret Webb and June Tangney[documented it clearly a while back](https://pubmed.ncbi.nlm.nih.gov/36343213/) . What's changed recently is the scale of the gap. A[2023 study](https://pubmed.ncbi.nlm.nih.gov/36917576/) measuring cost-per-quality-response across platforms found MTurk was already costing roughly double what better-vetted platforms did once quality was accounted for. By 2026, in[our own recently released paper](https://osf.io/preprints/psyarxiv/pvdjr_v2) , that gap had widened to 16x.


To make matters worse, nobody really knew or understood what was happening behind the scenes at MTurk. How workers were recruited, how responses were assessed, even how big the pool actually was - all of it was opaque. That made data integrity issues hard to diagnose in real time and even harder to fix. Researchers were left to infer quality problems from their own results rather than understand the mechanics that caused them.


The cumulative effect has been a steady erosion of trust. MTurk was once the platform of choice for academic researchers. But as quality declined, MTurk samples were no longer considered reliable enough for rigorous research, and academics increasingly moved away from the platform.


Given all this, MTurk’s closure doesn’t come as much of a surprise. However, the harsh reality is that these data integrity issues aren’t unique to MTurk. Poor data quality has been an open secret in parts of the online sampling world for a long time.


## The bigger story beyond MTurk's closure


For many researchers, bad data has often been treated as par for the course. You collect data knowing it isn’t perfect, clean it up as best you can, or, in some cases, end up having to discard it entirely. But the bar for data quality is rising across research fields, and if it continues to do so, we may well see other providers fall by the wayside if they can’t provide the reliable data researchers require.


Rigorous verification is key to meeting these standards. In an age where fraud (both human and AI-assisted) is a bigger concern than ever, the question of whether respondents are real, engaged people can no longer be an afterthought. We’ve seen what happens when dishonest participants and bots contaminate data; for example,[YouGov had to retract a well-cited, widely publicized study](https://yougov.com/en-gb/articles/54406-conclusions-of-investigation-into-2024-bible-society-study) in 2024 after discovering that a number of survey respondents were fraudulent.


Many research panels assume verification is the researcher’s responsibility, but panels can - and should - be doing more here. MTurk did little to no verification of its participant pool, and[it became progressively more polluted with bots as a result](https://todd.gureckislab.org/2021/06/09/the-poisoned-well) . Other samples haven't reached that level of pollution yet, but there's no guarantee they won't if vetting doesn't improve.


There's a broader ethical question to consider in all of this as well. On MTurk, the minimum fee per task is $0.01, meaning participants could be paid next to nothing to complete a survey. That creates an incentive structure in which participants try to complete as many tasks as quickly as possible to maximize their ROI. Low attention, low-quality responses are the inevitable outcome.


MTurk’s poor payment practices aren’t an isolated case. Other panels don’t have a minimum pay threshold or offer vouchers or gift cards instead of real cash, which leads to the same incentive issues that affect data quality. It might be time for the research industry to stop treating fair compensation as a nice-to-have and embrace it as a core factor in ensuring data integrity.


## How should online panels handle data integrity challenges?


If there's a lesson to be learned from this story, it's that online panels need to do more to own data integrity. And it takes more than a single tool or solution. A multi-layer system is needed to verify participants effectively, combat fraud, and keep on top of AI threats.


Here are some of the most important measures they can take to avoid the quality issues that led to MTurk’s decline.


#### Retain control over the pool itself


Platforms need to invest in the technology and processes to keep bad actors and bots from ever entering the sample in the first place, and use data to decide who gets in and out. Contaminated data shouldn’t be left for researchers to filter out after the fact.


Quality over quantity is important here. Some panels may try to impress with the sheer size and scale of their participant numbers. But a more selective approach ensures that only thoroughly vetted and highly engaged participants can join the pool and take studies, which results in less bad data to remove down the line. Prolific, for instance, admits only 13% of people from a waitlist based on demand. Of those people, just 55% pass onboarding and are verified to take studies.


#### Take responsibility for verification


It should be the panel's job to confirm a respondent is a real, unique human being, which is a distinct question from whether that person is a quality respondent.


There are several checks that panels can put in place to ensure participants are real and engaged, including ID verification, IP address validation, and response quality assessments. Checks shouldn’t be a one-and-done either. Ongoing, panel-wide fraud detection and ID reconfirmation checks are important for maintaining quality over time.


#### Get ahead of the AI threat


We know some people are already using LLMs to help answer open-text survey questions. And while fears of AI agent prevalence in online data providers[are currently overblown](https://osf.io/preprints/psyarxiv/pvdjr_v1) , platforms should be building defenses now, not waiting until it becomes a live problem.


This means[actively assessing for AI-like behaviors and responses](https://www.prolific.com/resources/how-prolific-detects-bots-and-ai-in-online-research) in studies. Verification checks can catch simple bots, but panels need to implement more advanced measures, such as live video selfies and in-app monitoring of automated patterns, to stay ahead of this evolving threat.


[Our own research](https://www.prolific.com/resources/authenticity-checks-how-we-tested-the-most-accurate-method-for-identifying-agentic-ai) has proven these measures work. When we tested Prolific’s bot authenticity checks, they achieved 100% accuracy, and our LLM authenticity checks worked on free-text questions with 98.7% accuracy.


#### Treat participants ethically


Happy participants make for happy researchers. Fair reward structures and policies will cultivate an ecosystem of motivated, engaged, and loyal participants who will produce more thoughtful responses and are more likely to stick around for multi-wave or longitudinal studies.


If you’re a researcher looking for an alternative to MTurk or thinking about switching to a different provider, there are a few key things to keep in mind when evaluating alternatives.


First, take some time to read independent, peer-reviewed research on these providers. There's a[growing body of published work](https://www.prolific.com/compare-prolifics-data-quality) in this space, and third-party papers are more trustworthy than marketing claims. Second, test it yourself. It doesn't take much to build a short survey, run a basic data-quality comparison across the platforms you're considering, and see how they compare.


## The bar for data quality has moved on


We shouldn't assume that what happened to MTurk will be a one-off. Any panel that doesn't keep up with what today's data quality demands - supplying real, engaged, trustworthy humans, verified as such - risks going the same way.


Platforms need to take genuine ownership of quality, rather than leaving it as one more thing for researchers to clean up after the fact. That's been true for over a decade. MTurk's closure just made it impossible to ignore.
