---
schema_version: "1.0.0"
document_id: "aaf4e5aadfc29a6ee7a3f358bc0b0275f3a0f6b5b6e4f9d4060a0e81180d1ecf"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/endurance-training-comparison"
published_at: "2026-04-23T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:a7f5bf79d99a811535bfe65903564a53345fd2a735d0040d5dd517ad295ab0d8"
---

# Running Is The Hardest Endurance Sport? Not so Fast | Terra

## Three intensity tiers — and the groupings aren't obvious


Median session average heart rate clusters into three distinct bands:


**Tier** **Modalities** **Median Avg HR**


High Running, Indoor Cycling, XC Skiing 131–145 bpm


Moderate Outdoor Cycling, Ski Touring, Open Water Swimming 119–129 bpm


Low Walking, Downhill Skiing, Hiking 98–102 bpm


The surprise is not that running tops the list. For me, it's the discrepancy between indoor cycling and outdoor cycling drops (a full 9 bpm). The turbo trainer eliminates coasting, traffic lights, descents, and for many people, the joy of riding for fun — the things that suppress average HR outdoors. I explored this indoor-outdoor divergence in detail in our previous post.


### Running's high intensity is genuine, not an artefact


A high average on its own doesn’t necessarily tell us very much. Maybe even one who runs has a very high Max HR, it’s a population of fitter people, not a harder exercise? So I calculate the intensity ratio, the session average as a fraction of the session peak. Running’s highest ratio of 87.5% could mean two things: the average is genuinely high, or the peak is suppressed. This matters because the metric alone can't tell you which.


The scatter plot resolves it. Running occupies the top-right quadrant: both the highest average HR (145 bpm) and the highest peak HR (166 bpm) of any modality. There is no ambiguity. Runners hold a high heart rate throughout and push to the highest peaks. The 87.5% ratio reflects genuine sustained output.


***Figure 2:** Average HR vs Peak HR Scatter. Running occupies the top-right quadrant: both the highest average and highest peak HR, confirming genuine sustained high output. Walking clusters bottom-left — its high ratio (86%) reflects a flat, low-amplitude session rather than true intensity.*


### Walking's 86% ratio is a statistical illusion


Walking ranks third in sustained intensity at 86.3%, just behind running and indoor cycling. On paper, that looks like genuine sustained effort. But the scatter tells the real story: walking clusters in the bottom-left, with both low average (98 bpm) and low peak (118 bpm). Its "high" ratio is simply the absence of intensity variation. A flat, low-amplitude session where heart rate never really goes anywhere produces a ratio that mimics hard exercise.


### Downhill skiing has the biggest within-session swings


At 68.3%, downhill skiing has the lowest sustained intensity ratio by far. The decomposition shows exactly why. Its median peak HR (149 bpm) rivals outdoor cycling, but its median average (102 bpm) is on par with walking and hiking. That 47 bpm gap is the heart rate signature of a stop-and-go sport: explosive effort on the piste, then lifts, queues, and standing in the cold. Or maybe it’s just blind terror, I get some pretty wild heart rates on gnarly descents.


***Figure 3:** Sustained Intensity Decomposition (3-panel bars). The ranking shifts between panels reveal the mechanism. XC skiing and outdoor cycling rank top-3 in peak HR but drop sharply in sustained intensity — high peaks with long low-HR recovery phases. Running leads in both average and ratio.*


### **XC skiing peaks like running but sustains like cycling**


Cross-country skiing yields the joint-highest median peak HR in the dataset, 163 bpm, virtually tied with running at 166 bpm. Yet its sustained intensity ratio is only 76.8%, seventh out of nine. The mechanism is clear from the decomposition: XC skiing involves explosive hill efforts that spike heart rate to near-maximum, separated by long gliding recovery phases that pull the average down.


This shows up strikingly in the three-panel decomposition, where the ranking reshuffles completely across metrics. XC Skiing jumps from 5th in average HR to 2nd in peak. Indoor Cycling leaps from 8th in peak to 2nd in sustained ratio. The ranking shifts reveal the underlying mechanism, whether a sport's character comes from high sustained effort, high peaks, or the gap between the two.


### Indoor cycling is the purest sustained effort after running


Indoor cycling's sustained intensity ratio (86.8%) sits just 0.7 percentage points behind running. That makes sense: both are continuous, self-paced activities with no coasting and no external interruptions. What separates them is the absolute level, indoor cycling's median average HR (134 bpm) is 11 bpm lower than running's. The cardiovascular demand of weight-bearing locomotion at a running pace simply exceeds that of seated cycling at a typical indoor effort, even when the relative intensity profiles are nearly identical.


### Within the same person, running still wins


Everything up to this point could be explained away by population differences; maybe runners are simply fitter, or more motivated, or younger. So I ran the analysis that controls for all of that: within-person comparison.


I identified 2,808 multi-sport users who logged at least five sessions in two or more modalities. For each person, we used their highest-ever recorded heart rate as their personal Max HR, then expressed each session's average HR as a percentage of that ceiling. The result is unambiguous. The same individuals who run at a median 78% of their personal Max HR cycle outdoors at just 63%, and walk at 51%. Among the 1,480 users who both run and cycle outdoors, 93% have a higher relative intensity when running, a median gap of 10 percentage points. It's not that runners are fitter than cyclists, it's that running, as a biomechanics act, demands more of the cardiovascular system than cycling does, even in the same body.


I also tested this using the 95th percentile of each user's peak HR rather than the absolute maximum, to exclude potential sensor artefacts. The ranking and gaps were identical.


***Figure 4:** Within-Person Relative Intensity Ridge. Each user's session average HR is expressed as a percentage of their highest ever recorded heart rate. Even controlling for individual fitness, running sits highest — 93% of runner-cyclists train harder when they run.*


## But running is mostly shorter — and that changes the picture


I can already hear people screaming: *But running sessions are shorter.* Fair point. The median run in this dataset lasts 41 minutes. The median outdoor cycle lasts 64. A hike: 115. Downhill skiing: 129. Running may be the most intense sport per minute, but is it the hardest session overall?


To answer that, we multiplied each session's relative intensity by its duration to create "intensity-minutes" — a single metric that captures total cardiovascular load. A 40-minute run at 78% of max yields 31 intensity minutes. A 64-minute cycle at 67% produces 42.


The rankings flip. Downhill skiing — the *least* intense per minute — tops the chart at 71 intensity-minutes because sessions last over two hours. Hiking (61) and XC skiing (52) also overtake running (31). Among the 1,480 runner-cyclists, only 39% accumulate more intensity minutes when running than when cycling.


***Figure 5:** Time-Weighted Intensity (3-panel bars). Running is the most intense sport per minute (B), but shorter sessions mean its total cardiovascular load per session is lower than outdoor cycling, XC skiing, or hiking (C). Intensity-minutes = relative intensity × duration.*


So which framing is right? That depends on how you think about intensity, and this is a problem we deal with every day at Terra when building physiological scores.


### Intensity isn't linear, and the weighting method changes the answer


Here's the issue with a straight duration × intensity calculation: it treats every beat per minute equally. But going from 110 to 120 bpm is not the same physiological event as going from 160 to 170. Above the lactate threshold, the metabolic cost of exercise rises exponentially, oxygen consumption drifts upward, lactate accumulates faster, and recovery cost per minute increases sharply. A linear model ignores all of this.


So I tested four weighting approaches:


- **Linear** — duration × relative intensity. The naive model.
- **Exponential** — duration × intensity². Squares the intensity term so high HR contributes disproportionately.
- **TRIMP** (Banister, 1991). The academic standard. Uses an exponential function calibrated against blood lactate data: load = duration × HRR × 0.64e^(1.92 × HRR). I estimated resting HR at 60 bpm when individual data weren't available.
- **Zone-weighted** - Assigns escalating multipliers by HR zone (1× below 60%, up to 5.5× above 90%). The consumer-platform approach.


The results are dramatic. Under the linear model, running ranks 7th, duration dominates, and cycling, hiking, and skiing all produce more total load. Apply the exponential weighting, and it climbs to the 6th. But under TRIMP, running jumps to 2nd, behind only XC skiing.


***Figure 6:** Weighting Method Bump Chart. How you weight intensity changes the answer. Under a linear model, running ranks 7th — duration dominates. Apply the exponential weighting derived from blood lactate data (TRIMP), and running climbs to 2nd, behind only XC skiing.*


In the paired runner-cyclist comparison, TRIMP is the only model where running edges ahead (53% of users, median delta +5). The linear model has cycling winning in 61% of users. The exponential narrows the gap to 56-44. TRIMP tips it. The reason is straightforward: running spends more time at high fractions of heart rate reserve, where the TRIMP exponential term amplifies the load. A runner at 78% of HRmax for 41 minutes generates more TRIMP than a cyclist at 63% for 64 minutes, because the exponential penalizes moderate intensity less than it rewards high intensity.


A caveat: TRIMP is designed to be calculated from the full heart rate time series by summing the exponential weighting at every second, not by applying it to a session average. We don't have second-by-second HR in this dataset, only session summaries. Averaging the heart rate before applying the exponential smooths out the spikes, which means I’m likely understating the load for variable sports like XC skiing and outdoor cycling (where those high-HR bursts would contribute disproportionately) and possibly overstating it for steady-state efforts like running. But in data science, you work with the information you have, and the direction of travel across four different models is consistent enough to draw conclusions.


This is exactly the kind of problem we think about at Terra when building training load scores. The model you choose isn't just a technical detail, it determines what the data tells you.


## Where our data falls short


We should be transparent about what this analysis cannot tell you. We have no demographic information — no age, sex, or fitness level — so we can't separate these effects from sport selection. We don't know training intent (easy run vs tempo session), which means our distributions blend structured training with casual activity. And the HR data comes from wrist-worn devices, which are less accurate than chest straps, particularly during activities with significant arm movement like swimming and skiing.


### References


1.⁠ ⁠Banister EW, Calvert TW. Planning for future performance: implications for long term training. Can J Appl Sport Sci. 1980;5(3):170–176. https://pubmed.ncbi.nlm.nih.gov/6778623/


2.⁠ ⁠Kreider RB, Cundiff DE, Hammett JB, Cortes CW, Williams KW, Hill DW, et al. Physiological responses of triathletes to maximal swimming, cycling and running. J Sports Sci. 1986;4(2):155–163. https://pubmed.ncbi.nlm.nih.gov/3821455/


3.⁠ ⁠Solli GS, Tønnessen E, Sandbakk Ø. Training load quantification in elite swimmers using a modified version of the training impulse method. Int J Sports Physiol Perform. 2017;12(3):373–381. https://pubmed.ncbi.nlm.nih.gov/24942164/


4.⁠ ⁠Wallace LK, Slattery KM, Impellizzeri FM, Coutts AJ. Establishing the criterion validity and reliability of common methods for quantifying training load. J Strength Cond Res. 2014;28(8):2330–2337. https://pubmed.ncbi.nlm.nih.gov/24662229/


5.⁠ ⁠La Gerche A, Heidbuchel H. Endurance sport and cardiovascular health: can too much exercise damage the heart? Heart. 2014;100(16):1227–1234. https://pmc.ncbi.nlm.nih.gov/articles/PMC4535361/
