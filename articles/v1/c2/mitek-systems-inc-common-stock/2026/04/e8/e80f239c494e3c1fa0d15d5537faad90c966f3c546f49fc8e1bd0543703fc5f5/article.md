---
schema_version: "1.0.0"
document_id: "e80f239c494e3c1fa0d15d5537faad90c966f3c546f49fc8e1bd0543703fc5f5"
company_key: "mitek-systems-inc-common-stock"
company: "Mitek Systems Inc."
source_id: "mitek-systems-inc-common-stock-rss-3bf79578716e"
canonical_url: "https://www.miteksystems.com/blog/beyond-deepfake-detection-building-resilient-identity-systems"
published_at: "2026-04-21T14:07:07+00:00"
first_seen_at: "2026-07-20T23:21:50.456876+00:00"
fetched_at: "2026-07-28T22:15:46.234407+00:00"
content_hash: "sha256:6d994c448d9443d8a12371ccc2ba1563e0958d615f53857cefd529aeccb3b4bf"
---

# Beyond deepfake detection: building resilient identity systems

We are in a new era of identity fraud. The tools being used by attackers today – think AI-generated deepfakes, synthetic media, and coordinated injection attacks – go beyond being incremental improvements on yesterday’s attacks. In many ways, fraudsters’ prosthetic masks and printed photos look quaint by comparison. Today’s attacks are fundamentally different, highly sophisticated threats that target a critical vulnerability in most identity verification systems: the belief that one detection layer is sufficient.


In research that looked at the efficacy of hundreds of identity verification deployments, over 70% of advanced fraud attempts required multiple detection layers to stop them. This is especially true for attacks like deepfakes. A single safeguard, like liveness detection or template matching, leaves your organization exposed to attackers once they understand what’s in place and where your remaining blind spots are.


The question that decision makers at financial institutions, fintechs, and other enterprises are now faced with isn’t whether the growing threat from deepfakes is real – it is. The question is: what does an effective layered defense against deepfakes require?


## Why liveness alone fails


Active liveness detection (the kind of liveness detection that requires user participation, for example, making gestures like turning their head or blinking) was designed to help prevent presentation attacks. A static photo or a pre-recorded video replayed on the screen won’t perform the required movement on command. It’s a simple defense against the threat it was built to counter.


Deepfake technology, though, has moved past simple video replays. Modern synthetic media can contain convincing facial-micro-expressions and is capable of quickly generating movements in response to system prompts. Advanced deepfakes, when tested against these conventional liveness checks, can pass. This isn’t because the technology failed, but because it was solving for the wrong problem – looking for responsiveness, not looking for video authenticity.


Active liveness also has a “cost” to the customer, as it creates substantial user friction. At one organization that implemented active liveness detection that required users to follow on-screen prompts and perform several actions in a multi-step process, customer dropoff was significant – resulting in a completion rate of only about 60%. After they made a strategic decision to implement passive liveness detection, which required users only to take a selfie rather than follow any prompts or make any gestures, their completion rates soared, to over 95%.


## Why deepfake detection alone falls short


When organizations rely too heavily on deepfake-specific detection alone, they solve one problem, but hit other limitations. Deepfake detection solutions are sophisticated. They’re able to detect digital signals like compression artifacts, inconsistent skin texture, unnatural eye movement or reflections, and other telltale signs of the use of face-swap algorithms or diffusion models. And they’re remarkably accurate: modern deepfake injection detection achieves greater than 99% accuracy when it comes to identifying the use of known generation engines.


While those numbers are impressive, they’ve got an implicit limitation: they’re only as good as the attacks they were trained on. In this era where new generative AI tools emerge monthly or weekly, it can be difficult to keep their training up to date.


For another example of why deepfake detection alone isn’t enough, consider a targeted phishing campaign found in a recent threat investigation. The campaign used over 3,000 injection attacks, which were fraudulent submissions that combined multiple attack vectors simultaneously. Some were deepfakes, others were not. An organization protecting only against deepfakes would have missed many of the attempts.


## The limits of point solutions


Looking at liveness alone and deepfake detection alone makes the danger of using point solutions clear. These systems create blind spots. Extensive testing and real-world fraud data confirm that the answer to whether one system alone is “enough” is no.


Your deepfake detection algorithm might be world class, but provides you no protection against template injection attacks, or against presentation attacks captured through insecure channels, when they get modified in transit. If a system captures 95% - or even 100% - of one type of attack but misses other sophisticated forms of fraud, it is still a system that will lose the battle against sophisticated fraud.


Layered detection is the only way for modern institutions to meet their security imperatives and simultaneously maintain usability. When passive liveness detection is combined with deepfake injection analysis, template-based fraud matching, and channel integrity verification, each layer uses its unique capabilities to catch different types of attacks, all without impacting the customer experience.


A layered system, for example, would catch a fraudster who created a synthetic video that passed liveness detection, but contained multiple deepfake markers. It would also catch an attacker who might be extensively familiar with deepfake detection algorithms but who made a mistake in template matching. It can also catch the fraudster who is capable of successfully injecting content into the biometric capture process but who created a detectable anomaly in channel analysis.


## What layered detection means in practice


To be effective, three critical capabilities are essential. Many systems lack detection capabilities in one or more of these areas.


Detection across capture analyze content at the moment it is captured. This is where passive liveness detection determines a real person is physically present, where deepfake indicators are detected, and where presentation attacks are stopped. Some systems only look at static images after submission; these systems miss the temporal and behavioral data that’s available during the actual capture process.


Detection across transit ensures the security of the path between a user’s device and your verification system. The goal is to ensure content isn’t intercepted, modified, or replaced. Channel integrity verification ensures that the data that arrives at your servers is the data that actually left the user’s device. This layer can catch sophisticated attacks that can’t be addressed through deepfake and liveness detection alone.


Detection across comparison compares the presented identity against known fraud patterns, and legitimate user profiles. This is where template-based fraud matching, profile anomaly detection, and behavioral analysis come into play. In these situations, a user might be flagged not because they presented altered or synthetic media but because their overall submission pattern is suspicious.


One major financial institution that was seeing losses from fraud initially attempted to solve the problem by upgrading just one layer, deepfake detection. While they got better at detecting synthetic media, their fraud losses actually increased. Why? Attackers simply switched tactics. To make progress, the institution in question had to shift their mindset from “how do we improve our detection of deepfakes?” to “what comprehensive layers of security are needed to make any single fraud tactic ineffective against us?”


### What Is the Best Way to Prevent Deepfake Fraud?


As the previous examples illustrate, there is no single best way.
The best defense against deepfake fraud is the same defense required to protect against injection attacks, presentation attacks, and all other sophisticated fraud tactics: the use of layered detection that operates across capture, transit, and comparison, with each layer operating independently and specifically tuned to maintain a high level of security and usability for your specific user base.


## The 5-point decision checklist


Here are five important dimensions to consider before upgrading your identity verification system:


1. Are your detection signals truly independent? If you have multiple layers but they’re looking for the same thing using different methods, you haven’t created a layered defense; you’ve created redundancy.
2. Do you have detection capabilities across capture, transit, and comparison? A system that only analyzes the final image missed attacks during capture. A system that doesn’t have channel integrity verification leaves you vulnerable to injection attacks. And one without comparative analysis leaves you exposed to fraud from compromised documents.
3. Can your system detect tactic switching? Sophisticated attackers will test your defenses and adapt. If a fraudster can find an undefended pathway by switching from deepfakes to injection attacks, your defenses aren’t truly layered.
4. What does your system do with ambiguous results? To preserve the customer experience for legitimate users, look for a system that supports graduated responses and not a binary/pass fail.
5. Is your system tuned for your customer population? A truly effective solution will have undergone rigorous testing and extensive training to ensure it delivers unbiased results across all populations, taking race, gender, age, and other demographic variables into account. A system, layered or not, built without this testing will implement unwanted discrimination and false positives.


## How layered detection reduces fraud without increasing friction


Conventional wisdom suggests that heightened security always has a tradeoff with speed and customer experience: the more checks you implement, the more likely a legitimate user is to drop off at any given checkpoint. But, when organizations implement layered detection that can operate seamlessly, they often experience stable or even improved completion rates.


Consider the example of the organization that switched from active to passive liveness. They maintained multiple detection layers while seeing completion rates improve from 60 to over 95%. The increased security operated in the background, and actually streamlined the process for their users.


With multiple layers detecting fraud signals independently, you can maintain high approval rates for your legitimate users while catching fraud. Real customers generally won’t experience any friction, and fraudsters won’t experience success.


This approach has been implemented by leading institutions in more than 70 countries. They’re now detecting fraud better, with authentication systems that work faster and cost less to operate while continuously adapting to threats that emerge.
