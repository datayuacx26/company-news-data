---
schema_version: "1.0.0"
document_id: "560362aa01d385f2fe97e52d44019081424445b9a684808e640979e02673dd96"
company_key: "yc-cinder-2"
company: "Cinder"
source_id: "yc-cinder-2-news-import-d9673863c17b"
canonical_url: "https://cinder.ai/resources/blog/north-korean-applicant-fraud-event"
published_at: null
first_seen_at: "2026-08-09T20:41:29.044096+00:00"
fetched_at: "2026-08-09T20:41:30.024984+00:00"
content_hash: "sha256:b1af4a39d61970967063a8052c80919c90129bf6341ee1ba936cab8ec199665d"
---

# A Night with Cinder: Know Your Applicant

During NY Tech Week, Cinder gathered tech and recruiting leaders at its New York City headquarters for an honest conversation about a growing problem: North Korean IT workers flooding US hiring pipelines and the companies that don't know it's happening to them.


Co-founder and head of engineering Declan Cummings and founding recruiter Bridget Martin walked the room through what this threat actually looks like, how it's evolved, and what it takes to stop it. The short version: your ATS is not catching it, and the number of companies quietly dealing with this is larger than the public conversation suggests.


## The scale of the problem


North Korea runs a sophisticated, state-funded operation to place engineers at US companies. Workers are trained through a competitive national pipeline, deployed abroad, predominantly to northeast China, and assigned monthly quotas they're expected to meet through remote employment. To receive payment, they need a US address for their laptop, which means recruiting American facilitators to handle physical onboarding. “They often don't know that they're working for the North Korean government until the FBI shows up at their house,” Cummings said.


The operation isn't targeting specific industries. It targets remote engineering roles in the US, because US engineering salaries are the fastest path to hitting quota. If you're hiring engineers remotely, you're likely a target.


The volume surprised even the people closest to it. “I looked at the numbers a couple months ago,” Martin said. “Between 20 and 30 percent of incoming applications for our engineering jobs, I had marked as fraudulent.” Since building a dedicated screening process, Cinder has identified over 10,000 fraudulent applicants in its pipeline. Without a structured process to surface them, that kind of volume would bury any recruiting team.


## The best defense is curiosity, not compliance


The companies catching these applicants aren't the ones with the most elaborate checklists. They're the ones that notice something off and keep asking why, that pull the thread instead of making an excuse to move forward.


No screening tool catches everything, but that instinct, noticing something off and following it, is harder to defeat than any checklist. The specific tells evolve constantly. The pattern behind them doesn't.


Tactics have shifted significantly since Cinder first encountered this in 2023. AI is now standard at every step: voice modulation, appearance-changing technology, AI-generated resumes and cover letters. The visual and auditory signals that once made fraudulent candidates legible on a call have largely disappeared. The harder shift is intermediaries. Operators are increasingly recruiting real people with real US identities to interview and complete verification steps on their behalf. The person on the call may pass every check.


Background checks won't save you either. Fraudulent applicants have cleared drug tests and employment verification by paying intermediaries to handle them. In New York, you can't initiate a background check until after a conditional offer, which means someone can reach payroll before any verification clears. “They will already have made money,” Martin said. “If they can do that across multiple organizations, they're making $5,000, $10,000.”


The defense has to happen earlier, before the call, based on signals that are harder to fake than a face or a voice.


## How we use Cinder to stop it


Cinder built a Know Your Applicant process that uses Cinder itself to screen for fraudulent applicants, and it's working. Since launching, no fraudulent North Korean applicants have made it through the screen to the interview stage. Recruiter time spent on fraudulent application review dropped from 6.5 hours per week to around 5 minutes.


The process ingests applicant data directly from our ATS and runs each application through a set of checks: identity and liveness verification, VoIP and proxy detection, IP reputation scoring, LinkedIn verification, GitHub cross-referencing. Anything that trips a flag goes into a human review queue rather than triggering an automated rejection. A VPN or a VoIP number alone isn't disqualifying, and good candidates shouldn't get filtered out by a single signal. What matters is the pattern.


Indicators from confirmed fraudulent applicants feed back into the system, so the screening improves over time. When new infrastructure appears, it gets added to the list. The adversaries adapt, and the system adapts with them.


For a full breakdown of how it's built, read[Know Your (North Korean) Applicants](https://cinder.ai/resources/blog/north-korean-applicants-how-we-use-cinder-to-identify-fraudulent-candidates-in-our-hiring-pipeline) .


## What the signals actually look like


There's no single tell. There's a cluster of things that don't add up, and the mistake is explaining each one away individually instead of looking at what they add up to.


“When you're a recruiter, you have a huge workload, you want to fill jobs, the resume looks really strong,” Martin said. “Okay, there's no LinkedIn. Some people don't have LinkedIn. No, that's actually almost not true. Most people have LinkedIn. We just can't make excuses.”


In practice: a missing or thin LinkedIn profile, a VoIP phone number, a suspicious or malicious IP address, a GitHub handle that doesn't match the name on the resume, an email address that reads like it was auto-generated. None of those is definitive on its own. Together, they are.
