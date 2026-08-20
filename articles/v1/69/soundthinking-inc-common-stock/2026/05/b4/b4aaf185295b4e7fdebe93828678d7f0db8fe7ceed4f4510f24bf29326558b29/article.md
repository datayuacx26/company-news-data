---
schema_version: "1.0.0"
document_id: "b4aaf185295b4e7fdebe93828678d7f0db8fe7ceed4f4510f24bf29326558b29"
company_key: "soundthinking-inc-common-stock"
company: "SoundThinking Inc."
source_id: "soundthinking-inc-common-stock-news-import-78496ddc2424"
canonical_url: "https://www.soundthinking.com/blog/evaluating-gunshot-detection-technology-impact-requires-the-right-measures/"
published_at: "2026-05-29T04:32:39+00:00"
first_seen_at: "2026-07-22T14:19:40.145434+00:00"
fetched_at: "2026-07-28T22:07:11.290939+00:00"
content_hash: "sha256:e866f0cba070c59ec8449a8acf983cbb083b4f043c08dda0288f5ef36463cfc0"
---

# Evaluating Gunshot Detection Technology Impact Requires the Right Measures

On May 26, 2026, sociology professor Rob Vargas of the UChicago Justice Project published a “[Chicago After ShotSpotter](https://uchicago-justice-project.github.io/chicago_crime_map/) ” crime map. There are many substantive issues with this analysis and its questionable conclusions.


While some have characterized this as an “academic study,” it is notable that the Chicago After ShotSpotter crime map has:


-


No peer review;


-


No methodology document (the file is literally blank);


-


No statistics;


-


Headline numbers that don’t replicate from its own data, and


-


No accounting for seasonality, which greatly affects overall crime rates and related police response.


Let’s examine a few of the most prominent claims and other methodological flaws.


The main headline, “[CPD Officers Responded Faster to 911 Calls on South, West Sides After ShotSpotter Was Removed: UChicago Analysis](https://news.wttw.com/2026/05/27/cpd-officers-responded-faster-911-calls-south-west-sides-after-shotspotter-was-removed) ” does not hold up to even the most basic scrutiny. The claim is that “Chicago police responded 4 minutes faster to the most serious 911 calls for help” after removal of the ShotSpotter gunshot detection system.


The 4.2-minute figure is real in the dataset, but the analysis behind it has four structural problems that, taken together, mean it does not support the causal claim the site attaches to it.


1. 1.


Gunshot Calls – the calls most affected by policy – are excluded altogether.


2. 2.


The pre/post windows compare summer to winter, with no seasonal adjustment – crime is higher overall in the summer months and lower in the winter.


3. 3.


The full 4.2 minutes includes a ~2-minute citywide improvement in untreated beats.


4. 4.


Invalid control – The crime call profiles and gunfire incident rates in the South and West Sides greatly exceed those in Lincoln Park. Those beats are not comparable.


In other words, strip those four choices out and there is no policy effect to attribute. (More detailed analysis in Exhibits A and C.)


In fact, Chicago’s own Police Department[data show](https://www.soundthinking.com/blog/api/media/file/22179-SoundThinking-SST-stats-response-CPD.pdf) (page 6) that from 2019 through 2024, CPD’s response time for incidents of gun violence was ~2.4 minutes faster for events that were initiated exclusively by ShotSpotter vs. events that were initiated exclusively by a call to 911.


Further, six independent studies reach the same conclusion as CPD’s own data:


1. 1.


Piza (Northeastern, peer reviewed): ShotSpotter alerts beat 911 calls by 93 seconds


2. 2.


Cook (Duke): The pilot area’s median response time improved by about 1.2 minutes more than the rest of the city after ShotSpotter went live in Durham.


3. 3.


Mares (SIUE): 1.5 min vs. 2.6 min for 911 (42% faster) in Winston-Salem


4. 4.


Lawrence (CNA, peer reviewed): 14.4% faster than 911 calls for a shooting and 25.7% faster for shots fired in Denver; in Richmond, CA, officers responded about 15% faster than 911 calls — a 30-second to 3-minute difference.


5. 5.


Goldenberg (Cooper Hospital, peer-reviewed clinical journal): 3.7 min vs. 5.4 min for 911 (31% faster) in Camden, NJ.


6. 6.


Pittsburgh City Controller (Democratic, elected): 5 min faster than 911 alone.


A summary of these studies can be found in Exhibit B.


I would also add two additional things to consider for context when evaluating the impact of gunshot detection technology. First, independent research from the Brookings Institution shows that only about 12% of gunfire incidents in American cities result in a 911 call, meaning the vast majority of gunfire goes unreported through 911. There are many reasons for this, but the fact remains that most gunfire incidents are neither known nor responded to, with unfortunate and predictable outcomes. Second, an independent evaluation by the[University of Chicago Crime Lab](https://crimelab.uchicago.edu/2024/09/researchers-shotspotter-helps-gunshot-victims-receive-rapid-first-aid-does-that-change-the-debate/) estimated that ShotSpotter directly contributes to approximately 85 gunshot victims being saved per year in Chicago, primarily by enabling first responders to reach gun violence victims faster.


**Conclusions**


This is not a rigorous causal evaluation. It is a public-facing map and descriptive before/after comparison being framed far beyond what the data and methodology support. The study does not actually measure ShotSpotter’s core purpose – to detect gunfire when people do not call 911 and get police and first responders to the scene faster.


Chicago deserves honest, sober assessments and analysis of gunshot detection and all public safety technologies. This piece does not meet that bar, and the policy questions it is being used to answer are too important to be decided on evidence this thin.


---


**EXHIBIT A: THE “4.2-MINUTE” RESPONSE TIME CLAIM**


*A methodological review of **Chicago After ShotSpotter** (UChicago Justice Project, May 2026)*


**THE CLAIM** . Beats that formerly had ShotSpotter sensors saw an average Priority 1 emergency response time improvement of 4.2 minutes after ShotSpotter was turned off in September 2024 — “roughly twice the improvement seen in beats without ShotSpotter.” The site argues this is evidence that ShotSpotter alerts were tying officers up and that removing the system freed them to respond to genuine emergencies.


**THE SHORT ANSWER** . The 4.2-minute figure is real in the dataset, but the analysis behind it has three structural problems that, taken together, mean it does not support the causal claim the site attaches to it.


1. 1.


**GUNSHOT CALLS — THE CALLS MOST AFFECTED BY THE POLICY — ARE EXCLUDED.**


The analysis is restricted to Priority 1 calls excluding gunshots. The stated reason: ShotSpotter-initiated calls can be reclassified afterward. That is a real data-quality concern, but the response is the wrong one. The mechanism the site proposes — “officers freed from ShotSpotter alerts can now reach other calls faster” — depends on knowing what happened to the displaced calls. Removing the displaced category from the analysis severs the causal chain.


The correct response to a reclassification concern is sensitivity analysis: run it with and without, report both. The reader sees only the version that excludes the call type most directly affected by the policy.


1. 1.


**THE PRE/POST WINDOWS COMPARE SUMMER TO WINTER** .


The site compares the six months before the September 2024 shutdown (roughly March–August 2024) to the six months after (roughly October 2024–March 2025). “Before” is summer; “after” is winter. The site does not deseasonalize.


The repository’s own monthly data shows that the seasonal pattern, not the policy change, is driving the headline number. The treated-minus-control differential in average response time, by month:


Mar Apr May Jun Jul Aug Sep* Oct Nov Dec Jan Feb


−0.6 +1.3 +1.8 +1.8 −0.1 0.0 −0.2 −0.7 −0.5 −2.5 −2.4 −2.2


*September 2024: the shutdown month.


ShotSpotter beats were slower than control beats during summer 2024 and faster in winter 2024–25. The flip is a seasonal pattern that has nothing to do with the policy. The shutdown month itself (September: −0.2) is essentially zero; the large negative differentials do not appear until December, three months later, coinciding with Chicago’s seasonal low. A correct analysis would use year-over-year or interrupted-time-series methods. This one does neither.


1. 1.


**THE FULL 4.2 MINUTES INCLUDES A CITYWIDE IMPROVEMENT IN UNTREATED BEATS** .


In the published data, response times improved 4.22 minutes in former-ShotSpotter beats and 2.04 minutes in non-ShotSpotter beats. The honest framing: response times improved 2.18 minutes more in former-ShotSpotter beats than in controls. The site headlines the full 4.2 figure, then mentions almost in passing that this is “roughly twice” the improvement elsewhere. The reader is led to attribute the entire 4.2 minutes to ShotSpotter’s removal when slightly less than half of it is a citywide trend happening in beats the policy did not touch.


**BOTTOM LINE.**


Response times improved citywide in late 2024 and early 2025, in beats with and without ShotSpotter coverage, driven in significant part by seasonal patterns. The differential between treated and control beats — once the citywide trend is netted out — is roughly 2 minutes, not 4.2, and the analysis excludes the gunshot calls that are the only direct test of the proposed mechanism. There is no statistical inference, no methodology document, no robustness check.


The 4.2-minute figure is being cited as evidence that ShotSpotter slowed police response to emergencies. It is not. It is evidence that response times in a particular set of beats improved over a particular twelve-month window for reasons the analysis does not isolate.


*All figures replicated from the project’s public repository (beats_with_stats_v2.geojson, monthly_response_time.csv) at github.com/uchicago-justice-project/chicago_crime_map. Full memo with category-overlap analysis and replication of the violent crime / homicide claims available on request.*


---


**EXHIBIT B: EVIDENCE REFERENCE: SHOTSPOTTER RESPONSE TIME**


*Six core studies on response time plus two foundational context studies | Prepared May 27, 2026 | For internal and PR agency use.*


**Purpose.** This document is the evidence stack for ShotSpotter’s response time benefits and the underlying public safety case the technology rests on. The six core studies — peer-reviewed academic research, federally-funded multi-city evaluations, a peer-reviewed clinical journal, and an elected municipal auditor — establish the response time finding across seven cities. Two foundational context studies (Brookings on the 911 silence gap and the University of Chicago Crime Lab on lives saved) frame why that response time finding matters operationally and in human terms. Each entry below includes the specific finding, the source citation, and an honest “use with care” note. The integrity notes are deliberate: every entry here can withstand pushback because every entry acknowledges what the source does and does not establish.


**AT A GLANCE**


Source City / Cities Response time finding Method Vintage


Piza et al. Chicago, Kansas City 93 sec faster than 911; police arrive closer to incident GPS data, matched quasi-experiment, NIJ-funded 2023–24, peer reviewed


Mares & Buettner Winston-Salem 1.5 min vs. 2.6 min for 911 (42% faster) Pre/post + comparison area; cost-benefit 2023–24


Cook & Soliman Durham, NC 1.2 min faster median response 12-month pilot, full operational data Feb 2024


Lawrence et al. (CNA) Denver, Milwaukee, Richmond CA 14–25% faster Denver; 15% (30s–3min) Richmond; mixed Milwaukee NIJ-funded multi-city; CAD timestamps 2024, peer reviewed


Goldenberg et al. Camden, NJ 3.7 min vs. 5.4 min for 911 (31% faster) 9 years of trauma center data, clinical 2019, peer reviewed


Pittsburgh Controller Pittsburgh ~5 min faster than 911 Municipal audit, full operational data Aug 2025


1. 1.


**Piza et al. — Chicago and Kansas City**


**Finding.** In Kansas City, ShotSpotter alerts went off **93 seconds before the first 911 call** reporting the same incident on average — a time savings that shaved off nearly 12% of overall police response. In Chicago, police officers stopped their patrol cars more often and **closer to the actual location of reported gunfire** when responding to ShotSpotter alerts than 911 calls, as measured by GPS coordinates of patrol vehicles.


**Source.** Eric L. Piza, Professor of Criminology and Criminal Justice, Northeastern University. National Institute of Justice grant #2019-R2-CX-0004. Five peer-reviewed articles 2023–2024 in *Journal of Quantitative Criminology* , *Journal of Experimental Criminology* , and *Criminology & Public Policy* . Direct quote, *City Journal* October 2025: “Cops tend to get out there quicker on ShotSpotter calls. ShotSpotter calls come in sooner than 911 calls about gunfire. Police collect more evidence when they’re on ShotSpotter scenes.”


1. 1.


**Mares & Buettner — Winston-Salem, NC**


**Finding.** The cleanest like-for-like response time comparison in the published literature. Officers were dispatched in **1.5 minutes for ShotSpotter alerts vs. 2.6 minutes for citizen reports** (a 42% improvement), and completed investigations in under 11 minutes per ShotSpotter incident vs. roughly 30 minutes per citizen report. Beyond response time, the ShotSpotter coverage area saw a **24% reduction in aggravated assaults and homicides** (approximately 87 fewer aggravated assaults), with estimated annual social cost savings of $5–8 million against an annual implementation cost of $230,000–350,000 — a $15–25 return per dollar spent.


**Source.** Dennis Mares and Andrew Buettner, Center for Crime Science and Violence Prevention, Southern Illinois University Edwardsville. *Improving the Police Response to Gunfire: A Cost-Benefit Analysis of ShotSpotter in Winston-Salem* , 2023, updated 2024. Data covers August 2021 through January 2024.


1. 1.


**Cook & Soliman — Durham, NC**


**Finding.** ShotSpotter more than doubled gunshot notifications in the Durham pilot area and produced a **1.2-minute reduction in median police response time.** Cook described the improvement as “a little over a minute out of around five minutes,” adding that the number of cases with slow response times fell substantially.


**Source.** Philip J. Cook, ITT/Terry Sanford Professor Emeritus of Public Policy and Economics, Duke University; Adam Soliman, Assistant Professor of Economics, Clemson University. *Evaluation of Durham’s ShotSpotter Installation: Results of a 12-Month Pilot Project* . Wilson Center for Science and Justice at Duke Law, February 15, 2024. Cook publicly described the pilot as “incredibly well managed” and described Durham as “a model for how it should be done.”


1. 1.


**Lawrence et al. (CNA) — Denver, Milwaukee, Richmond CA**


**Finding.** In Denver, call-to-arrival response times to GDT alerts were **14.4% faster than 911 calls for a shooting and 25.7% faster for shots fired.** In Richmond, California, officers responded to gunshot detection alerts **about 15% faster than 911 calls — a 30-second to 3-minute difference.** Milwaukee showed mixed results, which the authors attribute to implementation differences across departments.


**Source.** Daniel S. Lawrence and Kenneth J. Novak, Center for Justice Research and Innovation, CNA Corporation. National Institute of Justice funded. *Firearm shootings and the police response: examining the impact of gunshot detection technology. Police Practice and Research* , Vol. 26, No. 6, 2024.


1. 1.


**Goldenberg et al. — Camden, NJ (peer-reviewed clinical journal)**


**Finding.** From a peer-reviewed clinical study published in the *Journal of Trauma and Acute Care Surgery* : without ShotSpotter, police took an average of **5.4 minutes to respond compared with only 3.7 minutes when ShotSpotter was activated** (a 31% reduction in pre-hospital response time for gunshot wound patients).


**Source.** Goldenberg, A., D. Rattigan, M. Dalton, J. P. Gaughan, J. S. Thomson, K. Remick, C. Butts, and J. P. Hazelton. 2019. “Use of ShotSpotter Detection Technology Decreases Prehospital Time for Patients Sustaining Gunshot Wounds.” *Journal of Trauma and Acute Care Surgery* 87(6): 1253–1259. Cooper University Hospital trauma research team analyzing nine years of gun violence data from Camden, NJ.


1. 1.


**Pittsburgh Office of City Controller (Heisler)**


**Finding.** ShotSpotter “hasn’t significantly reduced crime in the past few years, but has decreased police response time compared to 911 reports.” Pittsburgh Councilwoman Barbara Warwick, citing the audit during a February 2026 council meeting, noted that **response times for the thousands of ShotSpotter alerts in 2025 were five minutes faster** than 911 calls. The report also found 911 calls have dropped 50% since the program was first implemented, meaning ShotSpotter is the only notification mechanism for half the gunfire incidents that used to generate 911 calls.


**Source.** Office of City Controller Rachael Heisler, City of Pittsburgh. *Department of Public Safety ShotSpotter System Special Report* , August 2025. Coverage by Pittsburgh Post-Gazette, WPXI, TribLive, and Government Technology.


**CONTEXT STUDIES — WHY THE RESPONSE TIME FINDING MATTERS**


The six studies above establish *that* ShotSpotter delivers faster police response to gunfire. The two studies below establish *why that matters* — first, because the vast majority of gunfire in America goes entirely unreported through 911 (Brookings); and second, because faster response on gunfire incidents demonstrably saves lives (University of Chicago Crime Lab). Together, these two findings convert the response time evidence from an operational metric into a public safety argument with a body count attached.


1. 1.


**Carr & Doleac (Brookings Institution) — Washington, DC and Oakland, CA**


**Finding.** Only **12% of gunfire incidents result in a 911 call to report gunshots** in Washington, DC and Oakland, CA. Only 2–7% of gunfire incidents result in a reported assault with a dangerous weapon. Reporting rates vary substantially within cities — in DC’s Police District 3 (Adams Morgan, Shaw, Columbia Heights), just 9% of gunfire incidents prompted a 911 call about hearing gunshots. Only 0.5% of gunfire incidents in DC resulted in a homicide. The implication: traditional measures of gun violence — 911 calls, reported crime, homicide data — capture only a small fraction of the actual gunfire occurring in American cities.


**Source.** Jillian B. Carr and Jennifer L. Doleac. “The geography, incidence, and underreporting of gun violence: New evidence using ShotSpotter data.” Brookings Institution working paper and Up Front blog publication, 2016. Data covers Washington, DC (January 2011–June 2013) and Oakland, CA (January 2008–October 2013). Doleac was at the time Assistant Professor of Public Policy and Economics at the University of Virginia Batten School.


1. 1.


**University of Chicago Crime Lab — “ShotSpotter saves ~85 lives per year in Chicago”**


**Finding.** Using a regression discontinuity design comparing shooting outcomes at the boundaries of adjoining Chicago police districts (those with ShotSpotter vs. those without), the Crime Lab found that after ShotSpotter went live, **fatality rates were about 4 percentage points lower** in areas with the technology — against an overall fatality rate of 17%, a roughly one-quarter reduction in the odds a gunshot victim dies. Applied to the 2,124 shootings in ShotSpotter areas in 2023, the analysis estimated a roughly 3-in-4 chance that the technology **saves about 85 lives per year** primarily by enabling first responders to provide lifesaving treatment to gun violence victims faster.


**Source.** University of Chicago Crime Lab researchers, published as an op-ed in the *Chicago Tribune* in September 2024 and subsequently endorsed in the Tribune editorial board’s “Chicago must keep ShotSpotter” editorial (September 14, 2024). The University of Chicago Crime Lab is housed within the Harris School of Public Policy and is one of the most-cited urban violence research centers in the United States.


---


**EXHIBIT C: THE UCHICAGO JUSTICE CENTER RESPONSE TIME STUDY**


*Four fatal flaws — and they are choices the authors made.*


-


**They compared summer to winter and called it a policy effect.** “Before” was March through August 2024. “After” was October 2024 through March 2025. Their own monthly data shows ShotSpotter beats were slower than control beats throughout the summer and faster throughout the winter — the seasonal flip Chicago sees every year. The shutdown month itself shows essentially zero effect. They had September 2023 through February 2024 available to compare against the post-shutdown winter. They chose not to. The honest year-over-year comparison would have shown a near-zero effect.


-


**They excluded gunshot calls — the calls ShotSpotter was built to detect.** They argued the data is noisy because of reclassification. Fine: run the analysis with and without, show both numbers. That is how researchers handle measurement error. Instead they ran it one way. The way they ran it excluded the calls where the policy difference should be largest. They picked the comparison most likely to make ShotSpotter look unnecessary, then drew a conclusion about whether ShotSpotter was necessary.


-


**They erased the gunshot victims no one called 911 for.** Fewer than 20% of gunshot incidents in Chicago generate a 911 call. For the other 80%, ShotSpotter was the only notification — the only reason officers and paramedics arrived. By excluding gunshot calls from their analysis, UChicago did not just exclude a noisy data category. They excluded the entire population of shooting victims whose response time was, before the shutdown, *zero minutes from the trigger pull to the sensor alert* — and is now, in many cases, infinite, because no one called. A paper claiming to measure “what happened to response times after ShotSpotter” that excludes the victims ShotSpotter was finding is not measuring response times. It is measuring something else and calling it response times.


-


**Their control group is not a control group.** They compare former-ShotSpotter beats to non-ShotSpotter beats. Those are not similar areas. Former-ShotSpotter beats are the highest-crime, highest-shooting beats in the city — they had ShotSpotter precisely because they had the gunfire problem. Non-ShotSpotter beats are largely lower-crime areas of the North and Northwest Sides with fundamentally different call profiles, response geographies, and baseline times. The right comparison would have been to similar high-crime beats in cities that kept ShotSpotter, or to ShotSpotter beats against themselves year-over-year. UChicago did neither. They compared the South and West Sides to Lincoln Park and called the difference a policy effect.


**This is not research. It is advocacy in academic packaging.** It uses university affiliation and a GitHub link to import credibility it has not earned. There is no methodology document — the file is literally blank. There is no peer review. There is no statistical inference. There are no controls for baseline crime, demographics, geography, or seasonality. Every analytical choice that could have been made two ways was made the way that produces the desired headline. By the standards any working criminologist would apply to a doctoral student’s first draft, this borders on academic malpractice. Before Chicago makes a public safety decision based on it, the authors should be asked, on the record, to show their work — and to explain why they did not.


*All figures replicated from the project’s public repository at github.com/uchicago-justice-project/chicago_crime_map.*
