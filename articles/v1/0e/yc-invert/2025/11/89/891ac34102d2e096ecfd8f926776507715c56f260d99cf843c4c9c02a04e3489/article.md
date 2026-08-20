---
schema_version: "1.0.0"
document_id: "891ac34102d2e096ecfd8f926776507715c56f260d99cf843c4c9c02a04e3489"
company_key: "yc-invert"
company: "Invert"
source_id: "yc-invert-news-import-968576ef11e8"
canonical_url: "https://invertbio.com/blog/engineer-blog-series-security-compliance-with-tiffany-huang"
published_at: "2025-11-07T00:00:00+00:00"
first_seen_at: "2026-07-24T00:25:27.297188+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:84c2833e1e842710b42692e84aa4e72956714e33430e82472ab09a8002fefcf0"
---

# Engineer Blog Series: Security & Compliance with Tiffany Huang

*Welcome to Invert's Engineering Blog Series, a behind-the-scenes look into the product and how it's built. For our third post, software engineering manager Tiffany Huang speaks about how trust and security is a foundational principle at Invert, and how we ensure that data is kept secure, private, and compliant with industry regulations.*


‍


## How does Invert build security into every stage of product development?


At Invert, security is part of our foundation. It's built right into our development lifecycle, not an afterthought. We start with risk assessments and we base development off of those risk-based approaches. We do peer reviews and have automated checks before any code is merged. We also follow strict security and compliance policies, such as data management and encryption standards, to ensure protection continues even after release.


‍


## In biopharma and biomanufacturing, compliance is critical. How does Invert ensure adherence to key regulations like FDA 21 CFR 411 and EU Annex 11?


We're very meticulous about compliance. For regulated data, every action, every change, every user interaction is fully traceable. It's timestamped and verifiable. We enforce role-based access controls and maintain detailed audit trails so electronic records are trustworthy.


Beyond traceability, we also ensure that the data is reliable, tamper-proof, and compliant with FDA regulations. We'll continue to validate that those controls are in place with both internal and external audits as well.


‍


## How does Invert maintain data integrity and auditability to meet those compliance standards?


We maintain what's essentially a tamper-proof logbook. Every record is timestamped, it has a user attributed to it, and it's stored immutably so nobody can go back in and change it. We have continuous monitoring so data integrity holds up for audits, and ensures that auditors are able to reconstruct every step of an event and be confident that no regulated data was changed or lost.


We also perform daily backups for our databases, along with restoration testing. All together, these measures ensure data is safe, resilient, and easily recoverable.


‍


## How does Invert strengthen its overall security posture through ongoing monitoring, staff training, incident readiness?


We continuously train employees at Invert, and we have regular training that keeps everyone sharp, from new hires to leadership. Our incident response tabletop exercises prepare us for anything that might happen, and we make sure we can act fast, minimize impact, and learn from any event that we do have.


‍


## How do you ensure Invert’s AI features align with emerging regulations like the EU AI Act or Annex 22?


We navigate AI governance by choosing the[National Institute of Standards and Technology AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) (NIST AI RMF) as our North Star, and align with the EU AI Act. Every AI feature goes through a structured risk assessment. We document everything thoroughly with human oversight before every release and keep that process transparent for customers. We also review practices regularly to make sure that we're always moving in the right direction as regulations evolve.


In addition, we never use customer data for training for model training without explicit agreement. Every feature is opt-in by default and transparently labeled when content is AI-generated.


‍


## How does Invert communicate and ensure transparency to build trust around data usage?


Our guiding principle is that our customers' data is their data.


As I mentioned, we never train AI models on customer data, unless there's a separate agreement that everyone's aware of. All AI features are opt-in by default, everything is transparent and fully documented—this is our trust contract with our customers. They know exactly when and how their data is being used.


We also have third party vendors that are important for us to provide contracted services and maintain performance. They must adhere to our same rigorous standards, and we review them every year. We expect the same security requirements and controls that we use, if not better.


‍


## What were some of the biggest challenges you faced in achieving security and compliance and of course how did you overcome them?


One of the biggest challenges was balancing the speed of development with the rigor of compliance and security. We were building the plane and keeping it safe at the same time—the key was to embed security directly into our workflow using risk-based approaches, some automated tools, and standardizing templates that could be used across all our AI features.


This way, we assessed the risk of each change upfront and had built-in security checks along the way. By doing that, we didn't have to sacrifice being secure to be fast.


‍


*Tiffany Huang is an engineering leader at Invert, where she drives AI governance, security, and compliance strategy—shaping how the company builds responsible, transparent, and trustworthy AI features. She helps teams innovate with confidence while keeping safety and integrity at the core of every product release*


‍
