---
schema_version: "1.0.0"
document_id: "14f9dbafcb37b37f9d13d7116ccfbf8f31181f053adcd6b375f216eb630921d7"
company_key: "yc-reality-defender"
company: "Reality Defender"
source_id: "yc-reality-defender-news-import-247f3b081c39"
canonical_url: "https://www.realitydefender.com/insights/defcon-34-recap"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T23:03:17.906744+00:00"
fetched_at: "2026-08-19T23:03:18.989069+00:00"
content_hash: "sha256:8baed3a785e7b972b3092f334b895547a724ff4e64394bdc69508a3b662fad8b"
---

# A village full of hackers couldn't tell real from synthetic at DEF CON 34

At


[DEF CON 34](https://defcon.org/html/defcon-34/dc-34-index.html) , Reality Defender ran a controlled test of the one skill every AI-fraud defense program wrongly assumes people have: telling a real voice, face, or video from a synthetic one. Hundreds of security professionals played. The average score was less than 50%, even though every player knew going in that half of what they’d see was a


[deepfake](https://www.realitydefender.com/insights/what-is-a-deepfake) and was actively trying to catch the fakes.


****


Each year, DEF CON, the world's largest hacker convention, brings together some of the security industry's most skeptical audiences, professionals whose entire discipline is built on not trusting things at face value. The Guess the Deepfake challenge put their skepticism to the test, running in the


[AI Village](https://aivillage.org/) all three days of the conference. Each round showed a player a 30-item deck of images and videos, split evenly between authentic and AI-generated content, and asked for one binary call per item: real or synthetic. Many of the 30,000 DEF CON attendees played, among them hackers, red teamers, security researchers, incident responders, and malware analysts. Players knew in advance that half the deck was synthetic and, following the test, received a score of how many questions they answered correctly out of 30.


Reality Defender's red team built every synthetic item using a mix of manipulation and generation techniques, including state-of-the-art diffusion models for image generation and editing, inpainting tools for facial manipulation, and text-based generative methods. In addition, the red team developed lip-synced media using advanced lip-syncing techniques and synthetic audio generated with state-of-the-art speech generation models. Authentic content came from real-world sources: podcasts and YouTube for video, stock photography marketplaces and in-house photos for images.


****


The core lesson is clear: once seeing and hearing stop being reliable proof of authenticity, human judgment can't be the final security control. With deepfakes hitting everyday workflows at unprecedented quality and scale, detection has to be a built-in control, sitting at the point where media enters these systems rather than a downstream check run after the decision's already been made.


****


Here's a closer look at the Guess the Deepfake results, and our week in Vegas.


Humans Can’t Reliably Detect Synthetic Media


How can organizations put the burden of distinguishing real from synthetic media on people, when security professionals actively looking for manipulation still perform worse than chance?


****


Synthetic media defeats instinct, attention, and expertise across modalities. Many visitors stepped up convinced they'd land on our leaderboard, but confidence didn't translate into accuracy. The average score was less than 15 out of 30. A coin flip would have landed there by chance alone; this room did worse.


“The conditions were as favorable as they will ever be. Players knew the deck was half synthetic, they were hunting for fakes, and nothing was riding on the answer.” –


[Alex Lisle](https://www.linkedin.com/in/alexlisle/) , CTO of Reality Defender.


Perception under these conditions is the best case, not the worst case. Real attackers don't hand out any advantages in real-world fraud campaigns. They deliberately introduce synthetic media at a moment of their choosing, within an otherwise routine interaction. At the same time, the target is focused on doing their job, not on judging whether the voice on the call or the face on the screen is real. Take away the warning, diversify your targets, add time pressure and a routine task, and the odds only get worse.


The Collapsing Cost of a Convincing Deepfake


Unreliable perception would matter less if fabrication were still hard. It is not. At DEF CON 34, in addition to our game, our red team brought to life a high-fidelity, real-time voice-cloning activation using basic open-source tools in roughly 15 minutes on commodity hardware. At the AI Village station next door, attendees watched their own faces be replaced in a live face swap, which also ran on open-source tooling. Capabilities that previously required specialized infrastructure, six-figure budgets, and deep technical expertise are now open to anyone with freely available software.


****


Part of that democratization traces back to a shift in the underlying technology. Early deepfakes ran on generative adversarial networks, or GANs, which pit one model against a second model until the output is convincing enough to fool it. The tools spreading fastest today increasingly run on diffusion models instead, building media by iteratively removing noise from a random starting point rather than racing a built-in detector. Diffusion output tends to carry fewer of the visual tells, flickering edges, asymmetric reflections, that once gave GAN-era fakes away.


On top of this, fraud tooling now ships the same way as phishing and exploit kits: packaged, subscribed to, priced for volume. ATHR, for example, is a vishing-as-a-service platform sold on underground forums for $4,000 plus 10% of profit that automates personalized lures, AI voice agents, and credential harvesting into a single subscription (


[Bleeping Computer](https://www.bleepingcomputer.com/news/security/new-athr-vishing-platform-uses-ai-voice-agents-for-automated-attacks/) ). Attackers no longer have to build anything. They subscribe.


Ultimately, because of the near-zero cost of creating and deploying high-quality deepfakes, the math tilts heavily in the attacker's favor. Fraudsters can easily tolerate hundreds of failed calls, automated account-recovery attempts, or synthetic applicants if just one succeeds in reaching the person or automated system authorized to reset a credential, grant access, move money, or disclose sensitive information. The enterprise, however, can’t afford to determine authenticity downstream.


Top Use Cases We Heard on the Floor


****


The collapsing skill and cost barriers to creating convincing manipulated content have driven concern across use cases. Speaking directly with security leaders throughout the conference, these were among the top concerns our team heard on the floor:


****


-


[Executive Impersonation](https://www.realitydefender.com/solutions/brand-executive-protection) in Financial Workflows


: Deepfake voices and video are increasingly deployed to


[impersonate corporate leadership](https://www.realitydefender.com/insights/deepfake-whaling-defense) and authorize high-value transfers. In a widely reported case, an employee at the engineering firm Arup authorized $25 million in wire transfers after joining a video call where every other participant, including the CFO, was a synthetic recreation (


[CNN, 2024](https://www.cnn.com/2024/05/16/tech/arup-deepfake-scam-loss-hong-kong-intl-hnk) ).


-


Hiring, HR, and Talent Acquisition


: Malicious actors use AI-generated video, cloned voices, and fabricated credentials to impersonate real people during remote interviews, secure employment, and infiltrate corporate networks or siphon salaries. Federal indictments highlight sophisticated nation-state actors, such as the


[North Korean operative who infiltrated KnowBe4](https://blog.knowbe4.com/how-a-north-korean-fake-it-worker-tried-to-infiltrate-us) using AI-manipulated photographs to bypass video interviews and background checks. Still, the threat extends far beyond state-backed hackers. Broad commercial exploitation now enables widespread proxy interviewing, weaponized by diverse threats, ranging from opportunistic scammers who target multiple illicit corporate payrolls to sophisticated adversaries seeking privileged insider access to critical systems and proprietary data. With one in four


[candidate profiles projected to be fake](https://www.realitydefender.com/insights/synthetic-employee-hiring-fraud) by 2028 (


[Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-07-31-gartner-survey-shows-just-26-percent-of-job-applicants-trust-ai-will-fairly-evaluate-them) ), deepfake verification in talent acquisition has shifted from a perimeter IT issue to a core boardroom priority.


-


Contact centers and customer support


: By pairing generative AI voice cloning with autonomous conversational agents, fraudsters can launch high-volume


[voice-based social engineering](https://www.realitydefender.com/insights/deepfake-detection-wall-street-vishing) attacks that bypass IVR menus. Furthermore, as agentic AI expands to handle 80% of customer support interactions by 2029 (


[Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-03-05-gartner-predicts-agentic-ai-will-autonomously-resolve-80-percent-of-common-customer-service-issues-without-human-intervention-by-20290) ), contact centers face operational threats, including a compounding financial risk where rogue


[AI callers](https://www.realitydefender.com/insights/agentic-ai-caller-problem) lock automated virtual agents into prolonged, token-hungry conversations because neither side hangs up, and an infrastructure risk where a high volume of these automated calls triggers a denial-of-service (DoS) attack that overwhelms system capacity, spikes handle times, and degrades service for real customers.


-


Identity Verification (IDV) and Biometric Bypasses:


The foundational promise of identity verification has always been simple: proving you are who you say you are. Today, generative AI means anyone can become anyone, forcing organizations to expand their strategy from validating identity credentials to proving a user's authentic liveness. Instead of trying to fool a smartphone camera from the outside, attackers are deploying


[digital injection attacks](https://www.realitydefender.com/insights/red-teaming-googles-new-selfie-video-sign-in) that use virtual webcams, API intercepts, and software emulators to feed synthetic video streams directly into the verification pipeline. This technique completely neutralizes traditional presentation attack detection and tricks biometric liveness algorithms into validating fraudulent identities at the point of ingestion, allowing bad actors to hijack the entire trust chain.


[Deloitte's](https://www.deloitte.com/us/en/insights/industry/financial-services/deepfake-banking-fraud-risk-on-the-rise.html) projection that U.S. fraud losses enabled by generative AI will surpass $40 billion by 2027 is no surprise. Synthetic audio, video, and imagery are no longer futuristic concepts; they are easy to create and are already appearing at the exact touchpoints where organizations make critical decisions.


****


Reality Defender Caught Every Deepfake in The AI Village


Following each visitor's real-time face swap next door, we ran the resulting clips straight through our models while the person wearing the borrowed face watched the result come back. Every single clip came back flagged as manipulated, at 99.8 to 99.9% per-file confidence.


Deepfake Detection Needs to Be Embedded into Real-Time Workflows


"When a voice, face, or piece of media can influence access, money, identity, or sensitive information, organizations need an independent detection layer capable of evaluating that media in real time before the person on the receiving end is forced to make the call." – Alex Lisle, CTO of Reality Defender


****


Visitors left the AI Village with a clear takeaway:


[deepfake detection](https://www.realitydefender.com/solutions) must be a built-in control at the exact point of ingestion, before the interaction has already been trusted, routed, or acted upon.


****


[Employees shouldn’t hold the onus of deciding](https://www.realitydefender.com/insights/ciso-deepfake-policy-ownership) whether a voice, face, or video is real. Our own study tested that exact judgment under nearly ideal conditions, and the result was below chance. Just as email security evaluates messages before they reach an employee, endpoint security analyzes activity in real time. Identity systems assess risk before access is granted, deepfake detection has to operate at the same point of ingestion. We don't expect humans to determine whether an attachment is ransomware; we shouldn't expect them to determine whether the person on the other end of a call or video is authentic.


****


Deepfakes can now convincingly recreate any voice, face, or video in real time. Seeing and hearing are no longer proof of authenticity. Reality Defender detects AI-generated and manipulated media across audio, video, images, and text before it reaches the point of decision. It


[deploys through API and native integrations](https://www.realitydefender.com/insights/api-first-deepfake-detection) directly inside existing workflows, no rip and replace.


****


Want to try our models yourself?


[Test out Reality Defender's public developer API and SDK for free in just two lines of code](https://www.realitydefender.com/insights/reality-defender-launches-free-access-to-deepfake-detection-api) .
