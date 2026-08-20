---
schema_version: "1.0.0"
document_id: "da296e311e6bdc4b3324889aa71e4d6bf1ce869f6f2c4348b1826856eedc9e9c"
company_key: "yc-formal"
company: "Formal"
source_id: "yc-formal-news-import-a5b088b50d89"
canonical_url: "https://www.formal.ai/blog/security-problem-to-solution/"
published_at: "2025-10-30T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:31.452672+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:bfdb0bf0106fca26d630e7e7e7c39fc12705feae1d97d12a357758c72acc7fdd"
---

# From Security Problem to Security Solution

We built what our customers needed: a security-first PHI protection system that could deploy on-premise and provide defense in depth. We deployed it and it integrated into their environment seamlessly. Then some interesting happened – other healthcare companies started asking about it.


## Why Healthcare Data Protection Needs a Security Mindset


At our core, we’re a security company. We think about threats, least privilege, and defense in depth. So when our customers came to us asking for PHI redaction and classification, we did what security companies do: we researched the threat landscape.


What we found was concerning. **Everyone is approaching this problem from the perspective of data handling when it’s actually best approached as a security problem.**


## Why Current Solutions Fall Short


We spent significant time evaluating industry solutions. The pattern was clear: every approach had critical failure modes.


**ML/AI-Only Solutions** have a fundamental problem: probabilistic outputs in a deterministic compliance world. ML models trained on clinical notes work great on more clinical notes. But production data is messy – support tickets, API logs, patient portal messages. Models don’t know what they don’t know, and when they fail, they fail silently. Furthermore, these models often lack the environmental context to inform classification. PHI just leaks, and you find out when you get the regulatory notice.


**Traditional NLP** is better at linguistic recognition but shares ML’s core weakness: it’s optimized for accuracy on its training set, not security. There’s no adversarial mindset – no resilience for when the system encounters data it wasn’t trained on.


**Regex patterns** are fast and deterministic – you know exactly what they catch. But they only catch what you explicitly program. Medical data is too varied for pattern matching alone, and one novel PHI format means you’re leaking data.


**The current landscape lacks security rigor:**


- **Traditional approaches** are like hardcoded firewall rules that can’t adapt
- **Industry giants** deploy modern, yet single-layer, solutions – the equivalent of perimeter-only security
- **AI-first startups** are running models in the cloud, introducing the aforementioned nondeterminism while simultaneously asking you to trust them with your most sensitive data


In security, we know better than to rely on a single control. So why would we do that with PHI?


## Defense in Depth: Applying Security Principles to PHI Redaction


We approached PHI protection the same way we approach network security – with layered defenses where each control compensates for the weaknesses of others.


**Layer 1: Pattern Recognition (Regex)** The fast path. Known PHI patterns like SSNs, MRNs, and phone numbers get caught immediately. Think of this as your signature-based detection – reliable for known threats.


**Layer 2: Linguistic Analysis (NLP)** Context-aware scanning that understands medical terminology and natural language. This catches PHI that appears in conversational text or clinical notes. Your behavioral analysis layer.


**Layer 3: AI Classification** The intelligent backstop. Our LLMs identify novel patterns, edge cases, sensitive words with typos, and contextually sensitive PHI that rule-based systems miss. This is your anomaly detection – catching zero-days in the PHI world.


Each layer operates independently. If one fails or produces a false negative, the others provide coverage. That’s defense in depth.


## On-Premise Architecture: Your Data Stays in Your Control


Yet another manner in which we differ from other “AI-powered” solutions is that **we deploy in your environment** . Whether that’s your on-premise infrastructure or your private cloud (AWS, Azure, GCP), the point is the same: your PHI never touches our infrastructure, never leaves your control, never crosses into third-party systems.


**Flow:**


- Client request → Connector evaluates against policy
- Allowed request → HTTPS resource
- Response → Intercepted by proxy
- Consult → AI satellite runs all three detection layers
- PHI classified in real-time
- Redacted response → Client


## **Results: Security Metrics That Matter**


When you approach this as a security problem, different metrics matter. We don’t just measure speed or overall accuracy – we measure what goes wrong, and then ensure it doesn’t.


Our hybrid approach delivers:


- Real-time inspection with minimal latency overhead
- Significant reduction in false positives compared to single-layer solutions
- Zero external data transmission – all processing in your environment
- Complete process visibility
- The ability to selectively redact via defined policy
- Industry topping accuracy


## **From Security Problem to Security Solution**


We built what our customers needed: a security-first PHI protection system that could deploy on-premise and provide defense in depth. We deployed it for them, it worked, and then something interesting happened – other healthcare companies started asking about it.


Turns out every healthcare technology company building on-premise or in their own cloud environments faces the same challenges, and has the same security requirements we do.


We’re now protecting PHI for healthcare companies that take security as seriously as we do. Companies that understand compliance isn’t just about checking boxes – it’s about building systems that actually don’t leak data.


## **Security Mindset, Healthcare Context**


Sometimes the best solutions come from applying reasoning approaches from one domain to problems in another. We brought security principles to healthcare data protection, and it turns out that’s exactly what the industry needed.


PHI isn’t just data – it’s data you absolutely cannot afford to leak. Protecting data you cannot afford to leak – that’s what security companies do.


You can just secure things.
