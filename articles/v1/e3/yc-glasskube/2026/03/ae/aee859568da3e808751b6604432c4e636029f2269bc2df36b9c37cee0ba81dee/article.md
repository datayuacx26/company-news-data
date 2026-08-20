---
schema_version: "1.0.0"
document_id: "aee859568da3e808751b6604432c4e636029f2269bc2df36b9c37cee0ba81dee"
company_key: "yc-glasskube"
company: "Glasskube"
source_id: "yc-glasskube-news-import-f40b83a58804"
canonical_url: "https://distr.sh/blog/license-key/"
published_at: "2026-03-12T00:00:00+00:00"
first_seen_at: "2026-07-21T21:44:16.826534+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:f57316f7ea708f1f37bfd7d6fed9cd870893b0639e7630ffb633b26dd71df243"
---

# Usage Limits, Seat Counts and Feature Gates for Self-Hosted Software

If your software runs in the cloud and you control the infrastructure, usage enforcement is a solved problem. Seat limit hit? Block the next login. Trial expired? Gate the UI. Feature not in the plan? Don’t render the button. You own the environment, so you own the controls.


Now try doing the same thing when your customer runs the software.


## The self-hosted gap


Vendors shipping to on-prem, self-hosted, or air-gapped customers run into a wall fast. Your customer controls the environment, and often the application itself. You can’t gate a UI you can’t reach. You can’t block a login against a server you’re not running. And “call home to validate the license” isn’t an option if the customer’s cluster has no outbound traffic.


So what actually happens? Most vendors either skip enforcement entirely in self-hosted environments, or ship something that relies on the customer playing by the rules. Neither is a real answer when seat-based pricing or feature tiers are a meaningful part of your business.


## How Distr handles it


We built licensing into Distr with two tools that cover different parts of the problem.


**Entitlements** control access at the distribution layer: which application versions a customer can deploy, which container images they can pull from your registry. If a customer is on your base tier, they only see what you’ve granted them. No enforcement logic needed on their side.


**License Keys** handle runtime enforcement inside your application, even when nothing external is reachable.


You define a JSON payload with whatever your application needs to enforce:


```text
{  "seats"  :   25  ,   "plan"  :   "  pro  "  ,   "modules"  : [  "  analytics  "  ,   "  reporting  "  ]}
```


Distr issues a signed JWT. Your customer injects it into their environment: an env var, a Kubernetes secret, a mounted config file. Your application verifies the signature at startup using your public key and reads the claims. No network call. No license server. No dependency on Distr being reachable at runtime.


It works fully offline. It works in air-gapped clusters. It is cryptographically secure.


Your application reads` seats: 25` and enforces it. Contract expires? Issue a new token with a new expiry. Customer upgrades to a higher tier? New payload, new token. Rotation is on your side. The customer just injects the updated value.


## Getting started


License management is available on Pro and Enterprise plans:


- [License Management Overview](https://distr.sh/docs/platform/license-management/)
- [Application Entitlements Guide](https://distr.sh/docs/platform/application-entitlements/)
- [Artifact Entitlements Guide](https://distr.sh/docs/platform/artifact-entitlements/)
- [License Keys Guide](https://distr.sh/docs/platform/license-keys/)


---


Questions or want to see it in action?[Join the discussion](https://github.com/distr-sh/distr/discussions) or[book a demo](https://cal.glasskube.com/team/gk/demo?duration=30) .
