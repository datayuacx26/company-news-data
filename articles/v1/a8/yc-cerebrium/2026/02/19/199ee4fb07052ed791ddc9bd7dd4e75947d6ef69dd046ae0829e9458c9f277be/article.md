---
schema_version: "1.0.0"
document_id: "199ee4fb07052ed791ddc9bd7dd4e75947d6ef69dd046ae0829e9458c9f277be"
company_key: "yc-cerebrium"
company: "Cerebrium"
source_id: "yc-cerebrium-news-import-eb465eef12b4"
canonical_url: "https://cerebrium.ai/blog/how-bithuman-scaled-digital-humans-10x-faster-with-cerebrium"
published_at: "2026-02-25T00:16:05+00:00"
first_seen_at: "2026-07-21T13:06:54.599340+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:1aa4fb23b78c9e76c5908771f5a7eab3fdb42f0326b3f540efc1988016fdc1bc"
---

# How bitHuman Scaled Digital Humans 10x Faster with Cerebrium

How bitHuman Scaled Digital Humans 10x Faster with Cerebrium


### Bringing Humans to the Digital World: The bitHuman Story


bitHuman is revolutionizing human-device interaction by creating intelligent digital characters capable of real-time, emotionally engaging conversations. "We're building the most advanced human-device interaction possible," explains Steve Gu, CEO and Co-founder. With his second AI startup, Steve is driven by a vision of scaling human presence and empathy into the physical world—particularly in omni-channel sales and customer engagement.


What makes bitHuman exceptional? Three core innovations stand out:


**On-device performance** – Their models run entirely on CPU (supporting both ARM and x86 architectures), eliminating GPU requirements and enabling deployments that are often 10x more economical than GPU-based alternatives.


**Generative flexibility** – They can generate both human and non-human avatars in real time, whether photorealistic or fantastical, using simple prompts via voice, image, or video.


**Offline-first capability** – Their models operate entirely offline without cloud dependency, with SDKs enabling users to run locally or self-host.


### The Breaking Point: When Azure Couldn't Keep Up


Before Cerebrium, bitHuman faced mounting infrastructure challenges that threatened their growth trajectory. Using Azure Functions and reserved GPU/CPU instances, they encountered a perfect storm of problems: unreliable autoscaling, steep learning curves, and most critically—cold-start latency that often exceeded 30 seconds.


"Launching a bitHuman instance often took over 30 seconds, which frustrated users and limited real-time responsiveness," Steve recalls. For a product promising instant, human-like interactions, this delay was unacceptable.


The technical requirements were demanding:


-


Auto-scale seamlessly from zero to hundreds of concurrent sessions


-


Handle sudden spikes—imagine 1,000 tradeshow attendees accessing avatars simultaneously


-


Minimize cold-start latency for models ranging from 500MB to 1.5GB


-


Maintain cost efficiency without idle infrastructure overhead


### The Search for Speed


bitHuman evaluated multiple options: in-house servers, Azure, RunPod, and Cerebrium. The turning point came when one of their engineers ran an internal demo with Cerebrium—launch times plummeted from 30 seconds to under 10, and even under 3 seconds when warm.


"Cerebrium's platform is not only fast and cost-effective, but also extremely developer-friendly," Steve notes. "Deploying with just a .toml file was refreshingly simple."


But performance was only part of the equation. The Cerebrium team's responsiveness sealed the deal. "Slack questions were typically answered within 30 minutes. The combination of performance, simplicity, and support made the choice obvious."


### From Weeks to Hours: The Transformation


The impact was immediate. Within the first week, bitHuman deployed services to run 24/7—something that had been far more complex and brittle on Azure. When they hit an initial blocker with file uploads, the Cerebrium team resolved it within 30 minutes of reporting.


"Frankly, I never expected to be hands-on with deployment myself," Steve admits. "My background is more in general management and technology vision. But with Cerebrium, I now feel empowered to deploy and manage services independently."


Tasks that previously required weeks of DevOps effort now took just hours, with clear workflows and simplified troubleshooting. The platform democratized deployment across their team.


### The Numbers Tell the Story


The business impact has been substantial:


##### **Performance gains:**


-


Cold-start times reduced from 30 seconds to under 10 seconds


-


Warm-start times dropped to under 3 seconds


-


Zero major downtime since migration


##### **Cost savings:**


-


$5K-$10K monthly savings


-


Deprecated entire Azure-based architecture


-


Eliminated need for reserved instances, Azure Functions, Storage, and DataDog


-


Everything consolidated into a single platform


##### **Development velocity:**


-


Deployment time reduced from weeks to hours


-


Serverless functions (GPU or CPU-based) deployed much faster


-


Better visibility and control across the stack


### Reliability at Scale


"We've had no major downtime. Reliability has been excellent," Steve confirms. For a company whose digital humans need to be available 24/7 across multiple channels, this stability is crucial. The improved performance directly translates to better user experiences—when someone interacts with a bitHuman avatar, it feels instantaneous and natural.


### A Partnership That Delivers


The relationship with Cerebrium extends beyond vendor and customer. "The team is highly responsive, fast, and practical. Support is always quick, helpful, and focused—making it a pleasure to work with them," Steve shares.


For bitHuman, Cerebrium has become more than infrastructure—it's an enabler of their vision. By removing technical barriers and dramatically improving performance, Cerebrium allows bitHuman to focus on what they do best: creating digital characters that feel remarkably human.


### Looking Forward


With infrastructure that can scale from zero to thousands of concurrent sessions in seconds, bitHuman is ready for whatever comes next—whether it's a massive tradeshow deployment or gradual organic growth. The combination of 10x cost efficiency, sub-3-second response times, and rock-solid reliability positions them to bring their vision of scaling human presence to reality.


As Steve concludes: "Keep up the good work! You guys are amazing!"


For companies building the future of AI interactions, bitHuman's experience with Cerebrium offers a clear lesson: the right infrastructure partner doesn't just solve today's problems—it unlocks tomorrow's possibilities.
