---
schema_version: "1.0.0"
document_id: "b99e59fbe28eb1be06f90fd39a779e19bf3e672e1ad2faf0306ae0141a7931ef"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/certificate-management"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:f92fcff70a1744d0316c7825872918d9f9808e0f316eb4ac00afe556d4178aa9"
---

# Certificate Management: The Complete Guide to Automating PKI and TLS/SSL Certificates Across The Lifecycle

Most of the time, seeing “Certificate expired” in your terminal might ruin your weekend plans, but usually not the company. Other times (well, one time in 2018), it causes[mobile coverage outages](https://www.theverge.com/2018/12/7/18130323/ericsson-software-certificate-o2-softbank-uk-japan-smartphone-4g-network-outage) across 11 countries and dozens of millions of people, a settlement of up to £100M, and a massive loss of trust.


This is what happened to Swedish telecommunications equipment company Ericsson. Someone forgot to renew the certificate for a node that acted as a gateway to services and controlled the LTE network. When it expired, it triggered a standard industry practice which deactivates any device in the network to prevent attackers from adding malicious devices to the network. And so LTE coverage went down across 11 countries and many telco companies.


The root cause was simple: Ericsson didn’t have software to discover its certificates and their expiry dates. Avoiding catastrophic fallout would’ve been as simple as a terminal command to renew the certificate.


This may be an avoidable mistake, but more common than you think. Many organizations don’t know the full extent of their certificates and when they expire.


Ericsson isn’t the only example.[MS Teams went down](https://techcrunch.com/2020/02/03/microsoft-teams-has-been-down-this-morning/) because of an expired SSL certificate,[Spotify had an outage](https://medium.com/@warstories/the-spotify-certificate-outage-every-engineer-should-learn-from-c4ec261fc356) , and[Alaska Airlines experienced flight delays](https://www.reuters.com/business/aerospace-defense/alaska-airlines-hit-by-it-outage-causing-ground-stop-seattle-2024-09-23/) . These are just outages from companies large enough for the media to cover them. But even at less prominent companies, certificate expiries are constantly taking down customer-facing services and internal infrastructure, harming customer trust or causing engineering to grind to a standstill.


This is only going to get worse: TLS certificate maximum lifetimes dropped to 398 days in 2020, and the CA/Browser Forum has approved a roadmap that takes them to 47 days by 2029. Certificates will require renewals more frequently than ever, which will put more strain on anyone managing certificates. Manual certificate management will become virtually impossible.


Certificate management is the practice of tracking, issuing, renewing, and revoking digital certificates across an organization's infrastructure.


Like most other security practices, modern certificate management is getting more complex with modern architecture, shortening lifecycles, and more. There are a lot of dedicated tools, each with their pros and cons. But let’s start with the basics.


[video](https://www.youtube.com/watch?v=jP-9Ak0tr8A)


## Why do certificates exist?


Certificates (also called certs) are fundamental to the internet because they authenticate identities online. Every single HTTPS connection is secured by certificates because it tells browsers, terminals, and, increasingly, AI Agents that the website they’re visiting is the real deal. If it wasn’t for certificates, a shady coffeeshop could make its WiFi router resolve DNS requests to “google.com” to any IP address they want to, including a spoofed copy that siphons off any credentials, while the user still believes they’re on Google.com.


Because certificates also encrypt connections, an internet without certificates would mean any ISP, shady coffee shop wifi operator, or anyone else along the infrastructure chain could intercept anything anyone does online. That would include passwords and credit card data in plaintext, any messages ever sent, and literally any other data sent from or to your device.


So without these tiny documents, the entire internet would cease to function as it does today.


## What is a digital certificate?


Most engineers mainly know certificates from the inconveniences they cause when a staging database throws an error. They can rattle off some version of these basics: certificates are data records that prove server/device identities, assure encryption, and protect data in transit.


Before diving into the details of managing certificates, it’s worth understanding what certificates are to understand the constraints we’re working with.


Certificates usually contain a few pieces of data:


### Identity


Each certificate is issued to an identity. This information is necessary because it makes certificates non-transferable. If Google holds the certificate for google.com, it can’t use that same certificate to authenticate itself as instagram.com.


- The subject. The subject is the recipient of the cert, typically a domain name, company name, etc. Browsers now directly check subject alternative names, but the subject still exists for possible dependencies and internal use cases.
- The subject alternative names: This is what browsers actually check. These are IP address(es), domain(s), etc. that the certificate is valid for.


### Issuer information


Each cert comes from a Certificate Authority (CA), the issuing party. These are necessary because browsers only trust a given set of CAs, the same way bars only accept government IDs and reject a Panda Express loyalty card, even if it states a birth year in the 19th Century.


This is important because anyone can create a CA (and most companies have internal CAs nobody would trust outside their own infrastructure). Most browsers have a list of CAs they trust by default. Almost all public-facing websites hold a certificate of one of these. The certificate contains two details:


- The issuer itself. This contains the CA that signed it, typically one of the big CAs browsers trust, like Let’s Encrypt.
- The CA Chain. CAs spin off intermediate authorities that issue their own certificates backed by the trust in the root CA. The certificate records the full chain.


### Validity


Certs have strict validity windows to ensure frequent rotation and to keep abandoned/expired domains from being hijacked by threat actors. They record exactly:


- Not valid before: When the certificate becomes valid.
- Not after: When the certificate expires.


### The public key


Certificates fundamentally work because of cryptography. Without getting too technical about it, the public key is a string of text that proves the certificate’s authenticity. The certificate contains:


- The public key (the certificate’s identifier)
- The algorithm used for the encryption (RSA 2048-bit, RSA 4096-bit, ECDSA P-256 etc.)


*Note: In contrast to the public key, each cert has a private key. The private key lets anyone impersonate the certificate’s subject names and decrypt its traffic. The private key should always remain secret.*


### The fingerprint


A fingerprint is a hash of all of the certificate’s contents, which acts as a unique identifier of a specific cert. It’s a long string of text contained in the certificate.


### Extensions


Some certificates contain additional information to specify permissions or serve specific use cases.


Some examples here:


- Key usage: what the cert is allowed to do.` Digital Signature` ,` Key Encipherment` , etc. A CA cert has` Certificate Sign` , but a regular certificate does not. A subcategory here is extended key usage, which enables things like` TLS Web Server Authentication` or` TLS Web Client Authentication`
- Basic constraints: whether this cert can act as a CA itself.
- CRL distribution points: where to check if the cert has been revoked
- OCSP: the URL for real-time revocation checking


### The signature


Finally, each certificate contains a digital signature that verifies all of the above content. The CA is saying: “I verified the authenticity of this entity and its control of this domain/device.” It contains:


- The signature itself
- The cryptographic algorithm used for the signature


Finally the certificate is stored as a` .pem` file, which encodes all of this information in base64.


This is the global standard format called[X.509](https://www.ssl.com/faqs/what-is-an-x-509-certificate/) . Each certificate binds a subject's identity to a public key (which is why certificate management is also called public key infrastructure). These are never manual workflows. There are plenty of dedicated[certificate management tools](https://infisical.com/blog/best-certificate-management-tools) (including Infisical) as well as CLI tools that automate the workflow.


The most intuitive type of certificate is the one that authenticates public websites. But there are different types of certificates.


### Public versus private certificates


Public certificates authenticate your website, services, or devices to anyone accessing them on the open internet. These are issued by globally trusted CAs like DigiCert, Let's Encrypt, and Sectigo. Their root certificates are embedded in operating systems and browsers.


Private certificates are minted by internal CAs from your organization. They don’t need to be trusted by random browsers because their only users will be inside your organization. Examples are connections between microservices, dev/staging databases, or a smart lock on your office door.


Internal CAs have capabilities public ones don’t. They can certify services with no public hostname, set custom validity periods, and enforce organization-specific policies around issuance and approval.


Most engineering organizations eventually need both. Public certificates authenticate end user-facing services while private certificates handle internal infrastructure. This covers what’s called the Chain of Trust.


### The Chain of Trust


Certificate authorities don't typically issue certificates directly from a single root, but use a hierarchy of CAs. A root CA sits at the top and one or more intermediate CAs sit beneath it, which issue the so-called leaf certificates.


This hierarchy allows for more robust security architecture. The root CA has the biggest fallout if compromised, so its private key can be kept offline or in a[hardware security module](https://infisical.com/docs/self-hosting/guides/production-hardening#hardware-security-module-hsm-integration) .


Intermediate CAs are certified by the root CA to sign certificates to services and devices. This limits the fallout in case of a breach and simplifies incident response.


Say an intermediate CA’s signing key is compromised. That also compromises every certificate it issued. If one intermediate CA signs certificates for` development` ,` staging` , and` production` , that one incident requires revoking the CA and reissuing every certificate, in all three environments. This would result in outages for customers and engineering grinding to a halt. With a CA for each environment, the same incident would only affect reissuing certificates in one environment.


In practice, companies often create intermediate CAs for each geography, each environment, or each type of service (e.g. TLS vs. code signing vs. client auth, etc.). As organizations grow, the complexity typically increases.


This hierarchy affects how certificates are validated, how revocation is handled, and how the management tooling needs to be structured.


## Why certificate management matters


Certificates are the type of infrastructure where “no news is good news.” It’s also the type of infrastructure that doesn’t stay quiet if something is wrong. The consequences of expired certificates are immediate. Good certificate management prevents outages, strengthens your security posture, and is sometimes necessary for compliance.


### The operational cost of expired certificates


Certificate expiration is one of the most common causes of avoidable outages. Suddenly, browsers refuse to connect, API clients throw errors, and users see errors. This is frustrating because each certificate has a clear expiration date.


[Microsoft Teams went down](https://www.theverge.com/2020/2/3/21120248/microsoft-teams-down-outage-certificate-issue-status) for millions of users in February 2020 because an authentication certificate expired without being renewed. This happens because of faulty certificate management, specifically in certificate tracking. As infrastructure scales, blind spots are inevitable without systematic management because services and environments proliferate. This is why certificate expiries keep happening at large enterprises, often caused by the massive amount of certificates they have.


These outages typically happen when a certificate was issued and deployed, with the expiration date noted *somewhere.* But if that *somewhere* is a manual tracking method like a spreadsheet, a calendar reminder, or a crude internal dashboard, it eventually fails. The certificate expires, services go down, and someone gets a meeting invite from HR.


Most day-to-day outages can be fixed by renewing a certificate in a minute or two, but examples like Ericsson’s show that results can also be catastrophic, especially when certificate management is manual and you need to scramble to find the expired certificate. Until then, the piece of infrastructure is treated as untrustworthy because it’s a security risk.


### Why certificate management matters for security


Expired certificates often create operational hiccups, but unmanaged certificates can also cause or worsen security issues.


The[2017 Equifax breach](https://en.wikipedia.org/wiki/2017_Equifax_data_breach) compromised personal data (social security numbers, addresses, driver’s licenses) of about 147 million people. Certificate management wasn’t the attack vector, but kept the company from detecting the attack. Equifax ran SSL/TLS inspection that should have monitored the network and sent alerts. When the inspector’s certificate expired, it stopped inspecting traffic. The absence of alerts created a false sense of security. The attackers retained access for 76 days.


Equifax is the most severe example of an underlying principle: not systematically tracking certificates creates unmonitored certificates, which can create security weaknesses attackers may exploit. Some examples include:


- **Rogue issuance:** A compromised (internal) CA lets attackers issue certificates for malicious infrastructure to impersonate legitimate infrastructure.
- **Weak cryptography:** Old certificates may still use outdated encryption, which makes them forgable.
- **Insecure practices:** If internal certificate management is painful and some failure is normalized, engineers could route around it with` NODE_TLS_REJECT_UNAUTHORIZED=0` or` curl -k.`
- **Difficult incident response:** Without centralized management, you probably don’t know about all of your certificates, making it harder to quickly revoke compromised certificates.
- **Increased blast radius:** Badly-scoped certificates and CAs actively expand the blast radius of any given breach.


Good certificate management means knowing what certificates exist, who issued them, where they're deployed, and whether they should still be trusted. This means always knowing what certificates you have, when they expire, and who issued them.


### Compliance requirements


Several major compliance frameworks have explicit requirements around certificate governance.


- PCI DSS v4 requires organizations to maintain a managed inventory of trusted certificates and to use strong cryptographic protocols with properly managed keys and certificates.
- ISO 27001's requires a framework for issuing public key certificates.
- SOC 2's common criteria for logical access include controls over cryptographic materials.


Compliance frameworks and regulations demand certificate management because it’s an important instrument for audits and accountability. Centralized certificate management enables companies to show an auditor the certificate inventory, the renewal process, and the evidence that certificates were rotated on schedule. Managing certificates better means simplifying compliance.


In practice, certificate management has a few core components every workflow needs to cover.


## How certificate management works


Certificate management spans the full certificate lifecycle from request to expiry or revocation. Each certificate usually lives through six stages, though specific mechanics vary by environment and tooling.


- **Discovery** is where most organizations start. Before you can manage certificates, you need to know where they are. Discovery involves scanning your infrastructure (servers, load balancers, Kubernetes clusters, internal services) to build a certificate inventory. This frequently surfaces forgotten or expired certificates and unites all certificates in one place.
- **Issuance** is the process of requesting and obtaining a certificate from a CA. For public certificates, this starts with generating a key pair and a Certificate Signing Request (CSR) to a public CA. The flow is the same for internal CAs, but without the external CA. Modern tooling like Infisical can[automate issuance entirely](https://infisical.com/docs/documentation/platform/pki/quick-starts/issue-first-certificate#issue-your-first-certificate) , particularly for internal certificates.
- **Deployment** means getting the issued certificate onto the infrastructure it needs to certify. Deployment creates much of the operational complexity because the target systems often have different mechanisms for consuming certificates and different requirements for certificate formats.
- **Monitoring** is the ongoing work of tracking deployed certificates: when they expire, whether they're correctly configured, and whether the Chain of Trust is still intact. Good monitoring surfaces problems weeks before a certificate expires to give you time to renew.
- **Renewal** replaces a certificate to avoid expiries and outages. Certificate management tooling can automatically detect an upcoming expiration, request a new certificate, and deploy it. Since[certificate lifetimes are shrinking](https://cabforum.org/working-groups/server/baseline-requirements/requirements/) , renewals will become more frequent. Relying on human-triggered renewals will become more difficult.
- **Revocation** invalidates a certificate, typically because it was compromised or issued incorrectly. Revocation is handled through Certificate Revocation Lists (CRLs) or the Online Certificate Status Protocol (OCSP), which allow clients to check whether a certificate is still valid. Managing revocation requires knowing where a certificate is deployed so replacement can happen alongside revocation.


In many organizations, these six stages are not handled by a single tool or, worse, not handled by any tool. Robust certificate management workflows unite the full workflow in one certificate manager and automate as much as possible. This allows fast incident response because revocation and alerting live in the same place, avoids expiry-driven outages by automating renewal, and avoids unmanaged certificates by regularly discovering certificates.


To more deeply understand the *why* of many of these practices, it’s worth understanding different types of certificates.


## SSL/TLS and mTLS certificates


The most common type of certificate engineers work with is the TLS certificate, which secures HTTP traffic between clients and servers. When a browser connects to a website over HTTPS, it's a TLS certificate verifies the server's identity and establishes an encrypted session. The same mechanism applies to API clients, mobile applications, and any other client communicating with a web endpoint.


TLS certificates come in three validation levels:


1. Domain Validation (DV) certificates perform a quick, automated check that the applicant controls the domain. Let’s Encrypt offers these certificates for free.
2. Organization Validation (OV) certificates verify the organization behind the domain. An example of this is that Google’s erstwhile URL shortener goo.gl also has a certificate attached to Google, even though the domain is not google.com.
3. Extended Validation (EV) certificates are only issued to high-profile entities who frequently get impersonated. Major banks tend to have EV certificates to provide extra security.


For most engineering purposes, DV certificates are sufficient to establish secure connections and signal trust to browsers. OV and EV certificates are most common in financial services and other sectors where risk is especially high.


A subtype of TLS certificates is a wildcard certificate. Wildcard certificates cover a domain and all of its immediate subdomains with a single certificate. A wildcard for` *.example.com` is valid for` api.example.com` ,` auth.example.com` , and so on, but not for` sub.api.example.com` . Wildcards simplify certificate management, but also mean that one expired certificate can take down your app and API, when it could merely affect the website.


### mTLS


Mutual TLS (mTLS) extends the standard TLS handshake so that both client and server present certificates and verify each other's identity. This is the mechanism that underlies authentication for almost all non-human identities, including microservices architectures, zero-trust network models, and device authentication in IoT environments.


mTLS certificates are typically private certificates because the parties involved are internal services with no public-facing endpoints and no public CA would issue certificates for them.


All of the certificates, CAs, and tooling combine into what’s called Public Key Infrastructure (PKI).


## PKI and certificate authorities


PKI is effectively the law that governs certificate management in an organization. It determines the policies of how certificates are issued, how trust is established between parties, and how to handle compromised certificates. This directly drives the infrastructure decisions an organization makes about its PKI and how engineers manage certificates.


At its core sits the certificate authority hierarchy, which always starts with a root CA always sitting at the top. Its trust is derived from a self-signed certificate, which makes its private key the failure point of any PKI strategy. A compromised root CA key collapses the entire infrastructure across your entire organization. For public CAs, it can take out swaths of the entire internet.


As organizations grow, root key security typically increases:


- Small startups validating ideas or building proofs of concept usually have a software-based, self-signed root CA.
- Cloud-native companies often use cloud-based HSMs (the big cloud providers all offer it) that offer dedicated hardware security in big data centers.
- Larger enterprises typically secure root keys with on-premises hardware in facilities with strict access control policies.
- The big public CAs usually have air-gapped HSMs with formal “key ceremonies” (strict processes for anything that uses the key) along with third-party audits.


Intermediate CAs inherit the trust from the root CA and then issue leaf certificates to entities using them. Your chain of intermediate CAs dictate what your incident response will look like and the interruptions it may cause. A compromised intermediate CA that authenticates all environments creates outages for engineers *and* users. A more tightly scoped CA might only affect` staging` , which avoids user-facing outages.


The principles of PKI are always the same: You define your Chain of Trust and how to maintain that trust to ensure both uptime and security. In practice, there are many differences, depending on your infrastructure.


## Certificate management by environment


Certificate management differs based on your architecture and use cases. An IoT device is expected to function for years while a Kubernetes pod may only live a few hours.


### AWS Certificate Manager (ACM)


For teams already operating on AWS, Certificate Manager handles TLS certificates for AWS-native services like


- Application Load Balancers
- CloudFront distributions
- API Gateway
- Others


ACM offers a convenient path for public certificates on these services. It issues certificates from a public CA, renews them automatically, and deploys them directly to supported services.


ACM is more limited in private certificate issuance: ACM Private CA is a separate service that carries its own pricing (roughly $400 per month per CA), and ACM certificates generally can't be exported for use outside AWS services.


AWS Certificate Manager works for AWS purists, but struggles as soon as infrastructure expands to hybrid or multi-cloud setups.


### Azure Key Vault


Azure Key Vault is Microsoft’s native[secrets manager](https://infisical.com/blog/best-secret-management-tools) . It bundles certificate management with secrets and key management. It can issue certificates from integrated CAs, store certificates centrally, and manage renewals through policy.


It integrates naturally with Azure App Service and other Azure infrastructure, but centralized management gets complicated as soon as you branch out from Microsoft’s services.


### GCP Certificate Manager


Google Cloud Certificate Manager is Google's dedicated certificate management service. It handles both Google-managed certificates (where Google handles issuance and renewal) and customer-managed certificates.


Like the AWS and Azure equivalents, it's well-integrated with GCP services and automates renewal for managed certificates, but is scoped to GCP services.


### Windows Certificate Manager and ADCS


On-premises and Windows-heavy environments often issue private certs with Active Directory Certificate Services (ADCS). This is Microsoft’s PKI solution, which ships as a role in Windows Server and Active Directory. As the name suggests, ADCS integrates with Active Directory for certificate issuance. It supports auto-enrollment for machines on the same domain, and handles certificate revocation through revocation lists published to Active Directory.


Most organizations using Windows Server are using ADCS, which works well enough until you try to run multi-cloud environments to certify infrastructure outside the Active Directory domain.


### Kubernetes


Certificate management in Kubernetes is complex for three reasons:


- The infrastructure is manifold: even mid-sized clusters have hundreds of pods
- Pods are ephemeral: their lifespan is often measured in hours.
- Kubernetes requires TLS handshakes for each microservice interaction, requiring additional certificates for a two-way handshake


Some teams use cert-manager, a Kubernetes-native controller that automates certificate issuance and renewal. It integrates with a range of issuers like Let's Encrypt via ACME, internal CAs, Infisical, and others. It stores certificates as Kubernetes Secrets and makes them available to pods and ingress controllers.


Kubernetes-native certificate management relieves teams of the manual processes around the certificate lifecycle, but creates the responsibility of getting configuration exactly right, as misconfigurations at scale can compound problems.


### IoT and device certificates


IoT certificate management is unique because it introduces a physical component. This creates new challenges:


- Small devices like Arduinos or Raspberry Pis have constrained resources that limit their operations or drain their batteries.
- Provisioning certificates at manufacturing time or first boot requires tooling that integrates with production workflows.
- Devices may not always have internet access, so standard revocation checking may stop working intermittently. Certificate management needs to balance security with practical concerns.


These constraints make IoT certificate management a specialized discipline within the broader field.


## How to automate certificate management


Manual processes don't scale, and trusting humans with these workflows inevitably causes errors, which leads to outages or weakens your security posture. As certificate lifetimes shorten, automation has gone from a nice-to-have to a baseline expectation.


Years ago, certificate management was an occasional chore, not an ongoing workflow. Public certificates lasted for years and private certificates could be issued for decades. But the CA/Browser Forum (which dictates how long public CAs can be valid) has decided that until 2029, the maximum lifetime of public certificates will shorten to 47 days. Private certificates are not subject to these rules, but PKI is still getting more difficult as infrastructure proliferates and security policies demand more frequent rotations.


Certificate automation is an established field with a few existing solutions.


### The ACME protocol


ACME (Automated Certificate Management Environment) enables fully automated issuance and renewal. Originally developed for public CA Let's Encrypt, ACME standardizes how a client can prove domain control to a CA and receive a signed certificate without a human in the loop.


Almost all certificate management platforms (including Infisical) implement the ACME protocol to enable zero-touch certificate management for any ACME-compatible client. Organizations willing to configure ACME, the operational burden of certificate renewal can be reduced to essentially zero.


### What automation looks like in practice


An automated certificate management workflow roughly follows this pattern:


- A certificate is issued and deployed to the target system
- A monitoring component tracks the certificate's expiration
- At a configurable threshold before expiry (often 30 days), a renewal job triggers automatically
- The new certificate is issued, validated, and deployed
- The old certificate is replaced without downtime.


If any step fails, an alert fires before the certificate expires.


Automation requires certificate discovery. Many companies don’t know all of their certificates. The certificates that were manually issued, deployed to a forgotten server, or issued for outdated projects can’t be managed if they’re not recorded anywhere. Certificate discovery is a necessary starting point for any organization.


### SCEP and EST


SCEP and EST are legacy certificate management protocols still in use at many organizations. SCEP (Simple Certificate Enrollment Protocol) was developed by Cisco and is prevalent in network devices, and mobile device management platforms. EST (Enrollment over Secure Transport) is SCEP's successor. It’s more secure and flexible.


Although SCEP and EST were invented in 1999 and 2013, respectively, established companies still frequently use the protocol with legacy infrastructure.


Migrating the way you automate certificates is painful and sometimes impossible. Many organizations opt for Certificate Management tools like Infisical that support ACME, SCEP, EST, and every other common method to ensure a single, centralized PKI system without workarounds.


## Choosing the right certificate management tool


Certificate management tools range from the cloud-native certificate stores bundled with AWS, Azure, and GCP to standalone platforms that cover the full lifecycle anywhere. The right choice depends on the scope of your problem.


First, you need to understand whether a tool works the types of certificates you need:


- Does it support both public CAs for external certificates and private CAs for internal infrastructure?
- Does it support building and managing a private CA hierarchy?
- Does it support the infrastructure you use?
- Can it grow with your needs, i.e. does it support integrations or infrastructure you might *eventually* need?


Lifecycle coverage is another consideration. Some tools do certificate discovery, while others only manage renewal, but don’t help you scan for certificates. The ideal solution covers the full lifecycle from discovery through revocation across all your environments. Operating a patchwork of specialized tools reintroduces potential blind spots and manual processes.


Many companies also have specialized deployment models. Some organizations in regulated industries or with data residency requirements require self-hosting. Others need to manage PKI for legacy infrastructure like on-premises services alongside Kubernetes-based cloud services. The ideal certificate management solution supports whatever model you need.


Options in the space include:


- **Infisical:** Supports the full certificate lifecycle and automates certificate automation via SCEP, ACME, EST, and API. It supports CA hierarchies, integrates with hardware security modules, and can operate direct syncs with the cloud providers’ native solutions.
- **Cloud provider solutions:** Azure Key Vault, AWS Certificate Manager, and GCP Certificate Manager are effective as long as your PKI stays simple and you don’t plan to expand beyond one infrastructure stack.
- **Venafi:** Certificate lifecycle management (CLM) with broad CA integrations, aimed at organizations standardizing certificate governance.
- **HashiCorp Vault PKI:** Vault's PKI engine can run a private CA and issue certificates on demand. It's a building block rather than a finished product, so teams have to build homegrown issuance workflows, monitoring, and UI on top.
- **Keyfactor:** PKI-as-a-service that includes certificate lifecycle management, covers private CA operation and certificate automation.


## Certificate management with Infisical


Infisical is an identity security platform that automates the full certificate lifecycle on any infrastructure:


- Provisioning a CA hierarchy with root and intermediate CAs to issuing X.509 certificates
- Discovering certificates to build an authoritative inventory across your infrastructure.
- Automating renewal through ACME, SCEP, and EST
- Integrates with HSMs to offer best-in-class root key security


For teams starting to improve their certificate management, Infisical is available as a cloud-hosted service or fully self-hosted on their own infrastructure.


[Explore Infisical Certificate Management](https://infisical.com/docs/documentation/platform/pki/overview) docs,[try it for free](https://app.infisical.com/signup) , or[book a demo](https://infisical.com/talk-to-us) to see how it fits your infrastructure.
