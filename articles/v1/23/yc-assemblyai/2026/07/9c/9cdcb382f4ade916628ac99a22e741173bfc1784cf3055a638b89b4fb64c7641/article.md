---
schema_version: "1.0.0"
document_id: "9cdcb382f4ade916628ac99a22e741173bfc1784cf3055a638b89b4fb64c7641"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/expanding-enterprise-security-and-data-residency-capabilities"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:5ba1300b0c721f28d2495359fab5c151254ce62e6ba48d30defbf83329894b53"
---

# Expanding Enterprise Security and Data Residency Capabilities

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


As more enterprise organizations choose AssemblyAI as their preferred speech-to-text and speech understanding provider, we're continuing to advance our platform to meet global operations' security and compliance requirements. Today, we are announcing significant enhancements to our security infrastructure and data residency capabilities, including EU Data Residency, ISO 27001 Certification, and renewed SOC 2 Type 2 Coverage. These updates strengthen our commitment to providing secure, compliant, and globally accessible AI solutions.


## EU Data Residency: Enhanced Control and Compliance


As we move into 2025, we're strengthening our enterprise foundation with enhanced security measures that match the scale and complexity of our customers' operations.


To support our customers and complement security certifications, today, we are introducing self-serve EU data residency capabilities. This new functionality addresses the growing demand for data sovereignty and regional compliance requirements.


Our new capabilities deliver:


- Instant access to EU-based processing through simple API parameters
- Guaranteed EU data sovereignty
- Location-optimized performance
- Detailed EU usage and cost monitoring tools


## Security That Drives Growth


As organizations scale their AI operations globally, security and compliance requirements shouldn't slow down expansion. Our security advancements remove these barriers, empowering your organization to:


- Scale operations confidently across new markets with pre-verified security standards
- Deploy AI solutions rapidly in regulated industries and EU markets
- Focus on innovation and growth while we handle security and compliance requirements
- Build lasting customer trust with enterprise-grade security


## Availability and Implementation


These security enhancements and EU data residency features are now available to all customers. Organizations can access these capabilities via the[EU endpoint](https://www.assemblyai.com/docs/speech-to-text/speech-recognition#select-the-region) , and monitor their EU usage[via the usage dashboard.](https://www.assemblyai.com/dashboard/login)


Here is how to use the EU endpoint with[AssemblyAI's python SDK](https://github.com/AssemblyAI/cookbook/blob/master/core-transcription/how_to_use_the_eu_endpoint.ipynb) .


```text
import   assemblyai   as   aai


aai.settings.base_url =   "https://api.eu.assemblyai.com"
aai.settings.api_key =   "YOUR_API_KEY"


transcript = aai.Transcriber().transcribe(  "https://assembly.ai/sports_injuries.mp3"  )


print  (transcript.text)
```


Data processing can be effortlessly managed across different regions with a simple one-line code switch, giving you complete control over where your data is processed.


- The default region is US, with base URL` api.assemblyai.com` .
- For EU data residency requirements, you can use our base URL for EU at` api.eu.assemblyai.com` .


For additional help getting started,[contact our support team](https://www.assemblyai.com/contact/support) for implementation assistance (available 24/7 for all customers).


Monitor your EU usage in the AssemblyAI dashboard.


Start Building with AssemblyAI


Sign up now to access AssemblyAI's powerful Speech-to-Text API and create accurate, custom subtitle solutions..


[Sign Up For Free](https://www.assemblyai.com/dashboard/signup/?ref=assemblyai.com)


## Achieving ISO 27001 Certification: Advancing Global Security Standards


We have successfully achieved[ISO 27001 certification](https://app.vanta.com/assemblyai/trust/7n80syl8zln1bn1qm3x8eg/resources) , an internationally recognized benchmark for security excellence. This milestone validates our sophisticated approach to protecting enterprise data and managing information security risks through our comprehensive Information Security Management System (ISMS).


### Why ISO 27001 Matters


For organizations operating globally, ISO 27001 certification is more than a compliance checkbox—it's a crucial business enabler. Our certification delivers:


- Verified implementation of world-class security protocols
- Independent validation of our risk management framework
- Enhanced confidence for regulated industry clients
- Expanded access to international market opportunities
- Efficient security verification for procurement teams


Our ISO 27001-certified ISMS encompasses our entire infrastructure, including processes, technology, and people, ensuring comprehensive protection of organizational and customer data through effective risk management.


## Renewed and Expanded SOC 2 Type 2 Coverage


Building on our enterprise security foundation, we have renewed and expanded our SOC 2 Type 2 certification to include all Trust Service Criteria. This comprehensive coverage strengthens our security posture and provides additional assurance across security, availability, integrity and privacy controls—reflecting the maturity of our security program and readiness for complex enterprise deployments.


## Commitment to Continuous Security Enhancement


These new EU residency capabilities, ISO 27001 certification, and expanded SOC 2 coverage reflect our evolution as a trusted enterprise AI partner. As we continue to lead innovation in AI technology, we're matching that innovation with enterprise-grade security and compliance capabilities that enable global organizations to deploy our solutions confidently.


For comprehensive details about our security program or implementation guidance, please consult our[Trust Center](https://app.vanta.com/assemblyai/trust/7n80syl8zln1bn1qm3x8eg) or[connect with our sales team.](https://www.assemblyai.com/contact/sales)
