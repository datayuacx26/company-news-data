---
schema_version: "1.0.0"
document_id: "d392b9f4f6ea6ccb910e76060b88669a7af2caa5c90829aeef58a5a24afee261"
company_key: "yc-gem"
company: "Gem"
source_id: "yc-gem-news-import-b1a764fa81ce"
canonical_url: "https://www.gem.com/blog/deepfake-interviews"
published_at: "2026-05-18T07:00:00+00:00"
first_seen_at: "2026-07-21T21:23:12.789713+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:9d3274f783550290a7d296d505b97a25dd4d9e62c877b8ede2e2614e2fd86e93"
---

# The rising issue of deepfake interviews

SJ Niderost


Posted on May 18


The recruiter conducting the video interview noticed something odd. The candidate's mouth movements didn't quite sync with their words. The lighting on their face seemed slightly off from the room behind them. When asked to turn their head, there was a brief lag before the video caught up.


This wasn't a bad internet connection. It was a deepfake.


Gartner predicts that by 2028, one in four job candidates will be fake. Nowadays, hiring managers report catching deepfake candidates during video interviews. A fully deepfaked candidate, complete face swap, voice cloning, and fabricated credentials, can now be created in minutes using consumer-grade tools.


Deepfake interviews represent the fastest-evolving threat in hiring, and most organizations aren't ready.


## What is a deepfake interview?


A deepfake interview is a video or audio interview where a candidate uses AI-generated technology to impersonate another person or create a fabricated identity in real time. This includes face-swapping technology that overlays one person's face onto another during a live video call, voice cloning that convincingly replicates someone's voice, and full synthetic identities that combine fabricated faces, voices, and backgrounds.


It's important to distinguish deepfake interviews from AI-assisted interviewing. A candidate using ChatGPT to help formulate answers during an interview is using AI for content assistance. A deepfake candidate is using AI for identity-level deception, presenting themselves as someone they're not or as a person who doesn't exist at all.


The deception can serve multiple purposes: a qualified person interviews on behalf of an unqualified person who will actually do the job, a fraudster uses a stolen identity to secure employment for salary theft or data access, or state-sponsored actors create synthetic identities to infiltrate companies for espionage.


## How the technology actually works


Deepfake interview technology operates on three levels of sophistication:


Face-swap deepfakes use real-time neural networks to overlay one person's face onto another during live video calls. The technology tracks facial landmarks (eyes, nose, mouth) on the actual person's face and replaces them with the target face, maintaining expressions and movements. Modern face-swap tools like DeepFaceLive can run on consumer laptops with decent GPUs, making this accessible to anyone willing to spend a few hundred dollars on software.


Voice cloning creates synthetic audio that replicates a specific person's voice from just minutes of sample audio. Services like ElevenLabs or Descript can generate convincing voice clones that retain the original voice's emotional tone and speaking patterns. When combined with face-swapping, the result is a candidate who looks and sounds like someone else entirely.


Full synthetic identities combine face-swap technology, voice cloning, and completely fabricated backgrounds. These aren't impersonating real people but creating fictional identities from scratch. The AI generates a face that doesn't exist, pairs it with a cloned or synthetic voice, and supports it with fake credentials, LinkedIn profiles, and work histories.


The Pindrop demonstration made this terrifyingly real when a reporter's face was successfully swapped in real time during a Zoom call, with the deepfake passing undetected by the other participants. According to HR Dive, a fully operational deepfake candidate can be created in approximately 70 minutes by someone with moderate technical skills.


## What the cases tell us


Deepfake interviews aren't theoretical. They're happening at scale across multiple industries.


### The North Korean IT worker operation


The most extensively documented case involves North Korean IT workers who have infiltrated over 300 US companies using deepfake interviews, stolen US identities, and coordinated infrastructure. According to Department of Justice prosecutions, these operations have stolen over $6.8 million in wages while potentially accessing sensitive company systems and data.


The scheme works systematically: operatives obtain stolen US identities (names, Social Security numbers, addresses), create convincing fabricated credentials and work histories, use face-swap and voice technology to complete video interviews as the stolen identity, and route company-issued laptops through US-based "laptop farms" to mask their actual location.


The workers often have real technical skills, making their initial work of sufficient quality to avoid suspicion while they pursue their actual objectives: salary collection and potential espionage.


### The KnowBe4 hire


In perhaps the most ironic case, KnowBe4, a cybersecurity company that trains organizations to spot phishing and social engineering, inadvertently hired a deepfake candidate who passed four rounds of video interviews. The fraud was discovered only after the person attempted to install malware on company systems on their first day of employment.


The case is instructive because KnowBe4 presumably had sophisticated security awareness, yet the deepfake passed multiple rounds of screening. This illustrates that even security-conscious organizations with trained interviewers can be fooled by well-executed deepfakes.


### The recruiter who caught it live


According to HR Brew, one recruiter noticed subtle visual glitches during a video interview: slight lip-sync delays, unnatural lighting on the candidate's face, and momentary freezing when the candidate moved their head quickly. When confronted, the candidate claimed technical issues and abruptly ended the call.


What's notable: **it took human intuition to spot the deepfake, not technology** . The recruiter's gut feeling that something was "off" led to closer observation. Most video conferencing platforms have no built-in deepfake detection, meaning every deepfake caught today relies on human observers noticing subtle artifacts.


## Why current detection methods have limits


The uncomfortable truth about deepfake detection: **we may never have a reliable single-point method for catching them in real time.**


The simple tests are already obsolete. Early advice suggested asking candidates to "wave your hand across your face" or "turn your head quickly" to expose deepfake artifacts. Modern face-swap technology handles occlusion and rapid movements with minimal artifacts. These tests create false confidence; they signal to sophisticated fraudsters exactly what you're looking for, and they can be easily defeated with slightly better technology.


Human detection is barely better than random chance. A meta-analysis of 56 studies found that humans detect deepfakes with approximately 56% accuracy, barely better than flipping a coin. We're not naturally equipped to spot subtle artifacts in real-time video, especially during the cognitive load of an interview, when we're focused on evaluating answers rather than scrutinizing facial movements.


Detection tools are in a perpetual arms race with generation tools. Every advance in deepfake detection is quickly countered by improvements in generation technology. Researchers develop methods to spot face-swap artifacts, and then the next generation of face-swap tools eliminates those artifacts. Commercial detection tools exist, but their accuracy degrades quickly as generation technology improves.


The quality gap is closing fast. Early deepfakes had obvious tells, such as unnatural blinking, audio latency, or visible seams around the face. Modern tools have largely eliminated these issues. As computing power continues improving, the remaining artifacts will disappear entirely.


The reality: **deepfake generation is getting easier and better faster than deepfake detection is improving.** We're not winning this arms race.


## Why remote hiring is structurally vulnerable


The deeper problem most organizations miss: remote hiring removed a verification layer that was never formally designed but always existed.


In-person hiring provided implicit identity verification through physical presence. You shook hands with the person. They walked through your office. They interacted with multiple people in different contexts. Security badges captured their photo. These weren't formal identity checks, but they created a web of verification that made impersonation extremely difficult.


Video interviews were adopted as a convenience, not as a secure channel for identity verification. We treated them as equivalent to in-person interviews without recognizing that video calls don't provide the same level of implicit verification. Organizations built their entire remote hiring process on a channel that was never designed for identity verification.


This structural vulnerability means deepfake interviews aren't a problem we can patch with better detection tools. They expose a fundamental gap in how remote hiring validates identity.


## What needs to change


Addressing deepfake interviews requires rethinking hiring processes:


**Shift from single-point verification to multi-signal validation.** Don't rely on one video interview to verify identity. Use multiple verification points throughout hiring such as LinkedIn profile consistency with application materials, employment verification before final interviews, ID verification at multiple stages, technical assessments with browser monitoring, and reference checks with verified contact information. A deepfake might fool one video call, but maintaining the deception across multiple verification points over weeks is exponentially harder.


**Embed fraud detection into your recruiting workflow.** Don't bolt identity verification on at the end. Platforms like Gem's AI Fraud Detection Agent automatically evaluate applications across multiple risk signals, including LinkedIn verification, device ID, and IP patterns, email validation, employment timeline consistency, and behavioral patterns. Catching fraud at the application stage prevents deepfake candidates from reaching video interviews.


**Accept that perfect detection isn't possible, so build for resilience.** Design hiring processes assume some deception will slip through initial screening. This means robust background checks before start dates, limited system access during initial employment periods, monitoring for suspicious behavior during onboarding, and clear protocols for handling discovered fraud.


**Prepare for compliance and legal implications.** Drafts of deepfake disclosure laws are already underway in multiple jurisdictions. Organizations need clear policies around what they tell candidates about AI detection, how they document suspected deepfake cases, when they escalate to law enforcement, and how they protect themselves legally. Document your fraud detection protocols now before you need them.


**Invest in recruiter training for the AI era.** Technology alone won't catch sophisticated deepfakes. Train recruiters to recognize subtle behavioral signals, such as unnatural response delays, audio artifacts, visual inconsistencies, and candidates who are evasive about turning on video or performing spontaneous actions. The human element remains critical, even as the technology improves.


Deepfake interviews aren't a temporary scare. The technology only gets better and cheaper as AI models improve and computing power increases. The question for hiring organizations is whether their hiring process is built to catch one before it's too late.


## FAQ


### How common are deepfake interviews?


Deepfake interviews are still relatively rare but growing rapidly. The actual number we think it may be could be higher, as sophisticated deepfakes go undetected.


The prevalence varies dramatically by industry and role type. Technology companies hiring for remote software engineering, data science, or cybersecurity roles report significantly higher hiring rates. Companies hiring internationally, particularly for roles where salary arbitrage is attractive (US salaries paid to workers in low-cost countries), see a higher volume of deepfake attempts.


The North Korean IT worker operation alone has infiltrated over 300 US companies, demonstrating that organized deepfake operations operate at scale rather than as isolated incidents. Gartner's prediction that one in four candidates will be fake by 2028 reflects the trajectory as tools become easier to use and more widely available.


### Can deepfakes be detected in real-time video calls?


Current technology cannot reliably detect sophisticated deepfakes in real-time video calls. While commercial detection tools exist, they suffer from high false-positive rates (flagging real people as deepfakes), high false-negative rates (missing actual deepfakes), and rapid obsolescence as generative technology improves.


Research shows humans detect deepfakes with only 56% accuracy, barely better than chance. The subtle artifacts that might expose a deepfake, slight lip-sync delays, unnatural lighting, odd eye movements, are difficult to spot during an interview when you're focused on evaluating the candidate's answers rather than scrutinizing their face.


Some organizations use simple tests, such as asking candidates to wave their hand across their face or turn their head quickly, but modern face-swap technology handles these movements with minimal artifacts. These tests mainly catch low-quality deepfakes while giving false confidence about sophisticated ones.


The most reliable approach is multi-signal verification across the hiring process.


### Are deepfake interviews illegal?


The legal status of deepfake interviews is complex and evolving. Using deepfake technology to impersonate someone else or create a false identity for employment purposes is illegal under multiple existing laws, even if no specific "deepfake interview" statute exists.


Federal charges can include wire fraud (using electronic communications to obtain employment under false pretenses), identity theft (using someone else's identity), conspiracy to commit fraud, and false statements to employers (in regulated industries or federal employment). Recent prosecutions of North Korean IT workers have resulted in indictments under these statutes.


State laws vary, but many jurisdictions criminalize identity fraud, computer fraud, and employment fraud that would cover deepfake interviews. Some states have passed or are considering specific deepfake legislation that may explicitly address employment contexts.


Beyond criminal liability, civil consequences include immediate termination if discovered after hire, employers suing to recover salaries and costs, professional reputation damage making future employment difficult, and potential deportation or visa violations for international workers.


The Department of Justice has made prosecuting organized deepfake employment fraud a priority, particularly for state-sponsored operations, so the legal risk for perpetrators is substantial and growing.


### What industries are most targeted by deepfake candidates?


Technology companies face the highest rates of deepfake interview attempts, particularly for remote roles in software engineering, data science, cybersecurity, and IT infrastructure. The combination of high salaries, remote work enabling geographic arbitrage, and technical complexity, making verification harder makes tech especially attractive to deepfake fraudsters.


Financial services and fintech companies are heavily targeted because positions provide access to sensitive financial data, the potential for fraud or data theft is high, and roles often offer substantial compensation with remote options.


Defense contractors and companies with government contracts face state-sponsored deepfake attempts to access classified information, steal defense technology IP, or compromise secure systems. The North Korean IT worker operations specifically target these sectors.


Healthcare and pharmaceutical companies attract deepfake candidates seeking access to patient data, drug development intellectual property, and research findings, though medical licensing requirements make sustained fraud harder.


Any industry offering remote-first positions with high salaries and minimal in-person requirements will see an increase in deepfake attempts, as the opportunity for salary arbitrage and the difficulty of detection make these roles attractive targets.
