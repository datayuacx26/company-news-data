---
schema_version: "1.0.0"
document_id: "66ab95a78b2b43e66e3663266105b7bdbaadd9a4739e8962c39f99195dba0d13"
company_key: "comscore-inc-common-stock"
company: "comScore Inc."
source_id: "comscore-inc-common-stock-news-import-2115fba46c13"
canonical_url: "https://www.comscore.com/Insights/Blog/Comscore-TV-Personification"
published_at: null
first_seen_at: "2026-07-25T00:27:51.627650+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:cdca55eab9bf6cd8a4a27d41ad901bdb735dee528468dd7193877e84dba65065"
---

# Comscore TV Personification

**Introduction: the problem statement**


In media measurement, there are several distinct uses of the term “personification”. Generally, it refers to the process of identifying the person or persons who are consuming media from inputs that are at a different level of granularity, for example device- or household-level measurements.


In the present discussion, I will focus on the process of personifying shared-screen viewing when measured at the household level. This methodology has been patented by Comscore as US Pat. No. 12,114,029, issued on October 8, 2024.


The problem statement is this: given knowledge of media consumption aggregated to the household level, as well as information about the persons who reside in the household (i.e., the household rosters), how can we estimate who within the household is consuming a particular unit of content? Or, more correctly, given consumption and roster information from a large number of households, how can we estimate the probability that different person-types are consuming that content? That is, the problem is not to try to estimate person-level consumption in any single household, but to measure average or aggregate consumption against any given program, ad, video, station, daypart, or whatever the “unit of content” to be measured may be.


To be more specific, the person characteristics most used in media measurement are age and gender. Comscore has traditionally measured person age/gender in the 18 “building block” buckets listed in Table 1, below. For the purposes of the present discussion, I will use television consumption as measured using set-top box (STB) return-path data as the primary example. The methodologies described here, however, apply equally well to CTV or digital consumption, when that consumption is measured at the household level and tied to known demographic rosters.


So, then, for a given unit of content, the problem is to take a large set of return-path data households with known rosters and estimate the probability for each of the 18 age/gender buckets to be watching that content.


**Approaches to Personification**


One way to perform the personification task is to equip a subset of households (a panel) with person meters that can identify which of the persons in each panel household is consuming what content. This is how a “people meter” panel works. It can be done by having a device connected to the TV where viewers indicate they are present by pushing a button or logging in with a remote control or mobile device. Or it can be done semi-passively, by having household members carry around a portable meter that listens for the audio from media content and identifies it by watermarking or ACR technology, or a camera can be installed on the TV that observes and records who is watching. In any case, a panel requires participation from all members of each panel household. Personification panels are expensive to build and operate – even more so to try to do so at the local market level; on the other hand, they directly measure the quantities of interest, and the methodology is easy to explain and understand.


Another, less familiar approach is to utilize a combination of classical statistics and modern data science to build inferential estimates of the person-level viewing probabilities. It is this approach that Comscore Personification (CSP) primary uses. Such an approach unlocks person measurement at scale, unconstrained by the limitations of a small panel. The rest of this paper is devoted to explaining and justifying such an approach.


Conceptually, the idea is to leverage the large number of STB households with deterministic demographic rosters to reveal an embedded “person viewing signal” within the household-level data. After all, houses don’t watch TV; people do. To take an artificial example, suppose there was a program that was only watched by men 45 to 54 years old. You would be able to see that simply by observing that no household without any M45-54 was watching; any viewing would be correlated with the presence of one or more M45-54 in a household. Real viewing probabilities are never so black-or-white, but nevertheless it is true that correlations between age/gender presence and viewing propensities can lead to an estimate of who is doing the viewing.


**Comscore Personification Methodology**


There are several ways to explain how Comscore uncovers the person signal from within the household observations. For the technically inclined, you can see this as an exercise in logistic regression, where the dependent variable (“Did household x watch content c?”) is a function of the person-level viewing probabilities. The details are left as an exercise for the reader.


Another way to look at it is as an application of Bayes’s Theorem, which is a well-known procedure for inverting conditional probabilities. To explain this application, let’s first define some notation. Let c represent the unit of content being considered, let Pc represent the (unknown, yet) person demographic viewing probabilities for content c, and Hc be the (observable) household viewing probabilities for content c. Then what we have could be denoted Probc(Hc | Pc): the probability of getting the observed household distribution, given the (unknown) person distribution. But what we want is Probc(Pc | Hc): the person probabilities, given the (observed) household distribution. Bayes’s Theorem allows one to calculate the second from the first.


A third way to illustrate our method is perhaps more intuitive. At the end of the day, for any given unit of content c, I really only need to calculate 18 numbers: the probability of viewing (or “rating”) of each of the age/gender buckets shown in Table 1. Ok, so suppose I have a candidate set of these 18 values. From that set, I can calculate the implied household probabilities for all households where I know the demographic roster. For example, if the probability of a female 25 to 34 years old watching a particular program is, say, 5%, then the probability of a single-person household with one F25-34 watching it is simply 5%. A two-person household with two F25-34s will have a probability of 9.75% of watching it. Where did this value come from? Well, if each of the two persons in this household have a 5% chance of viewing, then each has a 95% chance of not viewing, so the probability that person 1 did not watch and person 2 did not watch is 0.95*0.95 = 0.9025, or, in other words, the probability that at least one did watch is 1 – 0.9025 = 0.0975 or 9.75%. Without getting bogged down in the arithmetical details, the point is that there is a well-defined process of going from probabilities of individuals to probabilities of groups of individuals such as households. (There is an implicit assumption here, which I will return to).


Ok, so if I have a process for calculating household viewing probabilities from given person probabilities, then I’m almost there. I “just” need to keep trying different sets of the 18 person probabilities until I get as close as possible to the observed household ratings. Or, put differently, the challenge is to find the 18 values that collectively give me calculated household ratings as close as possible to what I actually observe. This is then a multidimensional nonlinear optimization problem, for which well-known procedures exist (downhill simplex being my personal favorite).


There is one implicit assumption in this last explanation that’s worth making explicit. The assumption that the “not-watching” person probabilities can be multiplied to compute the collective household probability of not watching assumes that the behavior of each person is independent of the behavior of all the others in the same household. But we know that’s generally not true: if one person is watching something, there is usually a greater probability that another in the same household will join them to watch. So, there is a “clumpiness” to human media consumption that isn’t captured by the simple statistical calculation described above. Obviously, this phenomenon is important to the measurement of co-viewing (multiple people watching simultaneously on the same screen) and deserves some special consideration.


If we condition purely on two data elements, demographic probabilities as described above, and number of people who live in a household, we can actually get fairly close to independently measured co-viewing rates. The size of the household is clearly an important factor: a single-person household, by definition, can only have a single resident viewer. A set of two-person households can average anywhere between one and two viewers, which is to say the “co-viewing factor” can be between 1.0 and 2.0. For many types of content, it ends up landing in the 1.3 range. Three-person households will have co-viewing factors between 1.0 and 3.0 and tend to be around 1.5. And so forth. But even accounting for household size in addition to the demographic probabilities, naïve co-viewing estimates tend to be somewhat low, due to the “clumpiness” described above.


To account for this peculiarity of human social behavior, new information must be brought to bear. Comscore solves this by using a recall survey, wherein respondents are asked who was watching what. There are many details to this survey which could be interesting to describe, but it’s important to understand that we only use the survey results to account for that little extra co-viewing that occurs after we’ve accounted for demographic probabilities and household size. The primary methodology is inferential and based on large return-path data sets and deterministic demographic rosters. The survey is a small but important additional correction. Mathematically, this additional effect (and the additional information from the survey) can be inserted into the calculation as an “interaction term” that accounts for the viewing correlations that exist among different members of the same household.


As a final step, the aggregate probabilities can be “pushed down” into the individual households using a Monte Carlo process so that all subsequent aggregations (for example, at the local level, or subset to specific sets of target households or persons) are logically coherent, by construction.


All of this may seem rather abstract; indeed, the solution to the problem is non-trivial and involves some fairly elaborate mathematics, statistics, and data science. Perhaps the best way to illustrate that the approach works is to show some concrete results.


**The Proof in the Pudding**


Comscore has in the past been able to use panel-based personification data. We have done literally thousands of comparisons between the panel estimates on the one hand, and our inferential methodology based on millions of set-top box households on the other. Figure 1 shows a side-by-side comparison of the personified estimates from the panel (in blue) and the Comscore Personification methodology (in red), for a particular national cable program, “Peppa Pig” on Nick Jr.


There is much to unpack here. First, the metric being shown is total viewing hours divided by the total person universe estimate in each bucket. This metric is proportional to the rating, and so it is a measure of how much viewing is occurring among all persons in each bucket. (Other metrics could be shown, for example number of persons viewing – but that metric would conflate the popularity of the show and the “size” of the bucket).


Second, I’ve included “error bars” of sorts here, but it’s worth mentioning that these are simply an estimate of sampling or standard error based on the size of the relevant population under measurement. I’ve made no effort to quantify errors due to sample bias or data collection issues. Think of these bars-on-bars as directional. I’ve also applied a convention whereby the blue estimates are darker-colored when they are not within these “error bars” of the red estimates, so as to highlight cases where the two methods give meaningfully different results.


And so what do we see here? Both methods give qualitatively similar measurements: this program is watched mainly by young children and younger adults (presumably parents), while being less popular among older adults. There are some differences, however: the panel method indicates fairly high popularity for this program among older teens and people in their early twenties. Which of the two trends is more realistic is for the reader to decide.


Another single-program example is shown in Figure 2, this time for “Golden Girls” on Hallmark. Here, again, both methods show broadly similar behavior, skewing older and more female, with some differences among older teens.


Figure 3 takes a different approach: rather than showing a single program, here we compare programs from two genres (“Politics/Public Affairs” for Figure 3a; “Kids” for Figure 3b), plotting the hours per (universe) person metric from the panel horizontally against the same metric from the inferential method vertically. Although there are some differences, the broad picture is largely the same, and if you believe that a panel is a good way to personify, then this plot demonstrates that the Comscore inferential methodology is roughly consistent.


**Conclusion**


It has sometimes been asserted that “there is no way to personify household measurements without a metered panel”. I believe that this statement represents a failure of imagination. Modern data science, built upon a foundation of classical statistics and applied to very large data sets, can produce robust results in many different arenas, sometimes in unexpected ways that are qualitatively different from traditional methodologies. Language translation systems, generative models of text and graphics, and speech recognition are examples of technologies that were thought to be unobtainable a generation ago.


Comscore has been a thought leader in the application of large data sets to traditional media measurement problems. Our latest advance, to use return-path TV data tied to known demographic rosters to estimate personification, is another example of that leadership.


**Acknowledgements**


Any project of this scale is a team effort. The development and implementation of the methodologies described here involved many of my Comscore colleagues. In writing this paper, I received useful feedback from Joe Garland, Robin Muller, and Joe Ruthruff.
