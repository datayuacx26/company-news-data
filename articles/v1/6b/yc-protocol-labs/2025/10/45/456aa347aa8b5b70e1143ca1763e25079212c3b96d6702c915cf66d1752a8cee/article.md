---
schema_version: "1.0.0"
document_id: "456aa347aa8b5b70e1143ca1763e25079212c3b96d6702c915cf66d1752a8cee"
company_key: "yc-protocol-labs"
company: "Protocol Labs"
source_id: "yc-protocol-labs-rss-95d08b63fd6d"
canonical_url: "https://protocol.ai/blog/attestation-follow-in-the-footsteps-of-tls/"
published_at: "2025-10-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:53.465065+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:cf3c82f276ec59a71f97c9e512dbc08c9266ddfdba96d6ff786d6d6e94414492"
---

# Can Attestation Follow in the Footsteps of TLS?

One of the more impactful changes to internet security in the last two decades has been the widespread normalization of TLS encryption for web browsing. The conscious push to create the free certificate authority through[Let's Encrypt (opens new window)](https://letsencrypt.org/) , coupled with the coalition-building of browser vendors that provided user visible motivation for servers to adopt and prefer the encryption mechanism, led to a real shift in behavior. It’s hard not to see the outcome: most of the web today benefits from increased integrity and confidentiality, among the most impactful, intentionally implemented changes to the security landscape.


In light of this success, it’s natural to think about whether there are other security mechanisms that could benefit from a similar concerted industry adoption. To understand this benefit, we can first look at what properties allowed the effective adoption of widespread TLS, and then we can use the mechanism of “trusted hardware” or remote attestation as an example of a technology that we could attempt to drive through similar means.


One of the most important structural features about the adoption of TLS was that user experience was limited to a small number of browser vendors (Google, Microsoft, Apple), which were amenable to improving security industrywide, not only because it was affecting end user perception and the reputation of their products, but also as a re-defining of the status quo. On the “supply side,” TLS and HTTPS certificates had been developed and were in use for a couple decades, but the going norm was that issuance of certificates required payment from a set of registrar companies set up for handling banking compliance requirements. This situation was seen as a negative by the browsers for a few reasons: they didn’t have a view or influence into the behavior of registrars to push for evolution or improvement of the certificate process.


A second structural component of TLS beyond an amenable chokepoint for adoption has been the support for an incremental division of labor. Developers can do a relatively straightforward upgrade, that then directly benefits their users. This combined with both lower costs through tooling, and through increased pressure through user visible warnings and reduction of capabilities on non-upgraded domains led to broad developer adoption over a relatively short period of time.


## # Towards broad attestation adoption


What would a broad industry push (modeled on Let’s Encrypt for TLS) look like to attempt to normalize the expectation of hardware attestation in online computation? This was one of the topics of conversation at the recent[Cypherpunk Retreat (opens new window)](https://www.protocol.ai/blog/threads-from-the-2025-cypherpunk-retreat/) .


There are probably three under-developed components that would go into such an effort:


(1) Exposing a user-visible signal indicating that hardware assurance is active;
(2) determining the standard for publisher ownership and certification of server computation; and
(3) tooling.


There’s a longer tail story around composition — where the immediate computation is certified, but not all of the remote services that are engaged with — that is probably best pushed towards a subsequent phase.


User-visible looks something like the lock icon shown in browser toolbars. In the case of attestation it is an indication of a similar property — that there’s assurance that the computation is what you think it should be. In the current landscape, this may be better encouraged through pushing on mobile app stores, where for instance only apps that meet the standard are able to run on devices with high security modes enabled, or where some advanced properties, like ease of access to biometric ID/passkeys, is limited to these cases without a warning.


The technical issue that seems most pressing to solve is how to represent the relationship between the developer, operator, and user. One of the simpler cases we might think about is that a user is attempting to interact with an instance of Nextcloud that has been set up by their organization and is running in AWS. We have a developer of Nextcloud, an operator of Amazon, and a clear user. When we look at where the expected hash of the memory image is generated/presented as the “correct” thing that should be attested though, we run into a blurry line between the upstream developer of the software Nextcloud, the organization that deployed it, or potentially the AWS marketplace integrator that configured and further packaged the software.


- Nextcloud could generate the expected memory image as part of the generation of their docker image packaging of their software.
- The organization could re-generate an expected image from the docker image they deploy, and include it in the DNS or TLS layers for the web service
- The integrator, if it re-packages the software with additional plugins may end up with a different image than upstream which it could then generate the expected attestation hash for.


Settling on a clear expectation of who is responsible is probably the most useful next step here. At the end of the day, integration likely does need to include provisioning of an additional DNS record or TLS field, which will fall with the organization, but they will likely be getting this value from one generated by either the software developer or integrator. We can imagine scenarios where the thing we would like to have delegated from the DNS record would be the valid releases of Nextcloud, as controlled by that upstream developer. This would spare the end deployer from managing dynamic DNS updates or worrying about changes during upgrades, though it would further constrain possible trust models.


Finally, as with the evolution of certbot, caddy, and other tools that integrate the automatic adoption of TLS, there is still a very immature set of tooling around attestation. It’s non-trivial within an application (either the web app on the server, or a mobile app on the client) to identify if the remote attestation is working properly, and then to warn the user or handle that case when it isn’t. Some of this can be handled by the constrained set of platform APIs, but there are integrations, along with tooling at the image generation, deployment, and application level that need additional support as well.


With these points, we have established that there is a plausible path and a similar set of amenable choke points in the space of hardware attestation as there has been for TLS adoption. While this is positive, it is also worth reflecting on who benefits and if the benefit is worth the effort to pursue.


## # Beneficiaries of Attestation


There are three primary roles that benefit from the adoption of attention:


- Hardware vendors, and those who approve them as being valid roots of trust
- Platforms, who get another tool for improved user security/to raise the bar against malware
- Users, who get some better guarantees against compromise


The biggest hindrance to clear expectations of value is the lack of clarity on what properties are provided by attestation. Hardware attestation provides, at a baseline, a higher expectation of integrity — that the code running is what we expect is running. This property can be coupled with other factors, like the reputation of the datacenter and operator where computation is running to increase confidence in the computation. Some implementations of attestation also couple it with systems designed to move towards confidentiality. These rely on hardware protections that make it more difficult for the operator, or for other programs on the same hardware, to introspect and see what the computation being attested is doing. In practice, this notion is much weaker. The space continues to see refinement as additional side-channel attacks are discovered, and additional defenses are added. The interesting note here is that this problem space is of low concern to the majority, and difficult to solve for the long tail. For most programs, placing computation in the datacenter of an operator with a high reputation is likely sufficient for their confidentiality needs; it’s been enough for most nation states to outsource to the cloud. The long tail of systems that want to operate in fully permissionless ways, where nodes can join with no reputation or identity, is the place where this is hardest, and also a place where confidentiality cannot be fully certain, as there will be an ongoing set of hardware attacks that can compromise that property at some cost. Being clear that the attestation system overall is a demonstration of integrity but not confidentiality will help make it more clear to developers and users alike what is being secured.


The final path of value to consider is that in many ways the use of hardware as a place of trust is a stop gap. We rely on it because it’s what we know how to do today. Ideally, the problem would be solved through encryption instead. Cryptography is catching up, but fully homomorphic encryption techniques for doing arbitrary computation over encrypted data remain slow today. One point of good news is that with minimal flexibility in presenting the types of attestation trust roots that is described here, the same end-to-end security system can be extended to also support remote computation over homomorphically encrypted data, as that becomes more prevalent. While that model is likely to require the user agent to provide additional support to ensure that data sent to the remote server remains encrypted, the signaling of a hash of the expected program or “image” will still be necessary and remain an essential part of the process.


## # Tl;DR


Hardware roots of trust and remote attestation are mildly good at improving computational integrity today. The interfaces will last into a future with even stronger guarantees, and it is this future potential that can motivate us to continue[building (opens new window)](https://datatracker.ietf.org/doc/draft-fossati-tls-attestation/) a movement and push for more attestation.
