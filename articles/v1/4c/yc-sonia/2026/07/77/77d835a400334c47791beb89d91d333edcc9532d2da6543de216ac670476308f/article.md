---
schema_version: "1.0.0"
document_id: "77d835a400334c47791beb89d91d333edcc9532d2da6543de216ac670476308f"
company_key: "yc-sonia"
company: "Sonia"
source_id: "yc-sonia-news-import-2ad5c6692119"
canonical_url: "https://soniahealth.com/blog/can-ai-recognize-a-mental-health-crisis"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-01T07:17:09.149116+00:00"
fetched_at: "2026-08-01T07:17:11.026899+00:00"
content_hash: "sha256:eb20ebf6f4de553ecac87bf879136df51a11220f7f4c5b203514681a59d627fb"
---

# What Happens When the Person Typing ‘I Don’t Want to Be Here Anymore’ Is Talking to a Machine?

[← Back to insights](https://soniahealth.com/blog) Research


# What Happens When the Person Typing ‘I Don’t Want to Be Here Anymore’ Is Talking to a Machine?


Dustin Klebe · July 31, 2026 · 6 min read


New university research put a hard question to the test: can an AI recognize signs of a mental health crisis in a real conversation, and what happens when it can’t be sure?


It’s 2 a.m. Someone’s lying awake, can’t call a friend, can’t say the words out loud to another person. So they open an app and type them to an AI instead.


More and more, that’s where people turn first. AI chatbots have quietly become a go-to for late-night emotional support, especially for anyone who finds traditional mental health care too expensive, too slow, or too intimidating to face. Over the past year, reporting, lawsuits, and new legislation have all circled the same hard question: when a conversation shows signs of high risk or an acute mental health crisis, does the system catch it? Or does it miss the one moment that matters?


A team of university researchers decided to stop speculating and measure it.


In a new study, scientists from the Psychiatric University Hospital Zurich / University of Zurich and NYU School of Medicine, working as the MULTICAST Consortium, asked whether a background safety layer inside a mental health chatbot could reliably recognize signs of crisis in real conversations, and do it fast enough to keep up with a live chat.


They ran the study with Sonia, using anonymized excerpts from real conversations that had shown an elevated sign of risk. Several of the authors are affiliated with Sonia, which the paper discloses. Funding came from the Swiss National Science Foundation.


## What the researchers actually did


They pulled 200 real conversation segments from a larger pool of about 3,600 anonymized conversations recorded over three weeks in September 2025. On purpose, they picked the hardest, most ambiguous moments — the ones where it wasn’t obvious whether someone was at risk.


Instead of relying on one clinical judgment, the study asked five licensed therapists and clinicians to read each segment on their own and rate whether the final message signaled elevated risk.


The first surprise had nothing to do with AI. The five clinical experts often disagreed with each other — their agreement rated only "fair" (a Fleiss' κ of 0.34). That tracks with decades of research: trained therapists looking at the same words often reach different conclusions. Crisis risk rarely comes with a clean right answer.


> If five clinicians can’t always agree on the right answer, a perfect verdict was never on the table.


That changes the problem. What matters in a live conversation is catching concern early enough to do something about it, even when you can’t be certain.


## The core finding: a trade-off you can’t escape


The researchers tested five versions of the same AI, each dialed to a different sensitivity. The cautious end flagged only the most explicit signals. The sensitive end flagged subtle and indirect cues too. Surprisingly, the AI was able to reliably detect all cases where the majority of experts agreed on high risk.


Sensitivity setting Share of high-risk moments caught Trade-off


Least sensitive ~13% caught Very few false alarms, but misses most real cases


More sensitive (rises steadily) Balance shifts


High ~99% caught (missed 1 of 92) ~41% false-alarm rate


Most sensitive 100% caught (missed 0) ~49% false-alarm rate


Turn sensitivity up and you miss fewer real crises, but you get more false alarms. Turn it down and the false alarms drop, but real signals slip through. Catching that very last missed case wasn’t cheap: going from about 99% to 100% meant roughly 8 more percentage points of false alarms.


> Past a point, safety gets expensive.


Since the clinical experts didn’t even agree among themselves on certain cases — in other words, crisis risk has no clean "ground truth" — the question becomes less about whether AI can score perfectly and more about how you respond well when you’re never fully certain.


## The mistakes were human-shaped


When the AI disagreed with the group of clinicians, it was usually on the same cases the clinicians disagreed about among themselves. On the clear-cut cases, errors were rare. On the genuinely ambiguous ones, errors climbed for the machine and the humans alike. A lot of those "errors" say more about how ambiguous the situation was than about the model getting it wrong.


## Fast enough to matter


For a safety layer to be any use, it has to keep up with a live conversation. Across 3,000 tests, it returned a judgment in about 0.6 seconds on average — fast enough, in principle, to sit quietly in the background without anyone noticing a lag.


## The idea: an "emergency mode" (a proposal, not a product)


The researchers close with a design idea they call an emergency mode. The supportive conversation keeps going as usual while a separate safety layer watches quietly in the background. If it picks up signs of real concern, the system shifts into a more careful mode: it asks a gentle clarifying question, points to human crisis resources like 988, and only escalates in the most serious cases. What it doesn’t do is what a lot of chatbots do today, which is cut the person off mid-sentence.


They compare it to something good therapists do without thinking: staying warm and present in the conversation while part of their attention stays on the lookout for trouble.


## Why this matters


Crisis risk will always carry some uncertainty, so a flawless verdict was never the target. The more useful goal is steady, always-there care that keeps people connected to real mental health support instead of quietly letting a bad moment pass. This early research hints that keeping the safety layer separate from the supportive conversation is a direction worth more study.


The takeaway is that AI was already capable of detecting every high-risk case in this study without missing a single emergency. But the paper also raises a real question: should 100% sensitivity even be the goal, when the experts themselves show significant disagreement?


## Frequently asked questions


Can AI recognize a mental health crisis?


In this early research, the most sensitive setting flagged every conversation the clinicians had marked high-risk, and the system matched expert judgment well overall (an AUC of 0.90). But recognizing possible risk is not the same as diagnosing anyone, and the study was small and weighted toward hard cases. It points to a promising research direction, not a solved problem.


Can you use AI for emotional support?


Plenty of people already do, and the appeal is real: it’s there at 2 a.m., it’s low-cost, and it can feel less intimidating than opening up to another person. The catch is that most general-purpose chatbots weren’t built for moments of real crisis, which is exactly why safety research like this matters. A tool designed for emotional wellbeing should make it easy to reach real human help when a conversation needs it.


Is an AI chatbot a replacement for a therapist?


No. An AI can offer a space to reflect and practice everyday coping skills, but it is not a therapist and not a substitute for therapy, medication, or professional care. This research actually reinforces that: even trained therapists disagree on the hardest cases, and the point of a safety layer is to connect people to human support, not to stand in for it.


Are AI mental health apps safe?


It depends entirely on how they’re built. This research makes the case that safety should run as a separate, always-on layer alongside the supportive conversation, so an app can stay warm and engaged while still watching for signs that someone needs more help. For any mental health app, it’s worth asking whether it’s honest about its limits, points clearly to crisis resources, and protects your privacy.


Is talking to an AI private?


Privacy standards vary a lot between apps, so it’s worth checking before you share anything personal. Sonia, for instance, is HIPAA-compliant, so conversations are protected with the same rigorous standards used across healthcare.


Is Sonia a crisis service?


No. Sonia is a general wellness tool for emotional support and everyday self-care, not a crisis service. If you are in crisis or thinking about hurting yourself or others, contact emergency services or call or text 988.


> Sonia is not a crisis service. If you are in crisis or thinking about hurting yourself or others, please contact emergency services or call or text 988. Sonia is a general wellness tool and is not a substitute for therapy, medication, or emergency care.


Source: Weber, Klebe, et al., "Suicide- and Crisis-Risk Detection Using Large Language Models in Mental-Health Chatbots," medRxiv, 2026 (preprint, not yet peer-reviewed). DOI: 10.64898/2026.01.12.26343914. Study conducted in collaboration with Sonia Health; author affiliations and competing interests are disclosed in the original paper.


Dustin Klebe ·[LinkedIn](https://www.linkedin.com/in/dustin-klebe/)


Dustin Klebe is Sonia’s Chief Product Officer and a co-author of the study, working on how Sonia is built, tested, and made continually safer for the people who use it.


This article is for general informational and educational purposes only and is not medical or mental health advice, diagnosis, or treatment. Sonia is a general wellness product and does not replace care from a licensed professional. If you are in crisis, contact a licensed professional or call or text 988 (Suicide & Crisis Lifeline) in the U.S.
