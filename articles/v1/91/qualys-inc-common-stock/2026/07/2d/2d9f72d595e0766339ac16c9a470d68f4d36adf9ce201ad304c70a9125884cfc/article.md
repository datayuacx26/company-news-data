---
schema_version: "1.0.0"
document_id: "2d9f72d595e0766339ac16c9a470d68f4d36adf9ce201ad304c70a9125884cfc"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-d1030f25037f"
canonical_url: "https://blog.qualys.com/product-tech/2026/07/27/the-sub-10-minute-cloud-takeover-how-exposed-iam-keys-misconfiguration-and-ai-are-rewriting-the-rules-of-cloud-breaches"
published_at: "2026-07-27T15:00:00+00:00"
first_seen_at: "2026-07-27T15:00:36.155605+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:e383a8b71b16da0b06563b48bededb11a60308d87618c8d101b41d2f91170f4c"
---

# The Sub-10-Minute Cloud Takeover: How Exposed IAM Keys, Misconfiguration and AI Are Rewriting the Rules of Cloud Breaches

#### Table of Contents


- Two Real-World Attacks. One Emerging Pattern
- Cloud Services Were Designed to Accelerate Work. They Also Accelerate Authority
- The 10-Minute Crypto Mining Attack
- The 8-Minute AI-Assisted Attack
- What Modern Cloud Reconnaissance Looks Like
- Why AI Attacks Happen So Fast
- How Both Attacks Share the Same Systemic Failures
- Understanding Risk Before It Moves: From Exposure to Activity
- Conclusion
- Frequently Asked Questions (FAQs)


---


#### Key Takeaways


- Two real-world cloud attacks reached meaningful impact in less than ten minutes despite pursuing entirely different objectives.
- Both attackers treated the environment as a connected system, using existing permissions and relationships to expand their reach.
- Reconnaissance increasingly focuses on understanding access and capability rather than discovering vulnerable assets.
- AI is compressing the gap between discovery, decision-making, and execution for cloud attackers.
- The interval between initial access and operational impact is shrinking, placing greater emphasis on visibility before activity begins.


---


## **Two Real-World Attacks. One Emerging Pattern**


Cloud environments were built to make useful work easier. Increasingly, they are doing the same for attackers. Recent incidents illustrate this point. In one attack, compromised AWS credentials enabled attackers to deploy cryptocurrency mining infrastructure across EC2 and ECS resources within ten minutes of gaining access. In another attack, an exposed AWS access key discovered in a publicly accessible S3 bucket enabled privilege escalation, movement across nineteen AWS principals, unauthorized use of Amazon Bedrock models, and broader cloud resource abuse in less than eight minutes.


One attacker wanted compute. The other wanted AI. The objectives were different, but the timelines were not. Both progressed with remarkable efficiency once access had been established. The similarity suggests that the story is not about cryptocurrency or AI. It’s about the environment itself and how modern cloud systems translate access into capability.


## **Cloud Services Were Designed to Accelerate Work. They Also Accelerate Authority**


The cloud’s greatest strength has always been its ability to remove friction. Infrastructure can be provisioned in minutes rather than weeks. New applications can be deployed through APIs rather than procurement cycles. AI became consumable through APIs. Capabilities that once required weeks of coordination can now be accessed in seconds. Organizations benefit from speed because cloud platforms make capabilities easier to access.


That same efficiency applies to permissions. A modern cloud identity often serves as a gateway to dozens of interconnected services, workloads, repositories, and automation workflows. A single role can provision infrastructure, access sensitive data, invoke AI models, modify serverless functions, retrieve secrets, and establish additional trust relationships. Few objects inside a cloud environment carry comparable influence.


This characteristic does not make cloud platforms inherently less secure, but it does change how risk manifests. Operational authority accumulates over time through application integrations, business requirements, temporary exceptions, inherited permissions, and expanding cloud adoption. By the time an identity becomes compromised, it may already represent years of accumulated access decisions. The resulting exposure reflects the gradual concentration of authority across an increasingly interconnected environment.


## **The 10-Minute Crypto Mining Attack**


In early November 2025, Amazon observed a[crypto-mining campaign](https://thehackernews.com/2025/12/compromised-iam-credentials-power-large.html) targeting AWS environments using compromised IAM credentials. The attackers authenticated using valid credentials and immediately began evaluating the resources available to them. Shortly after gaining access, they used APIs such as GetServiceQuota to understand the operational limits of the environment before provisioning resources. The activity resembled planning more than exploitation. Resource enumeration revealed available infrastructure, while existing permissions exposed opportunities to provision and expand. Within ten minutes, mining workloads were operational.


The technical progression itself was not especially novel. IAM permissions enabled visibility into resources and quotas. EC2 and ECS provided compute capacity. Lambda-related permissions supported persistence. Each service contributed a specific capability. The speed of the attack emerged from how easily those capabilities could be reached through a single compromised identity.


The diagram below shows how Qualys visualizes an attack path, mapping the progression from an initial credential exposure through reconnaissance and resource enumeration to the point where critical assets are impacted. This visualization helps security teams understand how an attacker advances through the environment and where intervention is most effective.


Qualys CDR detects suspicious activity at the earliest stages of the attack path, providing the visibility needed to identify and investigate threats before they escalate. When malicious activity is detected, Qualys QFlow can automatically initiate response actions such as revoking compromised credentials and blocking access to affected resources. By disrupting the attack before it reaches its objective, Qualys enables organizations to move from reactive incident response to proactively breaking the attack chain.


Viewed individually, none of these services represents unusual risk. Organizations rely on them every day to operate cloud environments at scale. Viewed collectively, they reveal how operational authority is distributed across cloud-native services and how quickly it can be exercised once access is obtained. The incident offers a useful reminder that cloud attacks increasingly succeed not because individual services fail, but because the relationships between those services create pathways to broader control.


## **The 8-Minute AI-Assisted Attack**


A[second intrusion](https://www.darkreading.com/cloud-security/8-minute-access-ai-aws-environment-breach) compressed the timeline even further. An AWS access key exposed within a publicly readable S3 bucket provided an entry point into a cloud environment. Within eight minutes, access was followed by movement across 19 AWS principals, Lambda code manipulation, Secrets Manager enumeration, Bedrock abuse, and the suppression of model invocation logging. The sequence reveals how quickly attackers can expand visibility, access, and operational control once an initial foothold has been established.


### **How Authority Expanded**


The attack unfolded through a sequence of interconnected capabilities:


- The exposed credential revealed available roles and access paths
- Lambda permissions enabled additional control and privilege escalation
- Secrets Manager exposed further credentials and operational context
- Amazon Bedrock provided access to foundation model services
- Model invocation logging was disabled, reducing visibility into subsequent activity


Each step expanded the attacker’s understanding of the environment while simultaneously increasing the authority available to them. What began as a single exposed credential evolved into access across identities, services, and AI resources because each permission revealed additional opportunities for expansion.


The introduction of AI services adds another layer to that system. Foundation models become resources that can be consumed, manipulated, or monetized once access has been established. As organizations expand their adoption of generative AI, identity governance extends beyond infrastructure, workloads, and data into the capabilities of the models themselves. The credential provided access, the environment provided context, and the permissions provided momentum.


## **What Modern Cloud Reconnaissance Looks Like**


Cloud reconnaissance has evolved alongside cloud architecture. Traditional attackers focused on discovering hosts, ports, applications, and vulnerabilities. Modern cloud attackers often begin by understanding authority. The initial objective is not simply determining what exists. It is determining what can be reached, modified, provisioned, or consumed from the permissions already available.


Common reconnaissance activities include:


- Enumerating IAM users, roles, and policies
- Mapping trust relationships between identities and services
- Evaluating service quotas and operational limits
- Discovering secrets, tokens, and embedded credentials
- Identifying privilege escalation opportunities
- Tracing pathways between identities, workloads, and cloud services


The crypto mining campaign provides a useful example. Shortly after gaining access, attackers used APIs such as GetServiceQuota to understand the environment’s operational boundaries before provisioning resources. The activity resembled planning more than exploitation. Understanding available authority became a prerequisite for exercising it.


The same pattern appeared in the AI-focused intrusion. Permissions were evaluated, identities were traversed, and additional sources of access were uncovered before cloud resources and AI services were abused. The sequence highlights a reality that many organizations still underestimate. Attackers increasingly invest effort in understanding the structure of authority inside an environment because that structure often reveals the most efficient path to broader control.


Viewed through this lens, reconnaissance becomes less about asset discovery and more about relationship discovery. The most valuable information is often not a vulnerable resource, but an overlooked connection between identities, permissions, services, and trust relationships. Those connections frequently determine how far an attacker can move once access has been established.


### **AI Is Compressing Decision Cycles**


The role of AI in cloud security is often framed through code generation, phishing content, or malware development. Those capabilities attract attention because they are visible. A more consequential shift is occurring elsewhere. AI is reducing the time required to understand unfamiliar environments, evaluate options, and act on available information.


Historically, sophisticated cloud attacks required a combination of technical expertise and patience. Attackers needed to interpret IAM policies, understand service dependencies, identify escalation opportunities, and adapt when initial assumptions proved incorrect.


Much of that effort was spent making decisions. AI increasingly accelerates those decisions by helping attackers analyze permissions, navigate cloud architectures, generate scripts, and evaluate alternative paths in real time.


The result is a gradual reduction in decision latency. Activities that once required extensive manual investigation can increasingly be performed as part of a continuous workflow. The practical impact is not merely faster execution. It is a shorter interval between discovery, evaluation, and action.


### **AI Occupies Both Sides of the Attack Surface**


The AI-focused intrusion highlights a dynamic that security teams will encounter more frequently in the years ahead.


**AI as Weapon (Offense)** **AI as Target (Abuse)**


Autonomous recon across dozens of cloud services in seconds LLMjacking: unauthorized foundation model invocation


Real-time privilege escalation path generation from IAM analysis Inference API abuse for illicit content generation or resale


On-demand malicious code generation with iterative refinement GPU compute theft via high-performance EC2 instance hijacking


Adaptive decision-making when initial paths fail ML pipeline contamination via injected training scripts


Identity-hopping automation across 19 principals in 14 sessions AI service costs have been amplified to tens of thousands per month


Reduced attacker skill barrier – LLMs democratize attack expertise Logging is disabled to evade detection of model invocation abuse


LLMjacking represents a newer category of cloud abuse. Rather than stealing data, attackers consume AI capabilities themselves, invoking foundation models, generating content, training workloads, or reselling access at the victim’s expense. As AI services become integrated into cloud platforms, model access increasingly resembles access to any other high-value cloud resource.


In the Bedrock-related intrusion, attackers attempted to consume foundation model services after gaining access to the environment. Models, including Claude, DeepSeek, Llama, Nova, Titan, and Cohere, became resources to be leveraged rather than tools to be protected. The incident reflects a broader shift in cloud security.


As AI services become embedded in cloud platforms, they inherit the same security realities as infrastructure, storage, and compute. Access determines value. Visibility determines control.


## **Why AI Attacks Happen So Fast**


The defining characteristic of both incidents was the speed with which compromise became consequence. Cloud environments were designed to reduce friction between intent and action. As AI, automation, and cloud-native services continue to evolve, that same property increasingly shapes how attacks unfold.


## **How Both Attacks Share the Same Systemic Failures**


**Root Cause** **10-Min Crypto mining** **8-Min AI Attack**


**Exposed access keys** ✓ Compromised IAM credentials with admin-like privileges ✓ Keys exposed in public S3 bucket


**Excessive permissions** ✓ IAM user held over-privileged access beyond operational need ✓ Credentials unlocked EC2, Lambda, Bedrock, STS, and more


**Lack of identity monitoring** ✓ Anomalous API calls not detected in real-time ✓ Logging disabled; 19-principal pivot went undetected


**Cloud misconfigurations** ✓ No controls to detect credential anomalies at access time ✓ Publicly accessible S3 bucket exposing sensitive credentials


**No MFA enforcement** ✓ Single-factor authentication enabled credential abuse ✓ Absence of MFA allowed immediate access post-discovery


**Attacker automation** ✓ Scripted enumeration and mining deployment ✓ LLM-assisted real-time decision-making and code generation


Exposed credentials are continuously harvested from public repositories, storage buckets, infrastructure-as-code artifacts, CI/CD pipelines, and other internet-facing resources. Once discovered, automated tooling can chain reconnaissance, privilege escalation, lateral movement, and resource deployment with little human intervention. What once unfolded as a sequence of discrete actions increasingly operates as a continuous workflow.


AI further accelerates this process by lowering the effort required to interpret permissions, analyze cloud architectures, identify escalation opportunities, and adapt when an intended path fails. The result is not simply faster attacks. It’s a reduction in the time available for defenders to recognize and respond to malicious activity.


## **Understanding Risk Before It Moves: From Exposure to Activity**


The attack paths described in these incidents did not emerge at the moment of compromise. They already existed within permissions, trust relationships, exposed credentials, and service configurations. The attackers simply discovered and traversed them. The challenge for security teams is that these pathways are rarely visible when resources are evaluated one finding at a time.


[Cloud Security Posture Management (CSPM)](https://www.qualys.com/apps/cloud-security-posture-management) and[Cloud Infrastructure Entitlement Management (CIEM)](https://www.qualys.com/apps/cloud-infrastructure-entitlement-management) help make these pathways visible by revealing how identities, permissions, resources, and services interact across cloud environments. Rather than treating misconfigurations as isolated findings, they provide context into which combinations of exposures create meaningful risk.


Attackers rarely think in terms of misconfigurations. They think in terms of pathways. A credential leads to a role. A role reveals a secret. A secret unlocks a service. What appear as isolated findings during a security review can become a connected route through the environment once an attacker begins exploring how those relationships interact.


**Examples of exposures that warrant immediate attention include:**


- Publicly accessible storage resources containing credentials or sensitive data
- Long-lived access keys that remain active beyond operational need
- Overprivileged IAM users, roles, and service accounts
- Dormant identities that retain access to critical resources
- Excessive permissions associated with AI, secrets management, or serverless services
- Trust relationships that enable unintended lateral movement


Visibility into exposures alone is rarely sufficient. Both incidents moved rapidly from access to action, traversing identities, invoking services, and expanding their reach within minutes. Understanding that progression requires visibility into how workloads, identities, and cloud services behave once an attack is underway.


Qualys[Cloud Detection and Response (CDR)](https://www.qualys.com/apps/cloud-detection-response) complements posture management by providing runtime visibility into suspicious activity across cloud environments, helping security teams identify and investigate malicious behavior before attackers can establish deeper persistence or expand their operational footprint.


## **Conclusion**


The crypto mining campaign and the AI-focused intrusion reveal how quickly cloud environments can transform access into action. Both attacks relied on capabilities that already existed within the environment, progressing through identities, services, and trust relationships with remarkable efficiency.


As organizations expand their use of cloud-native and AI services, understanding how those capabilities connect becomes increasingly important. In environments where compromise can unfold in minutes, visibility into those connections may be as valuable as visibility into the assets themselves.


---


**Complete the 5-minute Cloud & Application Security Maturity Questionnaire to receive a personalized cloud security maturity analysis and prioritized roadmap. If you’d like to discuss your results or your cloud security strategy, **talk to a[Qualys Cloud Expert](https://www.qualys.com/forms/totalcloud-demo)** .**


[Take the Cloud Maturity Questionnaire](https://neuronleaders.typeform.com/qualystc2)


---


## Frequently Asked Questions (FAQs)


**What is an attack path in cloud security?**


An attack path is a sequence of identities, permissions, resources, and trust relationships that allows an attacker to move from initial access to broader control of a cloud environment.


**Why are cloud attacks becoming faster?**


Automation, cloud-native APIs, and AI-assisted decision-making have significantly reduced the time required for reconnaissance, privilege escalation, and lateral movement.


**What is LLMjacking?**


LLMjacking is the unauthorized use of cloud-hosted AI models and services, allowing attackers to consume inference capacity, generate content, or monetize AI resources at the victim’s expense.


**How do attackers gain access to cloud environments?**


Common entry points include exposed access keys, public storage buckets, hard-coded secrets, excessive permissions, and compromised identities.


**Why is IAM security so important?**


Identity and Access Management determines what users, workloads, and services can access. A compromised identity with excessive permissions can quickly become a gateway to broader cloud resources.


**How can organizations reduce cloud attack risk?**


By continuously monitoring cloud posture, enforcing least privilege, securing credentials, and identifying risky relationships between identities, services, and resources before they are exploited.
