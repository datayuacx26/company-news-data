---
schema_version: "1.0.0"
document_id: "5248611597b38ea1dcaee580f5a65c8bc27c9141d9b355f86da2cd9faff9dca2"
company_key: "mitek-systems-inc-common-stock"
company: "Mitek Systems Inc."
source_id: "mitek-systems-inc-common-stock-rss-3bf79578716e"
canonical_url: "https://www.miteksystems.com/blog/deepfake-fraud-is-scaling-faster-than-defenses"
published_at: "2026-05-15T14:07:07+00:00"
first_seen_at: "2026-07-20T23:21:50.456876+00:00"
fetched_at: "2026-07-28T22:13:06.313483+00:00"
content_hash: "sha256:c6332b48252fdbd6e597a044adb22296919daa47d78168f184581874a1bc6c15"
---

# Deepfake fraud is scaling faster than defenses

Deepfake fraud is increasing. Consider, for example, a case that made global headlines: at one Hong Kong based company, an employee was convinced to transfer $25 million to fraudsters after receiving a video call from his CFO and colleagues. The CFO and colleagues, however, were all deepfakes. But they were convincing enough in appearance and behavior to overcome any skepticism from the recipient of the call, and to convince them to override normal transaction approval procedures.


Compared to the ways in which fraudsters will target identity verification systems using traditional forged documents or stolen credentials, deepfakes take the sophistication to the next level. For a deepfake, criminals will use generative AI tools to create synthetic media – whether it’s video, audio, or still images – that are capable of impersonating the target individual (or presenting a synthetic identity) with remarkable accuracy. For financial institutions, a deepfake that goes undetected can allow fraudsters to bypass liveness checks or biometric verification, or otherwise impersonate parties during high-stakes transactions, even if they’ve never met their victims in person.


The distinction in methodology matters because deepfake fraud happens at the point of capture. A synthetic video selfie submitted by a fraudster during the identity verification process is fundamentally different from an injection attack where the data is intercepted and altered as it moves through the authentication process. While both represent a threat to financial institutions, they require a different defensive posture.


## Why are deepfakes redefining identity fraud risk?


The recent acceleration in deepfake fraud is staggering. In North America, deepfake fraud increased 1,740% between 2022 and 2023. The numbers continue to grow and even to dominate some other types of attacks; deepfakes were 55% higher than injection attacks in early 2025, and projections for the continued growth of deepfake fraud suggest an increase of 700% more by 2031. These numbers reflect a trend that is clearly extending far beyond the handful of high-profile cases that have reached the news.


The growth is largely due to tools that make creating a realistic deepfake faster and easier than ever before. Generative AI tools have dramatically lowered the cost, needed skillset, and complexity of creating deepfake media. A decade ago, creating a deepfake video required specialized AI and machine learning skills, expensive software and hardware, and very likely weeks of work. But today, consumer-grade tools are capable of generating realistic deepfakes in minutes. For anyone with access to a computer, the barrier to entry is low.


## The impact of deepfakes


The financial impact of deepfake fraud is also accelerating. According to Deloitte, losses to fraud from generative AI could surge from $12.3 billion in 2023 to $40 billion by 2027. This reflects a tripling of losses in just a four-year period, a trajectory that should get the attention of everyone tasked with fraud prevention.


Because of their versatility, the impact of deepfakes is widespread. A single synthetic face can be used to impersonate an account holder during onboarding, a customer during a wire transfer approval, or an executive during a social engineering attack. Once a fraudster has built a convincing deepfake, they’ll iterate, refine and reuse it across as many targets and institutions as possible.


## The enterprise reality: the preparation gap


Are financial institutions ready for what’s coming? According to recent enterprise surveys, 74% of enterprises are already seeing AI-powered threats in their ecosystem. And an even more troubling statistic is that almost 9 out of 10 believe that these AI-powered threats are just getting started. And many fear they’re vulnerable. 60% of CISOs, CIOs, and IT leaders openly admit that they’re afraid their organizations might not be prepared to meet this threat.


That fear is justified if you consider that most identity verification systems were designed to detect stolen IDs and forged documents. Liveness detection added an additional layer to solve the question of whether a person was live and present, but even liveness detection was built under the assumption that it was examining a real video of a real person. Detecting deepfakes requires an entirely different toolset.


Deepfake creation is scaling faster than the implementation of new defenses, resulting in a widening preparation gap. Institutions continue to deploy point solutions like liveness check and document verification without layering in tools that can address a fundamental new vulnerability: AI-generated media that can appear indistinguishable from real content.


### Can deepfakes bypass liveness detection?


In some instances, yes.


An advanced deepfake that incorporates micro-expressions, eye movements, and temporal consistency will be able to bypass a basic liveness test because it’s incorporated what the algorithms are looking for. And doing this doesn’t even require the quality of deepfake that could fool a human reviewer. Software that has been trained on a few points of authentic video is a much easier target.


Passive liveness detection, where users take a selfie without performing any active gestures, will, in contrast, examine video and audio for artifacts that are unique to AI-generated content. These artifacts answer the question of whether the video itself is authentic, a different question entirely from whether the person in the video appears to be real because they are behaving in a realistic manner.


Passive liveness detection technology works against deepfakes because, despite their sophistication, they contain traces of digital manipulation detectable on the algorithmic level. Even a generative model that can fool human eyes will contain imperceptible differences in lighting, eye reflections, skin texture, and/or audio synchronization. These are the digital fingerprints that passive liveness hones in on, as proof that a neural network created the content.


Modern, multi-layered approaches that leverage passive liveness detection maintain a 99% pass rate for legitimate users while filtering out and rejecting deepfake images. Mitek’s IDLive Face, a leading liveness detection technology, is one of only two passive systems that achieved 100% impostor blocking in evaluations by the Department of Homeland Security. IDLive Face is easy to deploy seamlessly throughout the customer journey to provide stronger security across every touchpoint.


### Why detection alone fails


Many institutions make the mistake of treating deepfake defense as a detection problem alone. Once they’ve got the capability of identifying which videos are synthetic, they consider the problem solved. It’s a mindset that leads to the implementation of point solutions, like a new algorithm or detection tool, getting bolted onto their identity verification pipeline.


But this detection-only approach fails for a fundamental reason: fraudsters are innovating faster than detection systems can adapt. They’re now moving at the speed of AI development, and new deepfake techniques emerge more and more frequently. Every new technique means a detection algorithm must be retained, evaluated, and deployed, all while fraudsters are testing their next and newest evasion techniques.


This is why advanced identity verification systems aren’t reliant on a single technique. No approach – whether it’s liveness detection, document verification, or behavioral analysis – is sufficient on its own any longer. To be protected, institutions must combine software hardening, data analysis, and AI-powered authenticity checks into a cohesive, layered defense. Taking a layered approach like this makes deepfake fraud unfeasible for fraudsters: when they have to simultaneously overcome passive liveness detection, document matching, behavioral analytics ,and biometric validation, their costs and complexity escalate dramatically. This is when they pivot to easier targets.


### The injection attack connection


Deepfakes are only one vector in a broader attack surface. Injection attacks, where fraudsters manipulate data as it’s flowing through identity verification pipelines, are a threat in parallel. Deepfakes leverage manipulated content at the capture stage, while injection attacks present manipulated content during the processing or transmission stage.


Because of this, an institution that has implemented excellent deepfake detection at the capture stage might still remain vulnerable. This is why a comprehensive defensive strategy is needed. Deepfakes and injection attacks require different detection mechanisms, and a solution that provides a layered defense that can address multiple threat vectors, simultaneously.


## The path forward


Deepfake fraud is ramping up, because the technical barriers to content creation are collapsing and the economic incentive to do so is massive. This doesn’t mean, however, that deepfakes are unstoppable. The institutions that move from a detection-only mindset to a layered defense strategy, combine passive liveness with document verification and behavioral analytics, and recognize that deepfakes and other attack vectors like injection attacks are interconnected will be able to significantly reduce their losses.


attacks are interconnected will be able to significantly reduce their losses.
Deloitte’s projection of $40 billion in annual losses to this threat by 2027 looms heavily over financial institutions, but organizations that act now to protect their systems and their customers will be best positioned to do their part to ensure this prediction doesn’t come true. Organizations that don’t act and continue to rely on yesterday’s defenses will have to face tomorrow’s emergent threats unequipped.


Explore how layered defense strategies address deepfake fraud in our next post: why detection alone fails and what resilient identity systems actually require.
